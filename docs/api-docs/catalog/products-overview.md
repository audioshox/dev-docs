# Catalog Overview



The Catalog refers to a store's collection of physical and digital products. The Catalog includes all the information about a product such as MPN, warranty, price, and images.

## OAuth scopes

| UI Name  | Permission | Parameter                     |
|----------|------------|-------------------------------|
| Products | modify     | `store_v2_products`           |
| Products | read-only  | `store_v2_products_read_only` |

For more information on OAuth Scopes and authentication, see [Authentication](/api-docs/getting-started/authentication).

## Products overview

[Products](/api-reference/store-management/catalog/products/getproducts) are the primary catalog entity, and the primary function of the ecommerce platform is to sell products on the storefront and through other channels.


Products can be physical or digital:
* **Physical** - Material goods that have a weight and take up space.  Merchants ship them out, or customers pick them up.

* **Digital** - Intangible purchases that represent virtual goods, licenses, services, or events.  Customers download, redeem, experience, or attend them.


<!-- theme: info -->
> #### Note
> You can only create one product per request.

## Creating a product

The following sample `POST` request creates a physical product with no optional modifiers.


```http title="Example request: Create a physical product" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "name": "BigCommerce Coffee Mug",
  "price": 10.00,
  "categories": [
    23,
    21
  ],
  "weight": 4,
  "type": "physical"
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/products/createproduct#requestrunner) -->

## Creating products with variant options

To create a product with variant options that shoppers select, include a `variants` array in the request body.

```http title="Example request: Create a product with variants" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "name": "BigCommerce Coffee Mug",
  "price": 10.00,
  "categories": [
    23,
    21
  ],
  "weight": 4,
  "type": "physical",
  "variants": [
    {
      "sku": "SKU-BLU",
      "option_values": [
        {
          "option_display_name": "Mug Color",
          "label": "Blue"
        }
      ]
    },
    {
      "sku": "SKU-GRAY",
      "option_values": [
        {
          "option_display_name": "Mug Color",
          "label": "Gray"
        }
      ]
    }
  ]
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/products/createproduct#requestrunner) -->

<!-- theme: info -->
> #### Note
> When you create variant options using the `/products` endpoint, `display_type` defaults to a radio button (displayed as selectable boxes in some themes).

## Creating digital products

To create a digital product, set `type` to `digital`.

```http title="Example request: Create a digital product" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "name": "ebook: A Guide to Coffee",
  "price": 10.00,
  "categories": [
    23,
    21
  ],
  "type": "digital",
  "images": [
    {
      "is_thumbnail": true,
      "image_url": "{{image_url}}"
    }
  ]
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/products/createproduct#requestrunner) -->

<!-- theme: info -->
> #### Note
> You can only upload files to attach to digital products using [WebDAV or your control panel](https://support.bigcommerce.com/s/article/Creating-Downloadable-Products) -- the API does not support uploading assets other than product photos. You can set additional settings, such as file description and maximum downloads, in the control panel.

## Adding product images

To add an image to a product, send a request to the [Create a product image](/api-reference/store-management/catalog/product-images/createproductimage) endpoint.

```http title="Example request: Add a product image by image_url" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/images
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "is_thumbnail": true,
  "sort_order": 1,
  "description": "Yellow Large Bath Towel",
  "image_url": "{{image_url}}"
}
```

<!-- theme: info -->
> #### Product image limits
> Products are limited to **1,000** images in total, including any images associated with variants. Consult []() for additional product image-related limits. 

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/store-management/catalog/product-images/createproductimage#requestrunner) -->

```http title="Example request: Add a previously uploaded product image" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/images
Accept: application/json
Content-Type: multipart/form-data
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "is_thumbnail": false, //additional image
  "sort_order": 1,
  "description": "Yellow Large Bath Towel",
  "image_file": "{{image_path}}" //previously uploaded to BigCommerce
}
```

<!-- theme: info -->
> #### Note
> * If using `image_file`, set the `Content-Type` header to `multipart/form-data`. Otherwise, you will be unable to add subsequent requests.
> * Set `is_thumbnail` to true to use this image on product listing pages.
> * A product can have only one thumbnail image at a time.
> * If only one image is associated with a product, it is both the main product image and the thumbnail.
> * You can also add variant-specific images when [creating a variant](/api-reference/store-management/catalog/product-variants/createvariant).

## Adding product videos

To add a YouTube-hosted video as a product video, send a request to the [Create a product video](/api-reference/store-management/catalog/product-videos/createproductvideo) endpoint.

```http title="Example request: Add a product video" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/videos
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "title": "BigCommerce Mug Video",
  "description": "Video Describing the Mug",
  "sort_order": 1,
  "type": "youtube",
  "video_id": "_KMh8yqDSlg"
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/store-management/catalog/product-videos/createproductvideo#requestrunner) -->

