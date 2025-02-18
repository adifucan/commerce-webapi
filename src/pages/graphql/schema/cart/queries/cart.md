---
title: cart query | Commerce Web APIs
---

# cart query

Use the `cart` query to retrieve information about a particular cart.

Cart functionality is defined in the `Quote` module. A Quote represents the contents of a customer's shopping cart. It is responsible for performing tasks such as:

*  Tracking each item in the cart, including the quantity and base cost
*  Determining estimated shipping costs
*  Calculating subtotals, computing additional costs, applying coupons, and determining the payment method

## Syntax

`{cart(cart_id: String!) {Cart}}`

## Sample queries

### Cart ready for checkout

The following query shows the status of a cart that is ready to be converted into an order.

**Request:**

```graphql
{
  cart(cart_id: "CYmiiQRjPVc2gJUc5r7IsBmwegVIFO43") {
    email
    billing_address {
      city
      country {
        code
        label
      }
      firstname
      lastname
      postcode
      region {
        code
        label
      }
      street
      telephone
    }
    shipping_addresses {
      firstname
      lastname
      street
      city
      region {
        code
        label
      }
      country {
        code
        label
      }
      telephone
      available_shipping_methods {
        amount {
          currency
          value
        }
        available
        carrier_code
        carrier_title
        error_message
        method_code
        method_title
        price_excl_tax {
          value
          currency
        }
        price_incl_tax {
          value
          currency
        }
      }
      selected_shipping_method {
        amount {
          value
          currency
        }
        carrier_code
        carrier_title
        method_code
        method_title
      }
    }
    items {
      id
      product {
        name
        sku
      }
      quantity
      errors {
        code
        message
      }
    }
    available_payment_methods {
      code
      title
    }
    selected_payment_method {
      code
      title
    }
    applied_coupons {
      code
    }
    prices {
      grand_total {
        value
        currency
      }
    }
  }
}
```

**Response:**

```json
{
  "data": {
    "cart": {
      "email": "roni_cost@example.com",
      "billing_address": {
        "city": "Calder",
        "country": {
          "code": "US",
          "label": "US"
        },
        "firstname": "Veronica",
        "lastname": "Costello",
        "postcode": "49628-7978",
        "region": {
          "code": "MI",
          "label": "Michigan"
        },
        "street": [
          "6146 Honey Bluff Parkway"
        ],
        "telephone": "(555) 229-3326"
      },
      "shipping_addresses": [
        {
          "firstname": "Veronica",
          "lastname": "Costello",
          "street": [
            "6146 Honey Bluff Parkway"
          ],
          "city": "Calder",
          "region": {
            "code": "MI",
            "label": "Michigan"
          },
          "country": {
            "code": "US",
            "label": "US"
          },
          "telephone": "(555) 229-3326",
          "available_shipping_methods": [
            {
              "amount": {
                "currency": "USD",
                "value": 10
              },
              "available": true,
              "carrier_code": "tablerate",
              "carrier_title": "Best Way",
              "error_message": "",
              "method_code": "bestway",
              "method_title": "Table Rate",
              "price_excl_tax": {
                "value": 10,
                "currency": "USD"
              },
              "price_incl_tax": {
                "value": 10,
                "currency": "USD"
              }
            },
            {
              "amount": {
                "currency": "USD",
                "value": 15
              },
              "available": true,
              "carrier_code": "flatrate",
              "carrier_title": "Flat Rate",
              "error_message": "",
              "method_code": "flatrate",
              "method_title": "Fixed",
              "price_excl_tax": {
                "value": 15,
                "currency": "USD"
              },
              "price_incl_tax": {
                "value": 15,
                "currency": "USD"
              }
            }
          ],
          "selected_shipping_method": {
            "amount": {
              "value": 10,
              "currency": "USD"
            },
            "carrier_code": "tablerate",
            "carrier_title": "Best Way",
            "method_code": "bestway",
            "method_title": "Table Rate"
          }
        }
      ],
      "items": [
        {
          "uid": "Mg==",
          "product": {
            "name": "Strive Shoulder Pack",
            "sku": "24-MB04"
          },
          "quantity": 2
        },
        {
          "uid": "17",
          "product": {
            "name": "Savvy Shoulder Tote",
            "sku": "24-WB05"
          },
          "quantity": 1
        }
      ],
      "available_payment_methods": [
        {
          "code": "braintree_cc_vault",
          "title": "Stored Cards (Braintree)"
        },
        {
          "code": "braintree",
          "title": "Credit Card (Braintree)"
        },
        {
          "code": "checkmo",
          "title": "Check / Money order"
        }
      ],
      "selected_payment_method": {
        "code": "checkmo",
        "title": "Check / Money order"
      },
      "applied_coupons": null,
      "prices": {
        "grand_total": {
          "value": 105.26,
          "currency": "USD"
        }
      }
    }
  }
}
```

