---
title: addDownloadableProductsToCart mutation | Commerce Web APIs
contributor_name: Atwix
contributor_link: https://www.atwix.com/
---

# addDownloadableProductsToCart mutation

<InlineAlert variant="warning" slots="text" />

We recommend using the [addProductsToCart mutation](add-products.md) to add any type of product to the cart.

A downloadable product can be anything that you can deliver as a file, such as an eBook, music, video, software application, or an update. To add a downloadable product to a cart, you must provide the cart ID, the SKU, and the quantity. In some cases, you must include the IDs for downloadable product links. You can also optionally specify customizable options.

## Syntax

```graphql
mutation: {
    addDownloadableProductsToCart(
        input: AddDownloadableProductsToCartInput
    ): {
        AddDownloadableProductsToCartOutput
    }
}
```

## Example usage

The following examples show how to add a downloadable product to a shopping cart , depending on whether the **Links can be purchased separately** option is selected on the **Downloadable Information** section of the product page.

### Add a downloadable product to a cart with `Links can be purchased separately` enabled

The following example shows how to add a downloadable product in which the **Links can be purchased separately** option is enabled. The payload includes custom downloadable links `Episode 2` and `Episode 3`.

**Request:**

```graphql
mutation {
  addDownloadableProductsToCart(
    input: {
      cart_id: "gMV2BFQuNGiQmTnepQlMGko7Xc4P3X1w"
      cart_items: {
        data: {
          sku: "240-LV09"
          quantity: 1
        }
        downloadable_product_links: [
          {
            link_id: 7                 # Episode 2
          }
          {
            link_id: 8                 # Episode 3
          }
        ]
      }
    }
  ) {
    cart {
      items {
        product {
          sku
        }
        quantity
        ... on DownloadableCartItem {
          links {
            title
            price
          }
          samples {
            title
            sample_url
          }
        }
      }
    }
  }
}
```

**Response:**

```text
{
  "data": {
    "addDownloadableProductsToCart": {
      "cart": {
        "items": [
          {
            "product": {
              "sku": "240-LV09"
            },
            "quantity": 1,
            "links": [
              {
                "title": "Episode 2",
                "price": 9
              },
              {
                "title": "Episode 3",
                "price": 9
              }
            ],
            "samples": [
              {
                "title": "Trailer #1",
                "sample_url": "https://<M2_INSTANCE>/downloadable/download/sample/sample_id/16/"
              },
              {
                "title": "Trailer #2",
                "sample_url": "https://<M2_INSTANCE>/downloadable/download/sample/sample_id/17/"
              },
              {
                "title": "Trailer #3",
                "sample_url": "https://<M2_INSTANCE>/downloadable/download/sample/sample_id/18/"
              }
            ]
          }
        ]
      }
    }
  }
}
```

### Add a downloadable product to a cart with disabled `Links can be purchased separately`

The following example shows how to add a downloadable product in which the **Links can be purchased separately** option is disabled. All downloadable links are added to the cart automatically.

**Request:**

```graphql
mutation {
  addDownloadableProductsToCart(
    input: {
      cart_id: "gMV2BFQuNGiQmTnepQlMGko7Xc4P3X1w"
      cart_items: {
        data: {
          sku: "240-LV07"
          quantity: 1
        }
      }
    }
  ) {
    cart {
      items {
        product {
          sku
        }
        quantity
        ... on DownloadableCartItem {
          links {
            title
            price
          }
          samples {
            title
            sample_url
          }
        }
      }
    }
  }
}
```

**Response:**

```text
{
  "data": {
    "addDownloadableProductsToCart": {
      "cart": {
        "items": [
          {
            "product": {
              "sku": "240-LV07"
            },
            "quantity": 2,
            "links": [
              {
                "title": "Solo Power Circuit",
                "price": 14
              }
            ],
            "samples": [
              {
                "title": "Trailer #1",
                "sample_url": "https://<M2_INSTANCE>/downloadable/download/sample/sample_id/10/"
              },
              {
                "title": "Trailer #2",
                "sample_url": "https://<M2_INSTANCE>/downloadable/download/sample/sample_id/11/"
              },
              {
                "title": "Trailer #3",
                "sample_url": "https://<M2_INSTANCE>/downloadable/download/sample/sample_id/12/"
              }
            ]
          }
        ]
      }
    }
  }
}
```

## Input attributes

The top-level `AddDownloadableProductsToCartInput` object is listed first. All child objects are listed in alphabetical order.

### AddDownloadableProductsToCartInput object

The `AddDownloadableProductsToCartInput` object must contain the following attributes:

Attribute |  Data Type | Description
--- | --- | ---
`cart_id` | String! | The unique ID that identifies the customer's cart
`cart_items` | [[DownloadableProductCartItemInput!]!](#downloadableproductcartiteminput-object) | Contains the cart item IDs and quantity of each item

### CartItemInput object

The `CartItemInput` object must contain the following attributes:

import CartItemInput from '/src/_includes/graphql/cart-item-input-24.md'

<CartItemInput />

### CustomizableOptionInput object

The `CustomizableOptionInput` object can contain the following attributes:

import CustomizableOptionInput from '/src/_includes/graphql/customizable-option-input-24.md'

<CustomizableOptionInput />

### DownloadableProductCartItemInput object

The `DownloadableProductCartItemInput` object can contain the following attribute:

Attribute |  Data Type | Description
--- | --- | ---
`customizable_options` |[[CustomizableOptionInput!]](#customizableoptioninput-object) | An array that defines customizable options for the product
`data` | [CartItemInput!](#cartiteminput-object) | Required. An object containing the `sku` and `quantity` of the product
`downloadable_product_links` | [[DownloadableProductLinksInput!]](#downloadableproductlinksinput-object) | An object containing the `link_id` of the downloadable product link

### DownloadableProductLinksInput object

If specified, the `DownloadableProductLinksInput` object must contain the following attribute.

Attribute |  Data Type | Description
--- | --- | ---
`link_id` | Int! | A unique ID (`downloadable_link`.`link_id`) of the downloadable product link

## Output attributes

The `AddDownloadableProductsToCartOutput` object contains the `Cart` object.

Attribute |  Data Type | Description
--- | --- | ---
`cart` |[Cart!](#cart-object) | Describes the contents of the specified shopping cart

### Cart object

import CartObject from '/src/_includes/graphql/cart-object-24.md'

<CartObject />

## Errors

Error | Description
--- | ---
`Could not find a cart with ID "XXX"` | The specified `cart_id` value does not exist in the `quote_id_mask` database table.
`Could not find a product with SKU "YYY"` | A product with the SKU specified in the `data`.`sku` argument does not exist.
`Required parameter "cart_id" is missing` | The mutation does not contain a `cart_id` argument.
`Required parameter "cart_items" is missing` | The `cart_items` argument is empty or is not of type `array`.
`Please specify product link(s).` | You tried to add a downloadable product in which the `Links can be purchased separately` option is enabled, but you did not specify individual product links.