<!-- theme: info -->
> #### Note
> * A product can have more than one video.
> * Currently, the API only supports YouTube videos.
> * `video_id` corresponds to the `v` parameter in the URL (Ex: `https://www.youtube.com/watch?v=_KMh8yqDSlg`).

## Adding custom fields

To add custom fields to a product, send a request to the [Create a custom field](/api-reference/store-management/catalog/product-custom-fields/createcustomfield) endpoint.

```http title="Example request: Create a custom field" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/custom-fields
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "name": "Release Year",
  "value": "2018"
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-custom-fields/createcustomfield#requestrunner) -->

<!-- theme: info -->
> #### Custom field limits
> Custom field values are limited to **250** characters. For more about using custom fields, see [Custom Fields](https://support.bigcommerce.com/s/article/Custom-Fields).

## Adding bulk pricing rules

To add bulk pricing to products based on purchase quantity, send a request to the [Create a bulk pricing rule](/api-reference/store-management/catalog/product-bulk-pricing-rules/createbulkpricingrule) endpoint.

```http title="Example request: Create a bulk pricing rule" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/bulk-pricing-rules
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "bulk_pricing_rules": [
    {
      "quantity_min": 10,
      "quantity_max": 15,
      "type": "price",
      "amount": 3
    },
    {
      "quantity_min": 16,
      "quantity_max": 25,
      "type": "price",
      "amount": 5
    }
  ]
}
```

For details about configuring bulk prices, see [Bulk Pricing](https://support.bigcommerce.com/s/article/Bulk-Pricing).

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-bulk-pricing-rules/updatebulkpricingrule#requestrunner) -->


## Pricing precision

BigCommerce pricing is precise up to `4` decimal places. For example:

* `"$ 10.99999` rounds up to `$ 11`
* `"$ 10.99994` rounds down to `$ 10.9999`

Currency display settings allow for more than four decimal places. In such cases, the additional decimal places will display as `0`s.

## Adding product metafields

[Metafields](/api-reference/store-management/catalog/product-metafields/createproductmetafield) are key-value pairs intended to programmatically store data about a product or other entity. Metafield data does not appear in the storefront or the control panel, but can be useful to improve your catalog's integration with another service, such as a shipping app.

To add metafields to a product, send a request to the [Create a product metafield](/api-reference/store-management/catalog/product-metafields/createproductmetafield) endpoint.

```http title="Example request: Create a product metafield" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/metafields
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "permission_set": "read",
  "namespace": "Location",
  "key": "bin_number",
  "value": "#4456",
  "description": "location of the product",
  "resource_type": "product",
  "resource_id": 131
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-metafields/updateproductmetafield#requestrunner) -->

<!-- theme: info -->
> #### Note
> You can add metafields to variants, products, categories, and brands.



## Adding product reviews

To add reviews to a product, send a request to the [Create a product review](/api-reference/store-management/catalog/product-reviews/createproductreview) endpoint.

```http title="Example request: Add a product review" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/reviews
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "title": "Great Coffee Mug",
  "text": "This coffee mug kept my liquids hot for several hours.",
  "status": "pending",
  "rating": 5,
  "email": "testing@bigcommerce.com",
  "name": "BigCommerce",
  "date_reviewed": "2021-07-20T17:45:13+00:00"
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/store-management/catalog/product-reviews/createproductreview#requestrunner) -->

<!-- theme: info -->
> #### Note
> You cannot create reviews in the control panel.

## Variants and modifiers

Products vary, and those differences matter.  [Variants](/api-reference/store-management/catalog/product-variants/getvariantbyid) represent items that customers can purchase.  One style of shoes, for instance, can come in an assortment of sizes, colors, and materials.  If the product is signature sneakers, the variant is a brick-colored pair of signature sneakers with white soles and marine plastic uppers in US women's size 9.  Everything a customer can buy is a variant.  A product with no options is its own variant - called a _base variant_.

In this example, every meaningfully distinct pair of shoes is a product variant.  The differences that combine to create those options are variant options.  And the actual values of those options are variant option values.

### Variants options and values for signature sneakers

