The [endpoints](../../glossary.md#endpoint) of the Searchperience [Frontend](../../glossary.md#frontend) are usually internally and used in OM3 only.
However, the Searchperience Frontend could also deliver [types](../../glossary.md##types) like products and categories via external endpoints.
These external endpoints are exposed via Ambassador which is an API gateway that maps each endpoint with a prefix, e.g. `/ecommerce/search`, to the `/external` endpoint(s) of the Searchperience Frontend.

## Products

The external endpoint delivers products in the [Google product format](https://developers.google.com/shopping-content/reference/rest/v2.1/products).
Usually, the product endpoints in OM3 deliver [simple](../../glossary.md#simple-product) and [configurable products](../../glossary.md#configurable-product).
Configurable products contain more than one variant of a product, e.g. different sizes or colors.
The external endpoint only delivers simple products that means that each variant of a configurable product is also exposed as a simple product.
To identify all variants that belong to the same product, these products share an identifier which is called `itemGroupId`.
The products that have an attribute `itemGroupId` should also have one of these attributes: color, sizes, or gender.
