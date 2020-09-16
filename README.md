# apis_delfometrics
Here are the documentation of the different apis that connect the delfometrics backend

## Production

### one_items_features

works as a rest api in php with the aim of working on the declaration of a free market item

#### API design readme

1. Read one_items_features

    This route gets a specific one_items_features by its item_id.
    - Request
        ```bash
        GET https://one-item-features.azurewebsites.net/www/test?item_id=:item_id 
        example -> https://one-item-features.azurewebsites.net/www/test?item_id=MLM647635498
        ```
    
    - Possible returns

        Code | Answer
        ------------ | -------------
        `200 (OK)` | {"Response":{}}
        `200 (Not item_id exist)` | {"Response":{null}}
        
### item_per_day_sells

connect to machine_learning production algoritms obtein item_per_day_sells prediction and its error.

#### API design readme

1. Post a one_items_features obtein item_per_day_sells

    This route Post a one_items_features and obtein item_per_day_sells
    - Request
        ```bash
        POST https://prod-20.centralus.logic.azure.com:443/workflows/bcec4a46a2d64d109c1d8a83d0b7791b/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=leNB3U_CW5gc2be7GRIfH27VgZnPqQ_MCUKBbyca1Mg 
        ```
    - Body
        ```json
        {
  "items": [
    {
      "title": "Motosierra Gasolina Leñera  52 Cc Barra De 20  Hkm5220 Husky",
      "site_id": "MLM",
      "price": 1408,
      "reputation_vendor": 5,
      "vendor_sales_completed": 1261540,
      "logistic_type": "fulfillment",
      "free_shipping": "true",
      "ranking": 1,
      "conversion": 0.0008250988468418517,
      "condition": "new",
      "catalog_product": "true",
      "video": "false",
      "accepts_mercadopago": "true",
      "tags": "brand_verified good_quality_picture good_quality_thumbnail immediate_payment cart_eligible",
      "num_pictures": 5,
      "attributes": "SWORD_LENGTH_20_\" BRAND_Husky CUTTING_DIAMETER_560_mm ENGINE_DISPLACEMENT_52_cc FUEL_TYPES_Nafta GTIN_7503025059068 ITEM_CONDITION_Nuevo MODEL_HKM5220 PACKAGE_HEIGHT_30_cm PACKAGE_LENGTH_45_cm PACKAGE_WEIGHT_5500_g PACKAGE_WIDTH_10_cm POWER_2.5_hp RECOMMENDED_USES_Jardinería SELLER_SKU_7503025059068 TANK_CAPACITY_550_mL WEIGHT_7.5_kg",
      "reviews_average": 4.2,
      "reviews_total": 88,
      "official_store": "false",
      "deal_ids": "true",
      "warranty": "true",
      "listing_type_id": "gold_special"
    }
  ],
  "category_id": "MLM158032"
}
        ```
    - Possible returns

        Code | Answer
        ------------ | -------------
        `200 (OK)` | {} 
        `500 (Invalid format)` | `"respuesta-error":`
        `500 (Not category_id in DB)` | `"respuesta-error": "No se encuentra esa categoria en base de datos, posteriormente la adicionaremos en la base de datos"`
        
### item_per_day_weight

search item_per_day_weight of sales in a in a specific category of meli

#### API design readme

1. Read item_per_day_weight

    This route gets a day_weight in the present year dynamically by its category_id.
    - Request
        ```bash
        GET https://productionalpha.azurewebsites.net/api/day_visits_weight?code=ZH7nZn8a4MoC5CrafyAdLZkniR35nkmq2hNo2wsY8456dxdxFygJbQ==&category_id=:category_id 
        example -> https://productionalpha.azurewebsites.net/api/day_visits_weight?code=ZH7nZn8a4MoC5CrafyAdLZkniR35nkmq2hNo2wsY8456dxdxFygJbQ==&category_id=MLM158032
        ```
    
    - Possible returns

        Code | Answer
        ------------ | -------------
        `200 (OK)` | {}
        `200 (Not category_id in DB)` | empty
        
### one_item_catalog

Currently mercadolibre handles its articles in two ways 

1- as item_id. 
2- it is like product catalog product_id.

this api gets the item_id corresponding to the product_id. With the aim of putting everything in item_id format.

#### API design readme

1. Read one_item_catalog

    This route gets item_id corresponding to the product_id.
    - Request
        ```bash
        GET https://productionalpha.azurewebsites.net/api/one_item_catalog?code=RXzrvm59PH31jrMaa5KRe0lA/rro4CrMQ385FEQec2Qr1kM1V9mGEQ==&catalog_product=:product_id 
        example -> https://productionalpha.azurewebsites.net/api/one_item_catalog?code=RXzrvm59PH31jrMaa5KRe0lA/rro4CrMQ385FEQec2Qr1kM1V9mGEQ==&catalog_product=MLM14186128
        ```
    
    - Possible returns

        Code | Answer
        ------------ | -------------
        `200 (OK)` | {"response": item_id}
        `500 (Not product_id exist)` | error - 5 min loop empty result