### Cart discounts

In this query, the **Buy 3 tee shirts and get the 4th free** cart price rule from the sample data is active. This rule was modified slightly to add the label `3T1free`.  (If a cart price rule does not have a label, the application returns a default label of `Discount`.) A custom rule in which the customer saves 10% on the order by applying a discount code is also in effect.

The `3T1free` rule is applied first, and the application returns the price of a single shirt, $29, as the discount. The application then applies a 10% discount to the remaining total of the products in the cart.

If other promotions or price adjustments are applied to the cart through either store credit or gift cards, these are reflected under the `discounts` object with the appropriate label.

**Request:**

```graphql
{
  cart(cart_id: "v7jYJUjvPeHbdMJRcOfZIeQhs2Xc2ZKT") {
    email
    items {
      uid
      prices {
        total_item_discount {
          value
        }
        price {
          value
        }
        discounts {
          label
          amount {
            value
          }
        }
      }
      product {
        name
        sku
      }
      quantity
    }
    applied_coupons {
      code
    }
    prices {
      discounts {
        amount {
          value
        }
        label
      }
      grand_total {
        value
      }
    }
  }
}
```

**Response:**

```json
{
  "data": {
    "cart": {
      "email": "roni_cost@example.com",
      "items": [
        {
          "uid": "MjY=",
          "prices": {
            "total_item_discount": {
              "value": 37.7
            },
            "price": {
              "value": 29
            },
            "discounts": [
              {
                "label": "Discount (3T1free, 10% Off for New Customers)",
                "amount": {
                  "value": 37.7
                }
              },
              {
                "label": "Gift Card",
                "amount": {
                  "value": 0.1
                }
              },
              {
                "label": "Store Credit",
                "amount": {
                  "value": 0.1
                }
              }
            ]
          },
          "product": {
            "name": "Elisa EverCool&trade; Tee",
            "sku": "WS06"
          },
          "quantity": 4
        }
      ],
      "applied_coupons": [
        {
          "code": "NEW"
        }
      ],
      "prices": {
        "discounts": [
          {
            "amount": {
              "value": 29
            },
            "label": "3T1free"
          },
          {
            "amount": {
              "value": 8.7
            },
            "label": "10% Off for New Customers"
          }
        ],
        "grand_total": {
          "value": 84.76
        }
      }
    }
  }
}
```

### Tier price example

In the following example, tier prices has been established for product `24-UG01` and `24-UG05`, as shown in the following table:

Product | Quantity | Fixed/Discount | Amount
--- | --- | --- | --- |
24-UG01 | 5 | Discount | 5%
24-UG01 | 10 | Discount | 10%
24-UG01 | 15 | Discount | 15%
24-UG05 | 5 | Fixed | $16
24-UG05 | 10 | Fixed | $11

The cart in the example contains 12 units of `24-UG05` and 8 units of `24-UG-01`, so the price of `24-UG05` is $11, and the price of `24-UG01` is $18.05 (5% off).

**Request:**

```graphql
query {
  cart(cart_id: "v7jYJUjvPeHbdMJRcOfZIeQhs2Xc2ZKT"){
    items {
      uid
      quantity
      product{
        name
        sku
        price_tiers {
          quantity
          final_price {
            value
          }
          discount {
            amount_off
            percent_off
          }
        }
      }
      prices{
        price{
          value
        }
      }
    }
    prices {
      discounts {
        label
        amount {
          value
        }
      }
      subtotal_excluding_tax {
        value
      }
      applied_taxes {
        label
        amount {
          value
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
    "cart": {
      "items": [
        {
          "uid": "NjU=",
          "quantity": 12,
          "product": {
            "name": "Go-Get'r Pushup Grips",
            "sku": "24-UG05",
            "price_tiers": [
              {
                "quantity": 5,
                "final_price": {
                  "value": 16
                },
                "discount": {
                  "amount_off": 3,
                  "percent_off": 15.79
                }
              },
              {
                "quantity": 10,
                "final_price": {
                  "value": 11
                },
                "discount": {
                  "amount_off": 8,
                  "percent_off": 42.11
                }
              }
            ]
          },
          "prices": {
            "price": {
              "value": 11
            }
          }
        },
        {
          "uid": "NjY=",
          "quantity": 8,
          "product": {
            "name": "Quest Lumaflex&trade; Band",
            "sku": "24-UG01",
            "price_tiers": [
              {
                "quantity": 5,
                "final_price": {
                  "value": 18.05
                },
                "discount": {
                  "amount_off": 0.95,
                  "percent_off": 5
                }
              },
              {
                "quantity": 10,
                "final_price": {
                  "value": 17.1
                },
                "discount": {
                  "amount_off": 1.9,
                  "percent_off": 10
                }
              },
              {
                "quantity": 15,
                "final_price": {
                  "value": 16.15
                },
                "discount": {
                  "amount_off": 2.85,
                  "percent_off": 15
                }
              }
            ]
          },
          "prices": {
            "price": {
              "value": 18.05
            }
          }
        }
      ],
      "prices": {
        "discounts": [
          {
            "label": "200",
            "amount": {
              "value": 55.28
            }
          }
        ],
        "subtotal_excluding_tax": {
          "value": 276.4
        },
        "applied_taxes": [
          {
            "label": "US-MI-*-Rate 1",
            "amount": {
              "value": 18.24
            }
          }
        ]
      }
    }
  }
}
```

