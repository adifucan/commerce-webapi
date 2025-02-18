---
title: ProductInterface attributes | Commerce Web APIs
---

# ProductInterface attributes

Any type that implements `ProductInterface` contains all the base attributes necessary for the frontend of the product model.
The `items` that are returned in a `ProductInterface` array can also contain attributes from resources external to the `CatalogGraphQl` module:

-  Custom and extension attributes defined in any attribute set
-  The attribute is defined in the [PhysicalProductInterface](#physicalproductinterface) or [CustomizableOptionInterface](customizable-option.md)
-  Product types that define their own implementation of `ProductInterface` including:
   -  [SimpleProduct](types/simple.md)
   -  [BundleProduct](types/bundle.md)
   -  [ConfigurableProduct](types/configurable.md)
   -  [DownloadableProduct](types/downloadable.md)
   -  [GiftCardProduct](types/gift-card.md)
   -  [GroupedProduct](types/grouped.md)
   -  [VirtualProduct](types/virtual.md)

## ProductInterface attributes

The following table defines the `ProductInterface` attributes and objects.

Attribute | Data type | Description
--- | --- | ---
`attribute_set_id` | Int | Deprecated. This field should not be used on the storefront. The attribute set assigned to the product
`canonical_url` | String  | The relative canonical URL. This value is returned only if the system setting **Use Canonical Link Meta Tag For Products** is enabled
`categories` | [[CategoryInterface]](category.md) | The categories assigned to the product. See [CategoryInterface attributes](category.md) for more information
`country_of_manufacture` | String | The product's country of origin
`created_at` | String | Deprecated. This field should not be used on the storefront. Timestamp indicating when the product was created
`crosssell_products` | [ProductInterface] | An array of cross-sell products
`custom_attributes` | [ProductInterface] | An array of attribute names for a product. PWA Studio only.
`description` | ComplexTextValue | An object that contains detailed information about the product. The object can include simple HTML tags
`gift_message_available` | String | Indicates whether a gift message is available
`id` | Int | Deprecated. Use `uid` instead. The ID number assigned to the product
`image` | [ProductImage](#productimage-object) | An object that contains the URL and label for the main image on the product page
`is_returnable` | String | Indicates whether the product can be returned. This attribute is defined in the `RmaGraphQl` module.
`manufacturer` | Int | A number representing the product's manufacturer
`media_gallery` | [[MediaGalleryInterface]](#mediagalleryinterface) | An array of media gallery objects
`media_gallery_entries` | [MediaGalleryEntry] | Deprecated. Use `media_gallery` instead
`meta_description` | String | A brief overview of the product for search results listings, maximum 255 characters
`meta_keyword` | String | A comma-separated list of keywords that are visible only to search engines
`meta_title` | String | A string that is displayed in the title bar and tab of the browser and in search results lists
`name` | String | The product name. Customers use this name to identify the product.
`new_from_date` | String | Deprecated. This field should not be used on the storefront. The beginning date for new product listings, and determines if the product is featured as a new product
`new_to_date` | String | Deprecated. This field should not be used on the storefront. The end date for new product listings
`only_x_left_in_stock` | Float | The "Only X left Threshold" assigned to the product. This attribute is defined in the `InventoryGraphQl` module.
`options_container` | String | If the product has multiple options, determines where they appear on the product page
`price` | ProductPrices | Deprecated. Use `price_range` instead
`price_range` | [PriceRange!](#pricerange-object) |  A `PriceRange` object, indicating the range of prices for the product
`price_tiers` | [TierPrice] | An array of `TierPrice` objects
`product_links` | [ProductLinksInterface] | An array of [ProductLinks](#productlinks-object) objects
`rating_summary` | Float! | The average of all the ratings given to the product
`related_products` | [ProductInterface] | An array of related products
`review_count` | Int! | The total count of all the reviews given to the product
`reviews(pageSize: Int = 20 currentPage: Int = 1)` | [ProductReviews!](#productreviews-object) | The list of reviews of the product
`short_description` | ComplexTextValue | An object that contains a short description of the product. Its use depends on the store's theme. The object can include simple HTML tags
`sku` | String | A number or code assigned to a product to identify the product, options, price, and manufacturer
`small_image` | [ProductImage](#productimage-object) | An object that contains the URL and label for the small image used on catalog pages
`special_from_date` | String | Deprecated. This field should not be used on the storefront. The beginning date that a product has a special price
`special_price` | Float |  The discounted price of the product
`special_to_date` | String | The end date that a product has a special price
`staged` | Boolean! | Indicates whether the product is staged for a future campaign
`stock_status` | ProductStockStatus | The status of the stock. `ProductStockStatus` is an enumeration that can have the value of `IN_STOCK` or `OUT_OF_STOCK`. This attribute is defined in the `InventoryGraphQl` module.
`swatch_image` | String | The file name of a swatch image. This attribute is defined in the `SwatchesGraphQl` module.
`tax_class_id` | Int | An ID assigned to a tax class. This attribute is defined in the `TaxGraphQl` module.
`thumbnail` | [ProductImage](#productimage-object) | An object that contains the URL and label for the product's thumbnail image
`tier_price` | Float | Deprecated. Use `price_tiers` instead
`tier_prices` | [ProductTierPrices] | Deprecated. Use `price_tiers` instead
`type_id` | String | Deprecated. Use the GraphQL `__typename` meta attribute instead
`uid` | ID! | The unique ID for objects implementing `ProductInterface`
`updated_at` | String | Deprecated. This field should not be used on the storefront. The timestamp indicating when the product was last updated
`upsell_products` | [ProductInterface] | An array of up-sell products
`url_key` | String | The part of the URL that identifies the product. This attribute is defined in the `CatalogUrlRewriteGraphQl` module
`url_path` | String | Deprecated. Use `canonical_url` instead
`url_suffix` | String | The part of the URL that is appended to the `url_key`, such as `.html`. This attribute is defined in the `CatalogUrlRewriteGraphQl` module
`url_rewrites` | [[UrlRewrite]](#urlrewrite-object) | A list of URL rewrites
`websites` | [[Website]](#website-object) | Deprecated. This attribute is not applicable for GraphQL

### ProductPrices object

<InlineAlert variant="info" slots="text" />

The `ProductPrices` object has been deprecated. Use the [`PriceRange`](#pricerange-object) object instead.

The `ProductPrices` object contains the regular price of an item, as well as its minimum and maximum prices. Only composite products, which include bundle, configurable, and grouped products, can contain a minimum and maximum price.

Attribute |  Data Type | Description
--- | --- | ---
`maximalPrice` | Price | Deprecated. Use `PriceRange.maximum_price` instead
`minimalPrice` | Price | Deprecated. Use `PriceRange.minimum_price` instead
`regularPrice` | Price | Deprecated. Use `PriceRange.maximum_price` or `PriceRange.minimum_price` instead

### PriceRange object

The `PriceRange` object defines the price range for a product. If a product only has a single price, the minimum and maximum price will be the same.

Attribute |  Data Type | Description
--- | --- | ---
`maximum_price` | ProductPrice | The highest possible final price for a product
`minimum_price` | ProductPrice | The lowest possible final price for a product

### ProductPrice object

The `ProductPrice` object includes the regular price, final price, and the difference between those two prices.

Attribute |  Data Type | Description
--- | --- | ---
`discount` | ProductDiscount | The amount of the discount applied to the product. It represents the difference between the `final_price` and `regular_price`
`final_price`| Money! | The price of the product after applying discounts
`fixed_product_taxes` | [[FixedProductTax](#fixedproducttax-object)] | An array of fixed product taxes that either have been or can be applied to a product price
`regular_price` | Money! | The regular price of the product, without any applied discounts

### ProductDiscount object

The `ProductDiscount` object expresses the discount applied to a product as a fixed amount, such as $5, and as a percentage, such as 10%. The discount originates from special pricing or a catalog price rule.

Attribute |  Data Type | Description
--- | --- | ---
`amount_off` | Float | The actual value of the discount
`percent_off` | Float | The discount expressed as a percentage

### FixedProductTax object

Some tax jurisdictions have a fixed product tax (FPT) that must be applied to certain types of products. An example FPT is the Waste Electrical and Electronic Equipment (WEEE) tax, which is collected on some types of electronics to offset the cost of recycling.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money | The amount of the fixed product tax
`label` | String | The label assigned to the fixed product tax to be displayed on the frontend

### Price object

<InlineAlert variant="info" slots="text" />

The `Price` object has been deprecated. Use the [`ProductPrice`](#productprice-object) object instead.

The `Price` object defines the price of a product as well as any tax-related adjustments.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money | The price of the product and its currency code. See [Money object](#money-object).
`adjustments` | [PriceAdjustment] | An array of [PriceAdjustment](#priceadjustment-array) objects.

#### Money object

A `Money` object defines a monetary value, including a numeric value and a currency code.

Attribute |  Data Type | Description
--- | --- | ---
`currency` | CurrencyEnum | A three-letter currency code, such as `USD` or `EUR`.
`value` | Float | The price of the product

#### PriceAdjustment array

<InlineAlert variant="info" slots="text" />

The `PriceAdjustment` object has been deprecated. In cases where the value for the `code` attribute was `WEEE`, use `fixed_product_taxes.label` instead. If the value was `tax` or `weee_tax`, the taxes will be included or excluded as part of the price in the `ProductPrice` or `FixedProductTax` object, respectively.

The `PriceAdjustment` object defines the amount of money to apply as an adjustment, the type of adjustment to apply, and whether the item is included or excluded from the adjustment.

Attribute |  Data Type | Description
--- | --- | ---
`amount` | Money | The amount of the price adjustment and its currency code. See [Money object](#money-object).
`code` | PriceAdjustmentCodesEnum | One of `tax`, `weee`, or `weee_tax`.
`description` | PriceAdjustmentDescriptionEnum | Indicates whether the entity described by the code attribute is included or excluded from the adjustment.

#### ProductReviews object

`ProductReviews` contains an array of reviews written about the product.

Attribute |  Data Type | Description
--- | --- | ---
`items` | [[ProductReview]](#productreview-object)! | An array of product reviews
`page_info` | [SearchResultPageInfo!](../queries/products.md#searchresultpageinfo-attributes) | Metadata for pagination rendering

#### ProductReview object

import ProductReview from '/src/_includes/graphql/product-review.md'

<ProductReview />

#### ProductLinks object

`ProductLinks` contains information about linked products, including the link type and product type of each item.

Attribute | Type | Description
--- | --- | ---
`link_type` | String | One of `related`, `associated`, `upsell`, or `crosssell`.
`linked_product_sku` | String | The SKU of the linked product
`linked_product_type` | String | The type of linked product (`simple`, `virtual`, `bundle`, `downloadable`,`grouped`, `configurable`)
`position` | Int | The position within the list of product links
`sku` | String | The identifier of the linked product

### MediaGalleryInterface

The `MediaGalleryInterface` contains basic information about a product image or video.

Attribute | Type | Description
--- | --- | ---
`disabled` | Boolean | Indicates whether the media item is hidden from view
`label` | String | The label for the product image or video
`position` | Int | The media item's position after it has been sorted
`url` | String | The URL for the product image or video

### ProductImage object

`ProductImage` implements [`MediaGalleryInterface`](#mediagalleryinterface), which contains information about an image's URL and label.

### ProductVideo object

`ProductVideo` implements [`MediaGalleryInterface`](#mediagalleryinterface) and contains information about a product video.

Attribute | Type | Description
--- | --- | ---
`video_content` | ProductMediaGalleryEntriesVideoContent | Contains a [ProductMediaGalleryEntriesVideoContent](#productmediagalleryentriesvideocontent-object) object

### MediaGalleryEntry object

`MediaGalleryEntry` defines characteristics about images and videos associated with a specific product.

Attribute | Type | Description
--- | --- | ---
`content` | ProductMediaGalleryEntriesContent | Contains a [ProductMediaGalleryEntriesContent](#productmediagalleryentriescontent-object) object
`disabled` | Boolean | Whether the image is hidden from view
`file` | String | The path of the image on the server
`id` | Int | Deprecated. Use `uid` instead. The identifier assigned to the object
`label` | String | The "alt" text displayed on the UI when the user points to the image
`media_type` | String | `image` or `video`
`position` | Int | The media item's position after it has been sorted
`types` | [String] | Array of image types. It can have the following values: `image`, `small_image`, `thumbnail`
`uid` | ID! | The unique ID for `MediaGalleryEntry` objects
`video_content` | ProductMediaGalleryEntriesVideoContent | Contains a [ProductMediaGalleryEntriesVideoContent](#productmediagalleryentriesvideocontent-object) object

#### ProductMediaGalleryEntriesContent object

`ProductMediaGalleryEntriesContent` contains an image in base64 format and basic information about the image.

Attribute | Type | Description
--- | --- | ---
`base64_encoded_data` | String | The image in base64 format
`name` | String | The file name of the image
`type` | String | The MIME type of the file, such as `image/png`

#### ProductMediaGalleryEntriesVideoContent object

`ProductMediaGalleryEntriesVideoContent` contains a link to a video file and basic information about the video.

Attribute | Type | Description
--- | --- | ---
`media_type` | String | Must be `external-video`
`video_description` | String | A description of the video
`video_metadata` | String | Optional data about the video
`video_provider` | String | Optionally describes the video source
`video_title` | String | The title of the video
`video_url` | String | The URL to the video

### ProductTierPrices object

<InlineAlert variant="info" slots="text" />

The `ProductTierPrices` object and all of its attributes have been deprecated. Use [`TierPrice`](#tierprice-object) instead.

The `ProductTierPrices` object defines a tier price, which is a quantity discount offered to a specific customer group.

Attribute | Type | Description
--- | --- | ---
`customer_group_id` | Int | Deprecated. This attribute is not applicable for GraphQL
`percentage_value` | Float | Deprecated. Use `TierPrice.discount` instead
`qty` | Float | Deprecated. Use `TierPrice.quantity` instead
`value` | Float | Deprecated. Use `TierPrice.final_price` instead
`website_id` | Int | Deprecated. This attribute is not applicable for GraphQL

### TierPrice object

The `TierPrice` object defines a tier price, which is a price based on the quantity purchased.

Attribute | Type | Description
--- | --- | ---
`discount` | ProductDiscount | The price discount applied to this tier
`final_price`| Money! | The price of the product at this tier
`quantity` | Float | The minimum number of items that must be purchased to qualify for this price tier

### Website object

<InlineAlert variant="info" slots="text" />

The `Website` object has been deprecated because it is not applicable for GraphQL.

Use the `Website` attributes to retrieve information about the website's configuration, which includes the website name, website code, and default group ID. The `Website` object is defined in the StoreGraphQl module.

Attribute |  Data Type | Description
--- | --- | ---
`code` | String | A code assigned to the website to identify it
`default_group_id` | String | The default group ID that the website has
`id` | Integer | The ID number assigned to the store
`is_default` | Boolean | Indicates whether this is the default website
`name` | String | The website name. Websites use this name to identify it easier
`sort_order` | Integer | The attribute to use for sorting websites

### UrlRewrite object

The `products` query can request details about the `UrlRewrite` object. This object is defined in the UrlRewriteGraphQl module.

Attribute | Type | Description
--- | --- | ---
`parameters` | [[`HttpQueryParameter`]](#httpqueryparameter-object) | An array of target path parameters
`url` | String | The request URL

### HTTPQueryParameter object

The `HttpQueryParameter` object provides details about target path parameters.

Attribute | Type | Description
--- | --- | ---
`name` | String | The parameter name, such as `id`
`value` | String | The value assigned to the parameter

## PhysicalProductInterface

`PhysicalProductInterface`defines the weight of all tangible products.

Attribute | Type | Description
--- | --- | ---
`weight` | Float | The weight of the item, in units defined by the store
