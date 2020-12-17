# FAQ

## Frontend

### How is teaserData calculated?

`teaserData` is a calculated object attached to every product. It is built in the Searchperience Frontend using values available in the product fields.

In general:

* If we have the sorting set by selling count, then we fill teaserData with the values from the cheapest variant from the most sold variants. Moreover, if we have selected some facets at the same time, then we take into account only the variants which match all the selected facets.
* If we have some active facet selections, then we fill teaserData with the values from the variant with the cheapest `finalPrice` matching all of the provided filters/facets. Moreover, if multiple variants have the same cheapest `finalPrice`, then the smallest variant will be selected based on the fields `variant.attributes.clothingSize` or `variant.attributes.shoeSize`.
* If we do not have any facet selected, then we fill teaserData with values from configurableProduct. If any of the fields are missing from the variant, then we fill those fields with values from configurableProduct.
* If we have 'availablePrices' data available, then we take into account the price from that section to find the lowest price.

#### How does teaserData select variants?

Incoming document
```json
{
    "foreignId": "Z-en_GB-master",
    "marketplaceCode": "Z",
    "sellingCount": 49,
    "teaserData": "",
    "variants": [{
        "marketplaceCode": "A",
        "sellingCount": 0,
        "color": "red",
        "clothingSize": "s",
        "price": 10.99
    },{
        "marketplaceCode": "B",
        "sellingCount": 0,
        "color": "red",
        "clothingSize": "s",
        "price": 3.99
    },{
        "marketplaceCode": "C",
        "sellingCount": 0,
        "color": "red",
        "clothingSize": "l",
        "price": 3.99
    },{
        "marketplaceCode": "D",
        "sellingCount": 32,
        "color": "blue",
        "clothingSize": "xl",
        "price": 22.00
    },{
        "marketplaceCode": "E",
        "sellingCount": 32,
        "color": "blue",
        "clothingSize": "m",
        "price": 21.00
    },{
        "marketplaceCode": "F",
        "sellingCount": 33,
        "color": "yellow",
        "clothingSize": "s",
        "price": 3.99
    }]
}
```

| Criteria                                | logic to choose                                                                 | teaserPrice | teaserPriceFromPrice | selectedVariant | text/image                                      |
|:----------------------------------------|:--------------------------------------------------------------------------------|:------------|:---------------------|:----------------|:------------------------------------------------|
| nothing selected                        | cheapest variant                                                                | 3,99        | true (all)           | none            | configurableProduct, variants                   |
| color=red                               | cheapest and smallest variant with color=red                                    | 3,99        | true (all)           | B               | cheapest smallest variant with color=red        |
| nothing selected, sort by selling count | cheapest variant with highest selling count                                     | 3,99        | true (all)           | F               | cheapest variant with color=red                 |
| color=blue, sort by selling count       | color=blue, cheapest with highest selling count                                 | 21,00       | true (all)           | E               | cheapest variant with color=blue, selling count |
| color=red, sort by selling count        | color=red (no selling count available for color=red), cheapest smallest variant | 3,99        | true (all)           | B               | cheapest smallest variant with color=red        |

#### How is the teaserPrice applied?

Filled with the `activePrice` object taken from the variant with the cheapest `finalPrice` of all (no facet selection) or matching variants (facet selection).

#### How is the teaserAvailablePrices applied? 

Filled with the `availablePrices` object taken from the variant with the cheapest `finalPrice` of all (no facet selection) or matching variants (facet selection).

#### How is the teaserPriceIsFromPrice applied?

If the `finalPrice` of some variant's `activePrice` differs, this value is `true` indicating the teaserPrice is the "from price" (lowest value) from a range of `finalPrice` values of either all variants (no facet selections) or the matching variants (facet selection).
If the `finalPrice` of all variants is the same this value is false.

#### How is the finalPrice calculated? 

The `finalPrice` is a calculated price built using these rules:

* If `activePrice.isDiscounted` is set to true, the finalPrice is the value from the field `activePrice.discounted`.
* If `activePrice.isDiscounted` is set to false or completely missing, the finalPrice is the value from the field `activePrice.default`.

#### How is teaserData filled?

##### Case 1: Configurable Product - no facet selection

* shortTitle
    * taken from the cheapest-smallest-size variant, if field not set, taken from configurableProduct.
* shortDescription
    * taken from the cheapest-smallest-size variant, if field not set, taken from configurableProduct.
* teaserPrice
    * the `activePrice` object taken from the cheapest-smallest-size variant.
* teaserPriceIsFromPrice
    * if the `finalPrice` of every variants `activePrice` differs, this value is `true` indicating the teaserPrice is the cheapest of all variant's prices.
* selectedVariant
    * not existing in that case

##### Case 2: Configurable Product - one or more facets have been selected

* shortTitle
    * taken from the cheapest-smallest-size variant matched by facets/filters, if field not set, taken from configurableProduct.
* shortDescription
    * taken from the cheapest-smallest-size variant matched by facets/filters, if field not set, taken from configurableProduct.
* teaserPrice
    * the `activePrice` from the cheapest-smallest-size variant matched by facets/filters.
