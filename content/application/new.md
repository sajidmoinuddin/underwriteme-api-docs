## New [/application]
Creating new Application based on sent Customers and Products JSON data.

- ***customers*** `array` *(required)* - List of Customers (represented as `object`) for the Application.
    - ***referenceId*** `string` *(required)* - Temporary ID of the Customer to be used when referencing to ***livesAsssured*** for the Product and to identify them if any validation errors occur.
    - ***name*** `string` *(required)* - Customer name.
    - ***surname*** `string` *(required)* - Customer surname.
    - ***title*** `string` *(required)* - Customer title. Allowed values: `MR`, `MRS`, `MISS`, `MS`, `DR`, `REV`.
    - ***gender*** `string` *(required)* - Customer gender. Allowed values: `FEMALE`, `MALE`.
    - ***dateOfBirth*** `string` *(required)* - Customer date of birth. ISO 8601 date format (`YYYY-MM-DD`) required. Example: `1980-01-01`.
    - ***smoker*** `boolean` *(required)* - `true` if Customer smokes, `false` otherwise.
    - ***email*** `string` *(required)* - Customer email address.
    - ***occupation*** `string` *(optional)* - Customer occupation.
    - ***maritalStatus*** `string` *(required)* - Customer marital status. Allowed values: `MARRIED`, `SINGLE`, `SEPARATED`, `DIVORCED`, `WIDOWED`, `CIVIL_PARTNER`.
    - ***contactDetails*** `object` *(optional)* - Customer contact details. Not required but cannot be empty.
        - ***telephoneNumber*** `string` *(required)* - Customer main telephone number.
        - ***alternativeTelephoneNumber*** `string` *(optional)* - Customer alternative telephone number.
        - ***address*** `object` *(optional)* - Customer address details. Not required but cannot be empty.
            - ***line1*** `string` *(optional)* - Address first line.
            - ***line2*** `string` *(optional)* - Address second line.
            - ***town*** `string` *(optional)* - Address town.
            - ***county*** `string` *(optional)* - Address county.
            - ***postcode*** `string` *(required)* - Address postcode.    
- ***products*** `array` *(required)* - List of Products (represented as `object`) for the Application.
    - ***referenceId*** `string` *(required)* - Temporary ID of the Product to identify it if any validation errors occur.
    - ***type*** `string` *(required)* - Product type. Allowed values: `TERM`, `CRITICAL_ILLNESS`, `CRITICAL_ILLNESS_WITH_LIFE_COVER`, `INCOME_PROTECTION`.
    - ***coverAmount*** `number` *(required/optional)* - Product cover amount. Required if ***premium*** not specified. Does not allow decimals.
    - ***premium*** `number` *(required/optional)* - Product premium. Required if ***coverAmount*** not specified. Allows decimals.
    - ***premiumBasis*** `string` *(required/optional)* - Product premium basis. Required if ***type*** is `CRITICAL_ILLNESS`, `CRITICAL_ILLNESS_WITH_LIFE_COVER` or `INCOME_PROTECTION`. Allowed values: `GUARANTEED`, `REVIEWABLE`.
    - ***coverPeriod*** `number` *(required/optional)* - Product cover period in years. Required if ***coverUntilAge*** not specified. Does not allow decimals.
    - ***coverUntilAge*** `number` *(required/optional)* - Product cover until age in years. Required if ***coverPeriod*** not specified.  Does not allow decimals.
    - ***coverBasis*** `string` *(required)* - Product cover basis. Allowed values: `DECREASING`, `LEVEL`, `INCREASING`.
    - ***extendedCoverType*** `number` *(optional)* - Product extended cover type. Only valid if ***type*** is `INCOME_PROTECTION`. Allowed values: `FULL`, `BUDGET`. `FULL` is used by default if no value is specified.
    - ***deferredPeriodInWeeks*** `number` *(required/optional)* - Product deferred period in weeks. Required if ***type*** is `INCOME_PROTECTION`. Allowed values: `0`, `1`, `4`, `8`, `13`, `26`, `52`.
    - ***commissionSacrifice*** `object` *(optional)* - Commission sacrifice. Not required but cannot be empty.
        - ***initial***  `number` *(required)* - Initial commission sacrifice. Does not allow decimals. The lower this value is the higher initial commission will be. Value is a percent represented as integer value between `0` and `100`.
        - ***renewal***  `number` *(optional)* - Renewal commission sacrifice. Allows decimals. The lower this value is the lower renewal commission will be. Value is a percent represented as decimal value between `0` and `2.5`.
    - ***livesAssured*** `array` *(required)* - List of Customers (represented as `object`) for whom the Product is.
        - ***refersTo*** `string` *(required)* - Reference to ID of the Customer. Object with the same value must be available in ***customers*** list.
        - ***waiverOfPremium*** `boolean` *(optional)* - Flag to mark waiver of premium for Customer.
        - ***determinesCeaseAge*** `boolean` *(optional)* - Flag to mark which customer determines the cease age.
        - ***totalPermanentDisability*** `boolean` *(optional)* - Flag to mark total permanent disability for Customer.