| Variant options   | Variant option values                    | No. of variants |
|-------------------|------------------------------------------|----------------:|
| size (US Women's) | 6, 6.5, 7, 7.5, 8, 8.5, 9, 9.5, 10, 10.5 |              10 |
| upper material    | canvas, marine plastic, leather          |               3 |
| upper color       | brick, azul, gold                        |               3 |
| sole color        | charcoal, white, azul                    |               3 |
|                   |                                          |         **270** |


In theory, these variant options and variant option values form 270 possible distinct variants of signature sneakers.  In practice, these may not all exist!  Variants can have their own prices, weights, dimensions, images, etc.  They will inherit these values from the parent product if you do not specify them.  Variants are typically what you track inventory against, so each variant must have its own _SKU_.  The catalog generates the set of possible variants based on the variant options you configure using the control panel or the [Create a product](/api-reference/store-management/catalog/products/createproduct) endpoint.

<!-- theme: info -->
> Consult the [Create a variant](/api-reference/store-management/catalog/product-variants/createvariant) endpoint reference for limits. If you're working with more variants, categories and metafields can help to integrate multiple products.

## Variant options
If a product has variants, the shopper must select a value for each variant option before adding the product to their cart.  The shopper typically makes this choice by manipulating a UI element, such as a
  * **rectangle**, 
  * **radio button**, 
  * **swatch**, 
  * **product pick list**, or
  * **product pick list with images**.

### Variant options in V2 and V3

* _SKUs_ in V2 become _variants_ in V3.
* _Base variants_ do not correlate with SKUs in V2.


<!-- theme: info -->
> Creating a variant option does not generate a SKU. You can add SKUs to variants later using the [Update a product variant](/api-reference/store-management/catalog/product-variants/updatevariant) endpoint.

### Create variant options

The following request [Creates a variant option](/api-reference/store-management/catalog/product-variant-options/createoption) and populates it with values for the customer to choose between. In a separate request, you can add SKUs representing the resulting variants. To add new values to an existing variant option, use the [Update a variant option](/api-reference/store-management/catalog/product-variant-options/updateoption) endpoint.

<!-- theme: info -->
> If a product has variant options created using the V2 API, you cannot add additional variant options using the V3 API.

### Create variant options

```http title="Example request: Create variant options" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/options
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "product_id": 134,
  "name": "Size Rectangle",
  "display_name": "Size",
  "type": "rectangles",
  "option_values": [
    {
      "label": "S",
      "sort_order": 0,
      "is_default": false
    },
    {
      "label": "M",
      "sort_order": 1,
      "is_default": true
    },
    {
      "label": "L",
      "sort_order": 2,
      "is_default": false
    }
  ]
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-options/createoption#requestrunner) -->

<!-- theme: warning -->
> #### V2 SKU rules will override variant pricing
> Creating SKU rules via the V2 API or via CSV import will alter or override any variant price or sale price added to a product via the control panel, V3 API, or Price Lists UI.


### Variant examples:

| Product | Variant option | Variant |
| -- | -- | -- |
| T-Shirt | Blue<br>-<br> Small<br> Medium<br> Large| SM-BLU<br> SM-MED <br> SM-LARG
| Backpack | Black<br>Yellow<br>-<br>2L <br> 3L<br> 8L |BLACK-2L<br>BLACK-3L<br>BLACK 8L<br>-<br>YELLOW-2L<br>YELLOW-3L<br>YELLOW-8L|

## Creating variants

You can create variants in two ways:
* From existing variant options, using [Create a Product Variant](/api-reference/store-management/catalog/product-variants/createvariant) endpoint.

* By adding variants with options and SKUs, using [Create a Product](/api-reference/store-management/catalog/products/createproduct) endpoint.


The example below will go over using existing variant options to create the variants.

To fetch variant information, send a `GET` request to `/v3/catalog/products/{{product_id}}/options`.

```http title="Get product variant options"
GET https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/options
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "data": [
    {
      "id": 193,
      ...
      "option_values": [
        {
          "id": 163,
          "label": "S",
          "sort_order": 0,
          "value_data": null,
          "is_default": false
        },
        ...
      ],
      "config": []
    },
    {
      "id": 194,
      ...
      "option_values": [
        {
          "id": 166,
          "label": "Blue",
          "sort_order": 1,
          "value_data": {
            "colors": [
              "#123C91"
            ]
          },
        },
          ...

      ],
      "config": []
    }
  ],
  "meta": {
    ...
    }
  }
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-options/getoptions#requestrunner) -->

