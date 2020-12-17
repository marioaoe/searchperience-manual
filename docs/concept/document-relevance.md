## Product Relevance Calculation

The more relevant a product is the higher it will appear in search results. The order of search results is determined by several factors. However, as a general rule of thumb:

* The less common a word is though all products in the store, the more relevant it is when searching for it. Uncommon words like `hippopotamus` help zoom out the most relevant products. 
* Products that contain a word fewer times are more relevant. A product that contains the word `elegant` two times is more significant than a product that contains the same word five times.
* Words that exactly match a product make this product more relevant in comparison to other product with just a partial match. When searching for `phone`, products containing the word `headphones` are less relevant than the ones containing just `phone`.
* The field that contains the word plays an important role in relevance. When searching for `training`, a document containing this word in the title is more relevant than another product which contains the same word in the description.

Moreover, keep in mind Searchperience relevance modifiers such as [Widgets with a Boosting filter](../cockpit/widgets.md) and [Enrichments](../cockpit/enrichments.md), these elements are designed to override the rules described above and comply with specific business expectations.

## Fields inside Searchperience

To understand how Searchperience handles the content inside a document's field, first you should understand what sub-fields are and how they affect the content of a field.

### Sub-fields

Sub-fields are a copy of the content of a field, however it is transformed so that the content has a higher probability to match the search query. Here is an overview of the sub-field types used by Searchperience.

* **search**: Content is lowercased, splitted into smaller pieces.

```
# the word 'phone' gets splitted so it matches more queries
"phone" => ["p", "ph", "pho", "phon", "phone"]
```

* **exact**: Content is taken lowercased as a whole. Used to exact match fields like marketplaceCode.

```
# the term is exactly the same
"BS234" => ["bs234"]
```

* **path**: Content is broken down on `/`. Used for category paths.

```
# the path is splitted so all elements have a chance to match the query
"space/planet/earth" => ["space", "planet", "earth"]
```

To understand this let's try an example. Imagine a product with the following content:

```
...
"variants" : [ 
    {
        ...
        "title" : "iPhone X 64GB"
        ...
    }
]
...
```

The content of the field `variants.title` is `iPhone X 64GB`. As already stated Searchperience uses sub-fields to increase the probability of matching the user's query, this means that the following field will be implicitly (just inside the database and not visible to the end-user) have the following fields available for matching when searching.

```
variants.title.search   : ["i", "ip", "iph", "ipho", "iphon", "iphone", "x", "6", "64", "64g", "64gb"]
variants.title.exact    : ["iphone x 64gb"]
```

### Search Fields & Relevance Multipliers

Any fields not listed in the table are not considered when searching. The multiplier value indicates how significant is the match between user query and field's content. For example, if there is an exact match in the `variants.shortTitle` it is 5 times more relevant than a match in the `variants.shortDescription`.

| Field in Searchperience                          | Multiplier |
|--------------------------------------------------|------------|
| boostKeywords.exact                              | 5          |
| categoryPaths.path                               | 2          |
| marketplaceCode.exact                            | 5          |
| normalboostw.exact                               | 5          |
| highboostw.exact                                 | 4          |
| lowboostw.exact                                  | 3          |
| smartKeywordHigh.exact                           | 5          |
| smartKeywordLow.exact                            | 3          |
| smartKeywordNormal.exact                         | 4          |
| variants.attributes.baseColor.search             | 2          |
| variants.attributes.brandName.exact              | 10         |
| variants.attributes.brandName.search             | 4          |
| variants.attributes.gtin.search                  |            |
| variants.attributes.productClassification.search |            |
| variants.attributes.productClassification.exact  | 10         |
| variants.boostKeywords.exact                     | 10         |
| variants.description.search                      |            |
| variants.marketplaceCode.exact                   | 10         |
| variants.retailerName.exact                      | 10         |
| variants.retailerName.search                     | 4          |
| variants.retailerSku.search                      |            |
| variants.shortDescription.search                 | 2          |
| variants.shortTitle.search                       | 4          |
| variants.title.exact                             | 10         |
| variants.title.search                            | 4          |
| variants.shortTitle.exact                        | 10         |