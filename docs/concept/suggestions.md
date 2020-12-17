To understand [suggestions](../glossary.md#suggestion), you should have a basic understanding of a [document](../glossary.md#document) inside Searchperience.

Suggestions can be used to power a [search-as-you-type search](../glossary.md#livesearch) or as an auto-correction feature.

## Indexing

When indexing a document, Searchperience extracts suggestions from this document. 
That means suggestions are always generated out of the available documents, so there will never be an option that does not match a document.

* Depending on the document type, suggestions are generated from different fields. 
    * The conditions, if a suggestion should be generated, the fields itself and their processing are configurable and can be adapted depending on the use case and business expectations.
        * In most cases just the `title` field is used, e.g. for brands, locations and retailers.
        * Especially for products, fields like `variants.title`, `categoryPaths`, `variants.attributes.brandName`, among others are used.
    * It is also configurable which additional fields from the original document should be saved, e.g. `channel` and `locale`.  
* Suggestions are saved in [Elasticsearch](../glossary.md#elasticsearch) as its own document type. 
    * For indexing Elasticsearch uses differently configured analyzers to examine the fields. The most worth mentioning filters of these analyzers are
        * Stemming[^stemmer] 
        * Edge n-gramming[^ngramming]
    * While it is possible to know to which specific document type [`mimeType`](../glossary.md#mime-type) a suggestion corresponds, it is not possible to know the exact document from which the suggestion was extracted from.
    * A suggestion exists only once without reference to how often the term appears in the original documents. To ensure uniqueness, we create a hash from the suggestion `text` and the additional fields.

Example of a product suggestion document in Elasticsearch:

```json
{
  "mimeType": "application/json+sp-suggestion-product",
  "text": "Jaguar",
  "timestamp": 1579584603,
  "channel": "master",
  "locale": "en_GB"
}
```

## Searching

When searching, suggestions are returned based on how good the match is between [user input](../glossary.md#search-query) and the suggestion itself. 

* Elasticsearch uses the BM25[^BM25] similarity algorithm to measure the [relevance](../glossary.md#relevance) (score).
* The suggestions appear depending on how good is the match and if documents have the same score, sorted by length (from short to long).
* For searching Elasticsearch uses differently configured search analyzers to examine the fields. The most worth mentioning filters of these analyzers are
    * Stemming[^stemmer] 
    * Edge n-gramming[^ngramming]
* When a user inputs a query that returns 0 results, Searchperience will automatically check if there are any suggestions available:
    * If there are suggestions available, a [second search](../glossary.md#second-search) will be performed with the best suggestion as `currentQuery`.
    * Otherwise, an empty response will be returned.

Example of suggestions returned from Searchperience:

```json
{
  "suggestion": {
    "metaData": {
      "totalHits": 3,
      "currentQuery": "Head",
      "originalQuery": null,
      "took": 19
    },
    "hits": [
      {
        "document": {
          "text": "Headstrap"
        },
        "highlights": {
          "text": "<span class=\"sp-highlight\">Head</span>strap"
        }
      },
      {
        "document": {
          "text": "Headphones"
        },
        "highlights": {
          "text": "<span class=\"sp-highlight\">Head</span>phones"
        }
      },
      {
        "document": {
          "text": "Wireless Headphones"
        },
        "highlights": {
          "text": "Wireless <span class=\"sp-highlight\">Head</span>phones"
        }
      }
    ]
  }
}
```

[^stemmer]: More information about [Stemming](https://en.wikipedia.org/wiki/Stemming)
[^ngramming]: More information about [N-gramming](https://en.wikipedia.org/wiki/N-gram)
[^BM25]: More information about [BM25](https://en.wikipedia.org/wiki/Okapi_BM25)