## Input attributes

Attribute |  Data Type | Description
--- | --- | ---
`cart_id` | String! | A 32-character string that is created when you [create a cart](../mutations/create-empty-cart.md)

## Output attributes

The top-level `Cart` object is listed first. All interfaces and child objects are listed in alphabetical order.

### Cart object

The `Cart` object can contain the following attributes.

import CartObject from '/src/_includes/graphql/cart-object-24.md'

<CartObject />

### AppliedCoupon object

The `AppliedCoupon` object must contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`code` | String! | The coupon code applied to the order

### AppliedGiftCard object

The `AppliedGiftCard` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`applied_balance` | Money | Applied balance to the current cart
`code` | String | The gift card code applied to the order
`current_balance` | Money | Current balance remaining on the gift card
`expiration_date` | String | Gift card expiration date

### AppliedStoreCredit object

The `AppliedStoreCredit` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`applied_balance` | Money | The amount of store credit applied to the current cart
`current_balance` | Money | The customer's store credit balance before applying store credit to the cart
`enabled` | Boolean | Indicates whether store credits are enabled. If the feature is disabled, then the current balance will not be returned

### AvailablePaymentMethod object

The `AvailablePaymentMethod` object must contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`code` |  String! | The payment method code
`title` | String! | The payment method title

### AvailableShippingMethod object

The `AvailableShippingMethod` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money! | The cost of shipping using this shipping method
`available` | Boolean! | Indicates whether this shipping method can be applied to the cart
`base_amount` | Money | Deprecated. This attribute is not applicable for GraphQL
`carrier_code` | String! | A string that identifies a commercial carrier or an offline shipping method
`carrier_title` | String! | The label for the carrier code
`error_message` | String | Describes an error condition
`method_code` | String | A shipping method code associated with a carrier. Could be null if method is not available
`method_title` | String | The label for the method code. Could be null if method is not available
`price_excl_tax` | Money! | The cost of shipping using this shipping method, excluding tax
`price_incl_tax` | Money! | The cost of shipping using this shipping method, excluding tax

### BillingCartAddress object

