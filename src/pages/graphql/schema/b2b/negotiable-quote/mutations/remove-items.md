---
title: removeNegotiableQuoteItems mutation | Commerce Web APIs
edition: b2b
---

# removeNegotiableQuoteItems mutation

The `removeNegotiableQuoteItems` mutation removes the specified products from a negotiable quote.

<InlineAlert variant="info" slots="text" />

Removing the last product from a negotiable quote causes the quote to be in a terminal state. You cannot add products to the quote, nor can you modify the quantity of any items. You can only [close](close.md) or [delete](delete.md) the quote.

This mutation requires a valid [customer authentication token](../../../customer/mutations/generate-token.md).

## Syntax

```graphql
    removeNegotiableQuoteItems(
        input: RemoveNegotiableQuoteItemsInput!
    ): RemoveNegotiableQuoteItemsOutput
```

## Example usage

The following example removes a product from a negotiable quote.

**Request:**

```graphql
mutation {
  removeNegotiableQuoteItems(
    input: {
      quote_uid: "xCA4wSZEHsb5QbFiKfoq5k1Dk8vIPBgb"
      quote_item_uids: ["MTc="]
    }
  ) {
    quote {
      uid
      name
      updated_at
      items {
        uid
        product {
          uid
          sku
          name
        }
        quantity
        prices {
          price {
            value
          }
        }
      }
    }
  }
}
```

**Response:**

```json
{
  "data": {
    "removeNegotiableQuoteItems": {
      "quote": {
        "uid": "xCA4wSZEHsb5QbFiKfoq5k1Dk8vIPBgb",
        "name": "April 22 request",
        "updated_at": "2021-04-23 18:21:44",
        "items": [
          {
            "uid": "MTU=",
            "product": {
              "uid": "MjA=",
              "sku": "24-UG01",
              "name": "Quest Lumaflex&trade; Band"
            },
            "quantity": 7,
            "prices": {
              "price": {
                "value": 19
              }
            }
          },
          {
            "uid": "MTY=",
            "product": {
              "uid": "MTg=",
              "sku": "24-UG02",
              "name": "Pursuit Lumaflex&trade; Tone Band"
            },
            "quantity": 8,
            "prices": {
              "price": {
                "value": 16
              }
            }
          }
        ]
      }
    }
  }
}
```

## Input attributes

The `RemoveNegotiableQuoteItemsInput` object contains the following attributes.

Attribute | Data Type | Description
--- | --- | ---
`quote_item_uids` | [ID!]! | An array of IDs indicating which items to remove from the negotiable quote
`quote_uid` | ID! | The unique ID of a `NegotiableQuote` object

## Output attributes

The `RemoveNegotiableQuoteItemsOutput` output object contains the following attribute.

Attribute | Data Type | Description
--- | --- | ---
`quote` | NegotiableQuote | Contains details about the negotiable quote

### NegotiableQuote attributes

import NegotiableQuote from '/src/_includes/graphql/negotiable-quote.md'

<NegotiableQuote />