* teaserPriceIsFromPrice
    * if the `finalPrice` of the matching variants `activePrice` differs, this value is `true` indicating the teaserPrice is the cheapest of all matching variant's prices.
* selectedVariant
    * contains the `marketplaceCode` of the cheapest-smallest-size variant matched by facets/filters.

##### Case 3: Simple Product

* shortTitle
    * taken from the only variant, if field not set, taken from configurableProduct.
* shortDescription
    * taken from the only variant, if field not set, taken from configurableProduct.
* teaserPrice
    * the `activePrice` object taken from the first and only variant
* teaserPriceIsFromPrice
    * Always false since we only have one variant and therefore only one activePrice
* selectedVariant
    * not existing in that case

##### Case 4: Configurable Product - multiple variants have the same cheapest `final Price`

* shortTitle
    * taken from the cheapest-smallest-size variant, if field not set, taken from configurableProduct.
* shortDescription
    * taken from the cheapest-smallest-size variant, if field not set, taken from configurableProduct.
* teaserPrice
    * the `activePrice` from the cheapest-smallest-size variant.
* teaserPriceIsFromPrice
    * if other variants have a different `finalPrice`, this value is `true`
* selectedVariant
    * contains the `marketplaceCode` of the smallest-size variant.

## Cockpit

### How can I hide a popular search term?

To hide a popular search one needs to:

- Find the context in which the popular search term appears on
- Find the `foreignId` of the specific popular search item (also called topkeyword)
- Find the object in Searchperience's Cockpit and hide it.

#### What is a TopKeyword?

The `/popularSearches` Frontend endpoint returns a collection of TopKeywords objects. Each Topkeyword consists of various elements as can be seen in the next code snippet.

```json
{
    "foreignId": "a114e69a5746e2cfa326f7c8df21ec1a",    // hash of the fields' contents except occurrences 
    "searchQuery": "henne",                             // user input in search field
    "brandCode": "hennessy",
    "retailerCode": "world-duty-free",
    "categoryCodes": [
        "food_drink",
        "food_drink_wine_spirits",
        "food_drink_wine_spirits_cognac"
    ],
    "channel": "master",
    "locale": "en_GB",
    "referer": "-",
    "occurrences": 1
}
```

A topkeyword contains a `searchQuery` and information about the context in which the query lead to a purchase. This is the reason why information as `brandCode` or `retailerCode` are also present.

### How to find the context of a TopKeyword?

When sending a request to  <___PROJECT_DOMAIN_FRONTEND___/popularSearches>, it is possible to filter by a series of parameters such as `brandCode` or `categoryCode`. Possible filters are documented in the Searchperience API documentation. 

The context in which a popular search appears depend on the current location of the user in the web store. Depending on the category a user find himself in, the web store will request popular searches with the same category as the one the user is in. 

### Find the Id of the popular search item?

With the right context one can reproduce the web stores request to <___PROJECT_DOMAIN_FRONTEND___/popularSearches> by filtering the request. The resulting JSON contains a set of TopKeywords with the format already described [above](#How-to-find-the-context-of-a-TopKeyword?). Finally, one can obtain the `foreignId` by matching the `searchQuery` to the popular search item one is looking for.

### How to hide a document from showing in the search results?

1. Open Cockpit
2. Go to `Indexing > All documents` (left navigation panel)
3. Search for the `foreignId` you obtained by sending a filtered request to the Frontend.
4. Prevent the item from appearing in search results by clicking on the `Hidden` toggle.

## Indexer

### How is sellingCount/viewCount included in a product?

Values are enriched in the Indexer pipe step by matching data from data processing file.

Data contain information about selling/view count per week, month and year.

```json
"sorting":
{
  "price": 14.5,
  "sellingCount": {
    "week": 12,
    "month": 12,
    "year": 12
  },
  "viewCount": {
    "week": 14,
    "month": 14,
    "year": 14
  }
}
```

### How does TopSeller work?

Calculation done in the Indexer pipe step by evaluating data from dataprocessing 'topseller' statistic.
Depending on settings, top seller flag is set and boosting applied to the document.
Setting example from Indexer pipe step configuration:

```json
'topSeller' => [
  // values to get top sellers and enrich product
  'count' => 100,
  'period' => 'week',
  'boost' => 100
]
```

#### Logic of min and max values calculation

You may be wondering how we choose min/max values.

When a user provides min and max values for the range facet (e.g. using a slider) we build an Elasticsearch aggregation and an Elasticsearch query filter for it.
When the aggregation is returned from the Elasticsearch we use it to build the facet using the new value.
For min possible values we take lowest from aggregation or user input, and for the max possible value we take the highest from aggregation or user input.
If you look at the table below it may be a bit clearer:

|                                                            | available min from | available max from | selected min from | selected max from |
|:-----------------------------------------------------------|:-------------------|:-------------------|:------------------|:------------------|
| user did not select anything                               | result             | result             | result            | result            |
| user selected min < available min [^rangeMinMax]           | selected value     | result             | selected value    | selected value    |
| user selected max > available max [^rangeMinMax]           | result             | selected value     | selected value    | selected value    |
| user selected min/max </> available min/max [^rangeMinMax] | selected value     | selected value     | selected value    | selected value    |
