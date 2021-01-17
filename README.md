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
        `400 (Invalid format)` | `"error":`
        `404 (Not category_id in DB)` | `"error": "No se encuentra esa categoria en base de datos, posteriormente la adicionaremos en la base de datos"`
        
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
        ```
    
    - Possible returns

        Code | Answer
        ------------ | -------------
        `200 (OK)` | {"response": item_id}
        `500 (Not product_id exist)` | error - 5 min loop empty result

### time_category_training

Returns the approximate enter time for a category, this calculation is based on multiplying the number of items in the category by a value of how long it takes for items taken as a training average in previous categories

#### API design readme

1. Read time_category_training

    This route gets a string corresponding to the time in seconds.
    - Request
        ```bash
        POST https://mlconexionmeli.azurewebsites.net/api/time_category_training
        ```
    - Body
        ```json
        {
            "category_id": "MLM2354"
        }
        ```
    - Possible returns

        Code | Answer
        ------------ | -------------
        `200 (OK)` | {time_inseconds_string}
        `500 (error)` | error - 5 min loop empty result
        
## ml_training

### dinamic-ml-categories-training

To start with, it has a category_id from mercadolibre (currently from Mexico it is only allowed) this is in charge of training it and having the data available so that this category can be used in production:

#### API design readme

1. Read dinamic-ml-categories-training

    This route gets a json corresponding to the commitId with the order of training (return a anwser without finish the entire process, only to show that it start).
    - Request
        ```bash
        POST https://prod-09.centralus.logic.azure.com:443/workflows/bb8b6cd07e8b4f2ebf6e436d8baee782/triggers/request/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Frequest%2Frun&sv=1.0&sig=6KiSJiRLYathVAJYJHv_zdv_Ckin2QXaSKL4Qp2lXTk
        ```
    - Body
        ```json
        {
            "input_category_id": "mlm194037"
        }
        ```
    - Possible returns

        Code | Answer
        ------------ | -------------
        `200 (OK)` | {"category_id": category_id, "time_to_train": time_to_train_seconds}
        `500 (error)` | error - 5 min loop empty result

## API design Postman

You can import Api design on Postman with delfometrics-api-production.postman_collection.json file: 

1. Now open Postman and click Import.
2. Select the JSON file. Once the selection is complete, you can see that the JSON file gets imported as a Postman collection in the application.