- ***owner*** `string` *(optional)* - Owner of the application. If not specified, the authenticated user is used.
- ***quoteEffectiveDate*** `string` *(optional)* - The quote effective date. ISO 8601 date format (YYYY-MM-DD) required. Example: 1980-01-01.
- ***originatorId*** `string` *(optional)* - The id of the portal the application originated from.


### Create new Application [POST]
+ Request Valid Application. (application/json)

    + Headers

            Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTQyMjU0MDAzMH0.oyMYL7t57jhBvw-A3vghOAXl6cixpaTsZW69wz3p5M8

    + Body

            {
                "customers": [
                    {
                        "referenceId": "cus-001",
                        "name": "John",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "occupation": "underwriter",
                        "email": "john.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 120000,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            { "refersTo": "cus-001" }
                        ]
                    }
                ]
            }

+ Response 201 (application/json)

            {
                "id": "1502181407123020689",
                "customers": [
                    {
                        "id": "1001",
                        "referenceId": "cus-001",
                        "enquiryId": "029ab8d8-0a62-423e-8e84-6e8d505bb742",
                        "name": "John",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "email": "john.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "id": "029ab8d8-0a62-423e-8e84-6e8d505bb743",
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 120000,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            { "refersTo": "1001" }
                        ]
                    }
                ],
                "paymentBasis": "MONTHLY",
                "owner": "authenticated-user"
            }


+ Request Customer name cannot contain numbers. (application/json)

    + Headers

            Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTQyMjU0MDAzMH0.oyMYL7t57jhBvw-A3vghOAXl6cixpaTsZW69wz3p5M8

    + Body

            {
                "customers": [
                    {
                        "referenceId": "cus-001",
                        "name": "J0hn",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "email": "john.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 120000,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            { "refersTo": "cus-001" }
                        ]
                    }
                ]
            }

+ Response 400 (application/json)

            {
                "customers": [
                    {
                        "referenceId": "cus-001",
                        "name": {
                            "errorMessage": "Value must be a valid name. Must start with a letter. Can contain letters (A to Z), hyphens (-), apostrophes (') and spaces."
                        }
                    }
                ]
            }

+ Request Invalid cover amount for Product. (application/json)

    + Headers

            Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTQyMjU0MDAzMH0.oyMYL7t57jhBvw-A3vghOAXl6cixpaTsZW69wz3p5M8

    + Body

            {
                "customers": [
                    {
                        "referenceId": "cus-001",
                        "name": "John",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "email": "john.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 0,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            { "refersTo": "cus-001" }
                        ]
                    }
                ]
            }

+ Response 400 (application/json)

            {
                "products": [
                    {
                        "referenceId": "pro-001",
                        "coverAmount": {
                            "errorMessage": "Minimum cover amount is £1"
                        }
                    }
                ]
            }