The `BillingCartAddress` object implements [`CartAddressInterface`](#CartAddressInterface). It does not define any additional attributes.

### CartAddressCountry object

The `CartAddressCountry` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`code` | String! | The country code
`label` | String! | The display label for the country

### CartAddressInterface

The `CartAddressInterface` contains the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`city` | String! | The city specified for the billing address
`company` | String | The company specified for the billing address
`country` | [CartAddressCountry!](#cartaddresscountry-object) | The country code and label for the billing address
`customer_notes` | String | Comments made to the customer that accompanies the order
`firstname` | String! | The customer's first name
`lastname` | String! | The customer's last name
`postcode` | String | The postal code for the billing address
`region` | [CartAddressRegion](#cartaddressregion-object) | An object containing the region label and code
`street` | [String!]! | The street for the billing address
`telephone` | String | The telephone number for the billing address

### CartAddressRegion object

The `CartAddressRegion` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`code` | String | The state or province code
`label` | String | The display label for the region
`region_id` | Int | The unique ID for a pre-defined region

### CartDiscount object

The `CartDiscount` object must contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money | The amount of all discounts applied to the cart
`label` | [String!]! | A concatenated list of strings that describe each applied discount

### CartItemInterface

The `CartItemInterface` has the following implementations:

*  BundleCartItem
*  ConfigurableCartItem
*  DownloadableCartItem
*  GiftCardCartItem
*  SimpleCartItem
*  VirtualCartItem

See [`CartItemInterface`](../interfaces/cart-item.md) for details.

### CartItemQuantity object

The `CartItemQuantity` data type has been deprecated. Use the `cart_items_v2` attribute with the [`CartItemInterface`](#cartiteminterface) instead.

Attribute |  Data Type | Description
--- | --- | ---
`cart_item_id` | Int! | Deprecated. Use `CartItemInterface.id` instead
`quantity` | Float! | Deprecated. Use `CartItemInterface.quantity` instead

### CartPrices object

The `CartPrices` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`applied_taxes` | [[CartTaxItem]](#carttaxitem-object) | An array containing the names and amounts of taxes applied to the item
`discount` | [CartDiscount](#cartdiscount-object) | Deprecated. Use `discounts` instead
`discounts` | [[Discount]](#discount-object) | An array containing all discounts applied to the cart
`gift_options` | [GiftOptionsPrices](#giftoptionsprices-object) | The list of prices for the selected gift options
`grand_total` | Money | The total, including discounts, taxes, shipping, and other fees
`subtotal_excluding_tax` | Money | Subtotal without taxes
`subtotal_including_tax` | Money | Subtotal with taxes
`subtotal_with_discount_excluding_tax` | Money | Subtotal with any discounts applied, but not taxes

### CartTaxItem object

The `CartTaxItem` object must contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money! | The amount of tax applied to the item
`label` | String! | The description of the tax

### Discount object

A discount can be applied to the cart as a whole or to an item.

If a cart rule does not have a label, the application uses `Discount` as the default label.

The `Discount` object must contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money! | The amount of the discount applied to the cart
`label` | String! | The description of the discount

### GiftMessage object

import GiftMessage from '/src/_includes/graphql/gift-message.md'

<GiftMessage />

### GiftOptionsPrices object

The `GiftOptionsPrices` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`gift_wrapping_for_items` | Money | The price of the gift wrapping for all individual order items
`gift_wrapping_for_order` | Money | The price of the gift wrapping for the whole order
`printed_card` | Money | The price of the printed card

### GiftWrapping object

import GiftWrapping from '/src/_includes/graphql/gift-wrapping.md'

<GiftWrapping />

### RewardPointsAmount

The `RewardPointsAmount` object must contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`points` | Float! | The amount of reward points, in points
`money` | Money! | The amount of reward points, in the store's currency

### SelectedPaymentMethod object

The `SelectedPaymentMethod` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`code` | String! | The payment method code
`purchase_order_number` | String | The purchase order number
`title` | String! | The payment method title

### SelectedShippingMethod object

The `SelectedShippingMethod` object can contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money! | The cost of shipping using this shipping method
`base_amount` | Money | Deprecated. This attribute is not applicable for GraphQL
`carrier_code` | String! | A string that identifies a commercial carrier or an offline shipping method
`carrier_title` | String! | The label for the carrier code
`method_code` | String! | A shipping method code associated with a carrier
`method_title` | String! | The label for the method code

### ShippingCartAddress object

The `ShippingCartAddress` object implements [`CartAddressInterface`](#CartAddressInterface). It can also contain the following attributes.

Attribute |  Data Type | Description
--- | --- | ---
`available_shipping_methods` | [[AvailableShippingMethod]](#availableshippingmethod-object) | An array that lists the shipping methods that can be applied to the cart
`cart_items` | [[CartItemQuantity]](#cartitemquantity-object) | Deprecated. Use `cart_items_v2` instead
`cart_items_v2` | [CartItemInterface] | An array that lists the items in the cart
`items_weight` | Float | Deprecated. This attribute is not applicable for GraphQL
`pickup_location_code` | String | The code of the in-store pickup location where the customer will receive the order
`selected_shipping_method` | [SelectedShippingMethod](#selectedshippingmethod-object) | An object that describes the selected shipping method

## Related topics

*  [createEmptyCart mutation](../mutations/create-empty-cart.md)
*  [addSimpleProductsToCart mutation](../mutations/add-simple-products.md)
*  [setShippingAddressesOnCart mutation](../mutations/set-shipping-address.md)
*  [setShippingMethodsOnCart mutation](../mutations/set-shipping-method.md)
*  [setBillingAddressOnCart mutation](../mutations/set-billing-address.md)
*  [setPaymentMethodOnCart mutation](../../cart/mutations/set-payment-method.md)

## Errors

Error | Description
--- | ---
`Could not find a cart with ID \"xxxxx\"` | The ID provided in the `cart_id` field is invalid or the cart does not exist for the customer.
`The cart isn't active` | The cart with the specified cart ID is unavailable, because the items have been purchased and the cart ID becomes inactive.
`Field cart.cart_id of required type String! was not provided` | The value specified in the `cart.cart_id` argument is empty.
