---
title: changeCustomerPassword mutation | Commerce Web APIs
---

# changeCustomerPassword mutation

Use the `changeCustomerPassword` mutation to change the password for the logged-in customer.

To return or modify information about a customer, we recommend you use customer tokens in the header of your GraphQL calls. However, you also can use [session authentication](https://developer.adobe.com/commerce/webapi/get-started/authentication/gs-authentication-session).

## Syntax

`mutation: {changeCustomerPassword(currentPassword: String! newPassword: String!) {Customer}}`

## Example usage

The following call updates the customer's password.

**Request:**

```graphql
mutation {
  changeCustomerPassword(
    currentPassword: "roni_cost3@example.com"
    newPassword: "roni_cost4@example.com"
  ) {
    id
    email
  }
}
```

**Response:**

```json
{
  "data": {
    "changeCustomerPassword": {
      "id": 1,
      "email": "roni_cost@example.com"
    }
  }
}
```

## Input attributes

The `changeCustomerPassword` mutation requires the following inputs:

Attribute |  Data Type | Description
--- | --- | ---
`currentPassword` | String | The customer's current password
`newPassword` | String | The customer's new password

## Output attributes

The `changeCustomerPassword` mutation returns the `customer` object.

The following table lists the top-level attributes of the `customer` object. See the [`customer` query](../../customer/queries/customer.md) for complete details about this object.

import Customer from '/src/_includes/graphql/customer-output-24.md'

<Customer />

## Errors

Error | Description
--- | ---
`The current customer isn't authorized.` | The customer's token does not exist in the `oauth_token` table.
`Invalid login or password.` | The password specified in the `currentPassword` argument is not valid.
`Specify the "currentPassword" value.` | The password specified in the `currentPassword` argument is empty.
`Specify the "newPassword" value.` | The password specified in the `newPassword` argument is empty.
`The account is locked.` | The customer's password cannot be changed because the account is locked.

## Related topics

*  [customer query](../queries/customer.md)
*  [createCustomer mutation](create.md)
*  [updateCustomer mutation](update.md)