+ Request Application for two Customers (application/json)

    + Headers

            Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTQyMjU0MDAzMH0.oyMYL7t57jhBvw-A3vghOAXl6cixpaTsZW69wz3p5M8

    + Body

            {
                "customers": [
                    {
                        "referenceId": "cus-001",
                        "name": "John",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "email": "john.doe@domain.com"
                    },
                    {
                        "referenceId": "cus-002",
                        "name": "Jane",
                        "surname": "Doe",
                        "title": "MRS",
                        "gender": "FEMALE",
                        "dateOfBirth": "1980-01-02",
                        "smoker": true,
                        "email": "jane.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 0,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            {
                                "refersTo": "cus-001",
                                "waiverOfPremium": true,
                                "determinesCeaseAge": true
                            },
                            {
                                "refersTo": "cus-002",
                                "determinesCeaseAge": false
                            }
                        ]
                    }
                ]
            }

+ Response 201 (application/json)

            {
                "id":"1504221314246790121",
                "customers":[
                    {
                        "id":"6261",
                        "enquiryId":"6c6f61cb-127d-4890-b3b3-d3c58425b0b0",
                        "referenceId":"cus-001",
                        "name":"John",
                        "surname":"Doe",
                        "title":"MR",
                        "gender":"MALE",
                        "dateOfBirth":"1980-01-01",
                        "smoker":false,
                        "email":"john.doe@domain.com"
                    },
                    {
                        "id":"6262",
                        "enquiryId":"a0d4080d-3ef2-417a-b504-9725efa2d1cb",
                        "referenceId":"cus-002",
                        "name":"Jane",
                        "surname":"Doe",
                        "title":"MRS",
                        "gender":"FEMALE",
                        "dateOfBirth":"1980-01-01",
                        "smoker":true,
                        "email":"jane.doe@domain.com"
                    }
                ],
                "products":[
                    {
                        "id":"173c6b6d-0381-4e05-a532-895557c9a67a",
                        "referenceId":"pro-001",
                        "type":"TERM",
                        "coverBasis":"LEVEL",
                        "coverPeriod":20,
                        "coverAmount":100000,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured":[
                            {
                                "refersTo":"6261",
                                "waiverOfPremium":true,
                                "determinesCeaseAge": true
                            },
                            {
                                "refersTo":"6262",
                                "determinesCeaseAge": false
                            }
                        ]
                    }
                ],
                "paymentBasis": "MONTHLY",
                "owner": "authenticated-user"
            }

+ Request Valid Application with the owner specified. (application/json)

    + Headers

            Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTQyMjU0MDAzMH0.oyMYL7t57jhBvw-A3vghOAXl6cixpaTsZW69wz3p5M8

    + Body

            {
                "customers": [
                    {
                        "referenceId": "cus-001",
                        "name": "John",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "email": "john.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 120000,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            { "refersTo": "cus-001" }
                        ]
                    }
                ],
                "owner": "existing-user"
            }

+ Response 201 (application/json)

            {
                "id": "1502181407123020690",
                "customers": [
                    {
                        "id": "1001",
                        "referenceId": "cus-001",
                        "enquiryId": "029ab8d8-0a62-423e-8e84-6e8d505bb742",
                        "name": "John",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "email": "john.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "id": "029ab8d8-0a62-423e-8e84-6e8d505bb743",
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 120000,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            { "refersTo": "1001" }
                        ]
                    }
                ],
                "paymentBasis": "MONTHLY",
                "owner": "existing-user"
            }

+ Request Valid Application with a non-existing owner. (application/json)

    + Headers

            Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTQyMjU0MDAzMH0.oyMYL7t57jhBvw-A3vghOAXl6cixpaTsZW69wz3p5M8

    + Body

            {
                "customers": [
                    {
                        "referenceId": "cus-001",
                        "name": "John",
                        "surname": "Doe",
                        "title": "MR",
                        "gender": "MALE",
                        "dateOfBirth": "1980-01-01",
                        "smoker": false,
                        "email": "john.doe@domain.com"
                    }
                ],
                "products": [
                    {
                        "referenceId": "pro-001",
                        "type": "TERM",
                        "coverBasis": "LEVEL",
                        "coverPeriod": 20,
                        "coverAmount": 120000,
                        "commissionSacrifice": {
                            "initial": 10,
                            "renewal": 1.0
                        },
                        "livesAssured": [
                            { "refersTo": "cus-001" }
                        ]
                    }
                ],
                "owner": "non-existing-user"
            }

+ Response 400 (application/json)

           {
               "owner": {
                   "errorMessage": "Unknown owner non-existing-user"
               }
           }