In the above response, there are two variant options of size and color with three values each.

To combine the variant option values into variants and build out SKUs use the following endpoint:

`https://api.bigcommerce.com/stores/{{store_hash}}/v3/catalog/products/{{product_id}}/variants`

<!-- theme: info -->
> #### Note
> * You can create one variant at a time using this endpoint.  Individual variant options will contain an array of multiple values.
> * To use a variant array and create variants in the same call as the base product, use the [Create a product](/api-reference/store-management/catalog/products/createproduct) endpoint.


The `option_values` array combines the options small and blue to create the SKU SMALL-BLUE. The ID in the `option_values` array is the ID from the variant option response `option_values > id`. The `option_id` is the ID of the option.


```json title="Example variant objects" lineNumbers
{
  "id": 193, //option_id
  "product_id": 134,
  "name": "Size1533313432-134",
  "display_name": "Size",
  "type": "rectangles",
  "sort_order": 0,
  "option_values": [
    {
        "id": 163, //id
        "label": "S",
        "sort_order": 0,
        "value_data": null,
        "is_default": false
    },
    ...
  ]
}
```

### Create a variant using the product endpoint

The following example creates a base product, variant options, and variants in a single call to the [Create a product](/api-reference/store-management/catalog/products/createproduct) endpoint. Use this method to create a product and variants in a single call without creating variant options first (option display will default to radio button).


```http title="Example request: Create a product" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "name": "BigCommerce Coffee Mug",
  "price": 10.00,
  "categories": [
    23,
    21
  ],
  "weight": 4,
  "type": "physical",
  "variants": [
    {
      "sku": "SKU-BLU",
      "option_values": [
        {
          "option_display_name": "Mug Color",
          "label": "Blue"
        }
      ]
    },
    {
      "sku": "SKU-GRAY",
      "option_values": [
        {
          "option_display_name": "Mug Color",
          "label": "Gray"
        }
      ]
    }
  ]
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/products/createproduct#requestrunner) -->

<!-- theme: info -->
> #### Supported types
> Swatch, radio button, rectangle, dropdown, product list, and product list with images.




## Modifier options

[Modifier options](/api-reference/store-management/catalog/product-modifiers/getmodifiers) are any choices that the shopper can make to change how the merchant fulfills the product. Examples include:
* A checkbox to add shipping insurance
* Text to be engraved on the product
* A selected color for an unfinished product before it’s shipped

Critically, the modifier will not change the SKU/variant fulfilled, and you cannot track inventory against combinations of modifier values. Modifiers typically would not change which product is “picked off the shelf” in the warehouse, but they change what happens to that product before sending it to the shopper, or how a merchant can send it.

Modifier options:
* May be required or non-required
* Support all option types
* Cannot be used as part of a variant

You can add an adjuster to a modifier option to change things, such as increasing the price, changing the weight, or shipping rules.  You cannot apply adjusters to all modifier types.

### Modifier options example


| Product | Variant option | Variant |Modifier |
| -- | -- | -- | -- |
| T-Shirt | Blue<br>-<br> Small<br> Medium<br> Large| BLU<br> BLU-MED <br> BLU-LARG| Checkbox<br>Donate to Charity|
| Backpack | Black<br>Yellow<br>-<br>2L <br> 3L<br> 8L |BLACK-2L<br>BLACK-3L<br>BLACK 8L<br>-<br>YELLOW-2L<br>YELLOW-3L<br>YELLOW-8L| Text Field<br> Add Embroidery|

<!-- theme: info -->
> #### Modifiers that support adjusters
> Swatch, radio button, rectangle list, drop-down, product list, and product list with images.




### Add a modifier with price adjuster to an existing product

The following example shows how to add a modifier and a checkbox with a price adjuster to increase the product's price by five dollars.

Creating a checkbox with an adjuster requires two separate calls: one to create the checkbox and another one to add the adjuster. You can define adjusters within the `option_values` array, but `option_values` are not allowed in the request to create a checkbox modifier because creating a checkbox automatically generates two mandatory option values: `Yes` and `No`. Once you have created the checkbox and its option values, you can update the modifier to add an adjuster.

<!-- theme: info -->
> #### Modifiers that require a second step to add an adjuster
> Swatch, radio button, drop-down, rectangle list, product list, product list with images, and checkbox.


The following is an example request to [create a product modifier](/api-reference/store-management/catalog/product-modifiers/createmodifier).

```http title="Example request: Create a modifier" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{PRODUCT_ID}}/modifiers
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "type": "checkbox",
  "required": false,
  "config": {
    "default_value": "Yes",
    "checked_by_default": false,
    "checkbox_label": "Check for Donation"
  },
  "display_name": "Add a $5 Donation"
}
```
&nbsp;
<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-modifiers/createmodifier#requestrunner) -->
```json title="Example response: Create a modifier" lineNumbers
{
  "data": [
    {
      "id": 160,
      "product_id": 131,
      "name": "Add-a-$5-Donation1535039590-191",
      "display_name": "Add a $5 Donation",
      "type": "checkbox",
      "required": false,
      "config": {
        "checkbox_label": "Check for Donation",
        "checked_by_default": false
      },
      "option_values": [
        {
          "id": 149,
          "option_id": 160,
          "label": "Yes",
          "sort_order": 0,
          "value_data": {
            "checked_value": true
          },
          "is_default": false,
          "adjusters": {...},
        },
        {
          "id": 150,
          "option_id": 160,
          "label": "No",
          "sort_order": 1,
          "value_data": {
            "checked_value": false
          },
          "is_default": true,
          "adjusters": {...}
        }
      ]
    }
  ],
  "meta": {...}
}
```

Since this is a checkbox with two states, you create two option values. The default `adjuster_value` is null.

The following is an example request to [update a product modifier value](/api-reference/store-management/catalog/product-modifier-values/updatemodifiervalue).

```http title="Example request: Update modifier value" lineNumbers
PUT https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/modifiers/{{modifier_id}}/values/{{value_id}}
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "is_default": false,
  "adjusters": {
    "price": {
      "adjuster": "relative",
      "adjuster_value": 5
    }
  }
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-modifier-values/updatemodifiervalue#requestrunner) -->

