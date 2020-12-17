## How to build image links?

Searchperience provides images as references, so it is up to the consumer of the API to build up the URL if they want to get the image. 
Images in OM3 are directly served by the image service.

### Image URL schema

The URL is composed of the following items 

| Path element    | Description                                                                                                                      |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------|
| Domain          | The Domain of the OM3 image service                                                                                              |
| Source          | Application inside OM3 from where the image comes from e.g. `pim` or `mdp`. More info in [Image source](#source)           |
| Signature       | If using one of the [default image sizes](#image-size) just use `defaults`, otherwise consult with us.                  |
| Size            | Use one of the [default image sizes](#image-size) or a custom ones with the general form `{width}x{height}` e.g. 10x150 |
| Media Reference | String representing the location of the image in OM3. This is the string the Searchperience delivers.                            |

In general, the URL schema looks like this:
`https://<domain>/<source>/<signature>/<image_size>/<media_reference>`

Examples
```
# Image with `defaults` signature
https://images.om3.cloud/pim/defaults/400x/catalog/b/7/0/b/b70bd455bc6b8fbc070d573c67890689_pupm_071t_c01_79_hr.jpg

# Image with custom size
https://images.om3.cloud/pim/tMNNTLwq1uG2K_8G0DKtoXi1q9WW5jf-JOXiIvSvksI=/10x150/catalog/b/7/0/b/b70bd455bc6b8fbc070d573c67890689_pupm_071t_c01_79_hr.jpg
```

#### Source   

| Entity Type | Source    | OM3 Application  |
|-------------|-----------|------------------|
| Brand       | `mdp`     | MasterDataPortal |
| Location    | `mdp`     | MasterDataPortal |
| Product     | `pim`     | Akeneo           |
| Promotion   | `cockpit` | Cockpit          |
| Retailer    | `mdp`     | MasterDataPortal |
| Barcode     | `barcode` | --               |

#### Signature

The signature allows to define which size the image should have. You can access images with the  `defaults` signature when using predefined sizes. E.g. "200x", "300x", "400x" or "800x". 
For custom sizes you need a signed request. Please consult us for more information about signatures.

#### Image size

!!! note These are the default image sizes. It is possible to use custom ones, but then the signature cannot be `defaults`. 

* 100x
* 200x
* 300x
* 400x
* 800x

#### Media Reference

The media reference indicates the path where the image is stored in the Amazon S3 bucket. 
For most document types one can directly use the content of the `reference` field in the media object. However, for documents with source `pim` a prefix is necessary

| PIM Document Type | Prefix      |
|-------------------|-------------|
| `product`         | `/catalog`  |
| `category`        | `/category` |

#### Examples 
 
| Document Type | Reference                                               | Image Link                                                                                                            |
|---------------|---------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| `category`    | `8/2/7/5/8275b21eb93d74ed4ffeac43fb2539d7_banner_.jpg`  | `https://images.om3/pim/defaults/400x/category/8/2/7/5/8275b21eb93d74ed4ffeac43fb2539d7_banner_.jpg`                  |
| `product`     | `e/a/e/4/eae4685a25fbd7da95352d077b3183c3_80246531.jpg` | `https://images.om3/pim/defaults/300x/catalog/e/a/e/4/eae4685a25fbd7da95352d077b3183c3_80246531.jpg`                  |
| `brand`       | `/media/images/362/brand_logo_2019_01.png`              | `https://images.om3/mdp/lN3rodS1TAIPAR4L8zbatoUbxDaD0v1W1fJYzt4wYNg=/320x100/media/images/362/brand_logo_2019_01.png` |
| `retailer`    | `/media/images/8/cat.jpg`                               | `https://images.om3/mdp/b7zRILFhjEip2tTJyzhYBu1jTt_IAS9q3HgPMfPL1qY=/660x550/media/images/8/1-1/w/660/cat.jpg`        |
| `cockpit`     | `09dd0538a5e81abf873957a3d545a12a8f94560e/promo.jpg`    | `https://images.om3/cockpit/defaults/400x/09dd0538a5e81abf873957a3d545a12a8f94560e/promo.jpg`                         |