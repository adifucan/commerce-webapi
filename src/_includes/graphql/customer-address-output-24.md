The values assigned to attributes such as `firstname` and `lastname` in this object may be different from those defined in the `Customer` object.

The `CustomerAddress` output returns the following attributes:

Attribute |  Data Type | Description
--- | --- | ---
`city` | String | The city or town
`company` | String | The customer's company
`country_code` | CountryCodeEnum | The customer's country
`country_id` | String | Deprecated. Use `country_code` instead. The customer's country
`custom_attributes` | [CustomerAddressAttribute](#customeraddress-attributes) | Deprecated. Use `custom_attributesV2` instead
`custom_attributesV2` | [AttributeValueInterface](#attributevalueinterface-attributes)| Custom attributes assigned to the customer address
`customer_id` | Int | Deprecated. This attribute is not applicable for GraphQL. The ID assigned to the customer
`default_billing` | Boolean | Indicates whether the address is the default billing address
`default_shipping` | Boolean | Indicates whether the address is the default shipping address
`extension_attributes` | [CustomerAddressAttribute](#customeraddress-attributes) | Address extension attributes
`fax` | String | The fax number
`firstname` | String | The first name of the person associated with the shipping/billing address
`id` | Int | The ID assigned to the address object
`lastname` | String | The family name of the person associated with the shipping/billing address
`middlename` | String | The middle name of the person associated with the shipping/billing address
`postcode` | String | The customer's ZIP or postal code
`prefix` | String | An honorific, such as Dr., Mr., or Mrs.
`region` | [CustomerAddressRegion](#customeraddressregion-attributes) | An object that defines the customer's state or province
`region_id` | Int | The unique ID for a pre-defined region
`street` | [String] | An array of strings that define the street number and name
`suffix` | String | A value such as Sr., Jr., or III
`telephone` | String | The telephone number
`vat_id` | String | The customer's Tax/VAT number (for corporate customers)

### AttributeValueInterface attributes

import BetaNote from '/src/_includes/graphql/notes/beta.md'

<BetaNote />

The `AttributeValueInterface` contains the following attributes:

Attribute |  Data Type | Description
--- | --- | ---
`code` | String! | The attribute code
`uid` | ID! | The unique ID of an attribute value

Currently, `AttributeValueInterface` has two different implementations: `AttributeValue` and `AttributeSelectedOptions`.

In addition to the attributes described for `AttributeValueInterface`, the `AttributeValue` contains the following:

Attribute |  Data Type | Description
--- | --- | ---
`value` | String! | The attribute value

The `AttributeSelectedOptions` object contains the following attributes:

Attribute |  Data Type | Description
--- | --- | ---
`selected_options` | [AttributeSelectedOptionInterface!]! | An array containing selected options for a select or multiselect attribute

The `AttributeSelectedOptionInterface` contains the following attributes:

Attribute |  Data Type | Description
--- | --- | ---
`label` | String! | The attribute selected option label
`uid` | ID! | The unique ID of an attribute selected option
`value` | String! | The attribute selected option value

### CustomerAddressAttribute attributes

The `CustomerAddressAttribute` output data type has been deprecated. Use `custom_attributesV2` instead. It can contain the following attributes:

Attribute |  Data Type | Description
--- | --- | ---
`attribute_code` | String | Attribute code
`value` | String | Attribute value

### CustomerAddressRegion attributes

The `customerAddressRegion` output returns the following attributes:

Attribute |  Data Type | Description
--- | --- | ---
`region` | String | The state or province name
`region_code` | String | The address region code
`region_id` | Int | The unique ID for a pre-defined region
