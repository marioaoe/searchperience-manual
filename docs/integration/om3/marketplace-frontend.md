### How to build links to products detail page (PDP)?

In general the URLs are build based on the data you will find in each response. The base domain/url is fixed and not part of the response, so you need to configure it per environment as default value.

!!! note ""
    As convention we do use the following prefix for products `https://example.com/en/product/<value>`

#### URL Components

The URL we have to build consists of the following components:

| Segment                | Example Value          | Description                                                                                                         |
|------------------------|------------------------|---------------------------------------------------------------------------------------------------------------------|
| lang                   | en-US                  | 4 char language code                                                                                                |
| entity                 | product                | the entity which should be shown, in this case it will be always `product`                                          |
| urlSlug                | super-product-demo     | Speaking product name prepared to use as part of the URL (Akeneo field)                                             |
| marketplaceCode        | c2a4d5037fb0688a       | value representing the product key containing the retailer name and the retailer sku (Akeneo field)            |
| variantMarketplaceCode | c2a4d5037fb0688a       | value representing the product marketplaceCode of a simple which is assigned to an configurable (Akeneo field) |

#### Simple product link

When building links for single products. Identified by the `productType : simple` attribute. 

**Schema:**
```
/<lang>/<entity>/<urlSlug>_<marketplaceCode>
```

**Example:**
```
/en-US/product/jellycat-london-bashful-otter-medium_0ea6021d93877677
```

#### Configurable product link

When building links for configurable products. Identified by the `productType : configurable` attribute. 

**Schema:**
```
/<lang>/<entity>/<urlSlug>_<marketplaceCode>-<variantMarketplaceCode>
```

**Example:**
```
/en-US/product/jellycat-london-bashful-otter-medium_0ea6021d9387767-de149b1c2f05
```

!!!note ""
    If `results.product.hits.document.teaserData.selectedVariant` is not set, use the form of the productType=simple


#### Brand link

For brands you should use `https://example.com/en/brands/` as base domain/path, and append the value out of `results.brand.hits.document.code` as value to build for example:
`https://example.com/en/brands/apple`

#### Retailer link

For retailer you should use `https://example.com/en/retailer/` as base domain/path, and append the value out of `results.retailer.hits.document.code` as value to build for example:
`https://example.com/en/retailer/apple`