### Troubleshooting: 422 Errors

```json title="Example error: V2-V3 options" lineNumbers
{
    "status": 422,
    "title": "The product is currently associated with an option set, please remove it before editing an option or modifier.",
    "type": "https://developer.bigcommerce.com/api#api-status-codes",
    "errors": {
        "product_id": "The product is currently associated with an option set, please remove it before editing an option or modifier."
    }
}
```

To fix this error:

* Modify the products using the V2 API.
* Remove the option set using the V2 API or the control panel, then remake the variants and modifiers using V3.

## Complex rules

[Complex rules](/api-reference/store-management/catalog/product-complex-rules/getcomplexrules) allow merchants to set up conditions and actions based on shopper option selections on the storefront. You can use them to vary the following based on the shopper's option selections:
* Price
* Weight
* Image
* Purchasability

Adjustments made by complex rules are displayed to shoppers in real-time on the storefront.

For most merchant use cases, **best practice** will be to either assign values (such as a price) directly to a variant or use adjusters on the modifier option itself. However, complex rules exist for rare cases where a rule condition is too complex to express in those forms easily.

Use complex rules when an adjustment should be triggered by:
* The selection of values across multiple modifier options.
* The combination of a particular variant/SKU and a modifier option value.

### Complex rules example

| Product | Variant option | Variant |Modifier | Complex rule |
| -- | -- | -- | -- | -- |
| T-Shirt | Blue<br>-<br> Small<br> Medium<br> Large| SM-BLU<br> SM-MED <br> SM-LARG| Checkbox<br>Donate to Charity| Checkbox<br> Donate to Charity.<br> Add $5
| Backpack | Black<br>Yellow<br>-<br>2L <br> 3L<br> 8L |BLACK-2L<br>BLACK-3L<br>BLACK 8L<br>-<br>YELLOW-2L<br>YELLOW-3L<br>YELLOW-8L| Text Field<br> Add Embroidery| N/A

<br>

### Creating complex rules based on modifiers

Complex rules must have a combination of two or more modifiers, such as two checkboxes. The following example request to the [Create a complex rule](/api-reference/store-management/catalog/product-complex-rules/createcomplexrule) endpoint will add $10 to the product price when you check both boxes.

```http title="Example request: Create a complex rule" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/products/{{product_id}}/complex-rules
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "product_id": 1200,
  "enabled": true,
  "price_adjuster": {
    "adjuster_value": 10
  },
  "conditions": [
    {
      "modifier_id": 506,
      "modifier_value_id": 852
    },
    {
      "modifier_id": 507,
      "modifier_value_id": 854
    }
  ]
}
```
<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/product-complex-rules/updatecomplexrule#requestrunner) -->

### Troubleshooting: 422 Errors

Complex rules must consist of multiple conditions that trigger the rule adjustment. If multiple conditions are not specified, the request will return a 422 error.

```json title="Example error: complex rules" lineNumbers
{
    "status": 422,
    "title": "The rule must contain multiple modifier conditions with unique modifier ids or a variant condition and modifier condition",
    "type": "https://developer.bigcommerce.com/api#api-status-codes"
}
```

## Creating brands

To create a brand, send a request to the [Create a brand](/api-reference/store-management/catalog/brands/createbrand) endpoint.


```http title="Example request: Create a brand" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/brands
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "name": "BigCommerce",
  "page_title": "BigCommerce",
  "meta_keywords": [
    "ecommerce",
    "best in class",
    "grow your business"
  ],
  "image_url": "{{image_url}}"
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/brands/createbrand#requestrunner) -->

For general information on brands and their use cases, see [Managing Brands](https://support.bigcommerce.com/s/article/Managing-Brands).

## Categories

[Categories](/api-reference/store-management/catalog/category/getcategories) are a hierarchy of products available on the store, presented in a tree structure. A store's category structure determines the primary menu structure of most storefront themes directly tied to it.

BigCommerce's V3 REST API does not require products to be associated with a category during creation. You can add new products to a catalog without a category, which can be assigned later if desired. A store's category can contain multiple products or no products at all and still be valid. 

You can associate products with multiple categories. A product associated with categories does not currently have any priority or weighted order (there's no “primary category”). The absence of priority or weighted order makes it difficult to integrate with some external systems that might wish to use a product's categories to map to a category structure.

The following is an example request to the **single storefront** [Create a category](/api-reference/store-management/catalog/category/createcategory) endpoint. To work with categories in a multi-storefront or multi-channel context, use the [batch categories endpoints](/api-reference/store-management/catalog/categories-batch/createcategories) and consult the [categories section of the Multi-Storefront API Guide](/api-docs/multi-storefront/api-guide#categories).

```http title="Example request: Create a category, single storefront" lineNumbers
POST https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/categories
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}

{
  "parent_id": 18,
  "name": "Shoes",
  "description": "Shoes Available for purchase",
  "sort_order": 1,
  "page_title": "Shoes",
  "is_visible": true
}
```

<!-- [![Open in Request Runner](https://storage.googleapis.com/bigcommerce-production-dev-center/images/Open-Request-Runner.svg)](/api-reference/catalog/catalog-api/category/createcategory#requestrunner) -->

### Category tree

[Category Tree](/api-reference/store-management/catalog/category/getcategorytree) returns a simple view of the parent > child relationship of all categories in the store. You can use this endpoint to fetch the categories if building out a custom navigation for a store.

```http
GET https://api.bigcommerce.com/stores/{{STORE_HASH}}/v3/catalog/categories/tree
Accept: application/json
Content-Type: application/json
X-Auth-Token: {{ACCESS_TOKEN}}
```

**Response:**:

```json
{
  "data": [
    {
      "id": 25,
      "parent_id": 0,
      "name": "Towels",
      "is_visible": true,
      "url": "/towels/",
      "children": [
        {
          "id": 26,
          "parent_id": 25,
          "name": "Bath Towels",
          "is_visible": true,
          "url": "/towels/bath-towels/",
          "children": [
            {
              ...
              "children": [
                ...
                ]
            },
            ...
          ]
        },
        ..
      ]
    },
    ...
  ],
  "meta": {...}
}
```

## Product Sort Order

[Product Sort Order](/api-reference/store-management/catalog/product-sort-order) allows you to manage the sort order of products displayed on any given category page. Products assigned to multiple storefront categories can have different sort order values per category.

### Product sorting on a storefront 

The Catalog API supports two manually managed methods of product sorting: on a category level and a product level. If a user combines both sorting methods on a storefront, products with sort order values on a category level take priority. If there is no sort order value on a category level, the Catalog API sorts products by values on a product level.

Product sorting methods:

1. Manually specified sort order on a category level.
2. Manually specified sort order on a product level. `0` by default. 

<!-- theme: info -->
> #### Note
> Products with the same sort order value either on a category or a product level are sorted by `product id` as a second criterion.



## Related resources

### Endpoints
* [Catalog API](/api-reference/store-management/catalog)

### Webhooks
* [Products](/api-docs/store-management/webhooks/events#products)
* [Categories](/api-docs/store-management/webhooks/events#category)
* [SKU](/api-docs/store-management/webhooks/events#sku)
