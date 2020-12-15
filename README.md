## 1. Introduction

USDA’s Economic Research Services (ERS) is making data from USDA’s Agriculture Resource Management Survey (ARMS) available through an Application Programming Interface (API) to better serve customers. The data in the API are available in JSON format and provide attribute-based querying. The data are also available in bulk files.

This page provides a brief explanation of the API to get you started. For user convenience, we provide demos using R: open source statistical programming language.

A list of variables, by report, is provided in section 3. An exhaustive list of all variables and detailed descriptions is found in the file <a href="https://www.ers.usda.gov/media/10257/allvariables.csv">AllVariables.csv</a>.

## 2. Getting Started with the ARMS-Data-API

You may use the ARMS Data API to develop a service to search, display, analyze, retrieve, view, and otherwise "get" information from ARMS data.

### 2.1 
Users of the ARMS data API are required to sign up for an API key via api.data.gov, a free API management service for Federal agencies. A valid email address is required to obtain a key. Once users have completed the signup requirements, the API key will be automatically sent to the user-provided email address. API keys provide a means to notify users of changes in the APIs.

**Note:** A key is not required to use the bulk download facility.

### 2.2 Using the API
The API support GraphQL and REST API methods:

* Rest API

Rest API endpoint: https://api.ers.usda.gov/data/

**REST Methods**

The REST API supports POST and GET methods for calling data, except where noted.

Each endpoint also supports the OPTIONS method that returns the endpoint schema and details on input fields and requirements, such as format, whether the field allows

**/arms/state**

This API resource gets all States and the metadata and variables available for each of the States.

_Input fields:_ not needed

_Method_: supports GET only

``` 
GET https://api.ers.usda.gov/data/arms/state?api_key=YOUR_API_KEY
```

**/arms/year**

This API resource gets all available years for which data are available.

_Input fields:_ not needed

_Method:_ supports GET only

```
GET https://api.ers.usda.gov/data/arms/year?api_key=YOUR_API_KEY
```

**/arms/surveydata**

_Input fields:_ "Year" is a required field; at least one of the two input fields ("report" or "variable") is also required

_Method:_ supports GET and POST

_OPTION:_ returns the schema for the survey data REST resource

```
POST https://api.ers.usda.gov/data/arms/surveydata?api_key=YOUR_API_KEY

{
 "year": [2011, 2012, 2013, 2014, 2015, 2016]
 "state": "all"
 "variable": "igcfi"
 "category": "NASS Regions"
 "category2": "Collapsed Farm Typology"
}
```

The above retrieves "Gross Farm Income" for years 2011 through 2016 for "All States" and broken by Category = NASS regions and Category2 = Collapsed Farm Typology.

```GET https://api.ers.usda.gov/data/arms/surveydata?api_key=YOUR_API_KEY?year=2015,2016&state=all&report=income+statement&farmtype=operator+households&category=collapsed+farm+typology&category_value=commercial```
This GET request returns income statements for years 2015 and 2016 of commercial farms across all States.

**Note:**  Multiple values for fields that allow multiple values are separated by "commas" (,), and names of fields exceeding more than one word are separated by a "plus" (+). Different query parameters such as year, states, report, etc. are separated by an "ampersand" (&).

**/arms/category**

This API resource lists categories and subcategories within each category. It also provides relevant metadata and variables available for each of the categories and subcategories.

_Input fields:_ not required but can be passed on; see below.

_Method:_ supports GET and POST

 This resource can be used in two ways:

   1. When used without any input variables, ALL categories and subcategories are returned.

`POST https://api.ers.usda.gov/data/arms/category?api_key=YOUR_API_KEY`

`GET https://api.ers.usda.gov/data/arms/category?api_key=YOUR_API_KEY`

   2. When used with a specific category name, all details and subcategories within that category are returned

`POST https://api.ers.usda.gov/data/arms/category?api_key=YOUR_API_KEY`

```{ 
 "name": "Collapsed Farm Typology" 
}
```   

`GET https://api.ers.usda.gov/data/arms/category?id=age,ftypll`   

This query returns Category-related information for "Operator age" (id=age) and "Farm Typology" (id = ftypll)

**/arms/report**

 This API resource gets available reports with the relevant metadata and variables available for each report. This resource can be used  in two ways:

   1. Retrieve a list of ALL reports
    
`POST https://api.ers.usda.gov/data/arms/report?api_key=YOUR_API_KEY`

`GET https://api.ers.usda.gov/data/arms/report?api_key=YOUR_API_KEY`

   2. Retrieve metadata and variables for a specific report based on name, ID, or keyword.

`POST https://api.ers.usda.gov/data/arms/report?api_key=YOUR_API_KEY`

```{
"name": "balance sheet"
}
```

**/arms/variable**

This API resource gets variables with the relevant information and metadata for each of the variables used in ARMS. This resource can be used in two ways:

  1. List ALL variables.

```POST https://api.ers.usda.gov/data/arms/variable?api_key=YOUR_API_KEY```

```GET https://api.ers.usda.gov/data/arms/variable?api_key=YOUR_API_KEY```

  2. Search "variable" by ID, report, name, and keywords. All input fields are optional, and one can use all or none of them to get the desired data.

```POST https://api.ers.usda.gov/data/arms/variable?api_key=YOUR_API_KEY
 {
  "report": "Business Balance Sheet"          
  "name": "Liabilities current debt"
 }
 ```
 
**/arms/farmtype**

One can use this REST resource to get all Farm Types or search by keyword, name, or ID.

_Input fields:_ optional

_Method:_ supports GET and POST

`POST https://api.ers.usda.gov/data/arms/farmtype?api_key=YOUR_API_KEY`

`{
  "name": "operator households"
}` 

`GET https://api.ers.usda.gov/data/arms/farmtype?api_key=YOUR_API_KEY`
`GET https://api.ers.usda.gov/data/arms/farmtype?api_key=YOUR_API_KEY&name=operator+households`

* GraphQL

GraphQL is a modern method to easily interact with an API. The GraphQL endpoint for ARMS data is https://api.ers.usda.gov/data/arms/graphql.

* GraphQL methods

GraphQL is for advanced users. If you are not familiar with GraphQL, use our REST API. A GraphQL client, such as ChromeiQL, is needed to interact with the ARMS API. <a href="https://graphql.org/">More information on GraphQL</a> is available.

## 3. Reports

ARMS reports are a collection of financial data points about U.S.-based farms that can be sorted and viewed in a variety of ways, including national level, State level, farmer type, and farmer age.

The ARMS team has created some commonly used tailored reports from the ARMS dataset.  Currently, the following tailored reports are available on the WebTool and through the API:

* Farm Business Balance Sheet
* Farm Business Income Statement
* Farm Business Financial Ratios
* Structural Characteristics
* Farm Business Debt Repayment Capacity
* Government Payments
* Operator Household Income
* Operator Household Balance Sheet
 
## 4. Variables
Each report contains a different set of variables. A list of all variables sorted by report and with detailed descriptions and short IDs may be found in the file <a href="https://www.ers.usda.gov/media/10257/allvariables.csv">AllVariables.csv</a>.

## 5. Available Categories
Categories are meta tags that provide context for the data. The available categories are listed below with their short IDs inside the parenthesis. Values for each category and their definitions may be found in the file <a href="https://www.ers.usda.gov/media/10257/allvariables.csv">AllVariables.csv</a>.

* Residence farms
* Intermediate farms
* Commercial farms

Economic Class (sal)

* $1,000,000 or more
* $500,000 to $999,999
* $250,000 to $499,999
* $100,000 to $249,999
* less than $100,000

Farm Typology (ftyppl)

* Retirement farms (2012 to present)
* Off-farm occupation farms (2012 to present)
* Farming occupation/lower sales farms (2012 to present)
* Farming occupation/moderate-sales farms (2012 to present)
* Midsize farms (2012 to present)
* Large farms (2012 to present)
* Very large farms (2012 to present)
* Nonfamily farms (2012 to present)
* Retirement farms (1996 through 2011)
* Residential/lifestyle farms (1996 through 2011)
* Farming occupation/lower sales farms (1996 through 2011)
* Farming occupation/moderate-sales farms (1996 through 2011)
* Large farms (1996 through 2011)
* Very large farms (1996 through 2011)
* Nonfamily farms (1996 through 2011)

Operator age (age)

* 34 years or younger
* 35 to 44 years old
* 45 to 54 years old
* 55 to 64 years old
* 65 years or older

Farm Resource Region (reg)

* Heartland
* Northern Crescent
* Northern Great Plains
* Prairie Gateway
* Eastern Uplands
* Southern Seaboard
* Fruitful Rim
* Basin and Range
* Mississippi Portal

NASS region (n5reg)

* Atlantic region
* South region
* Midwest region
* Plains region
* West region

Production Specialty (spec)

* Tobacco, cotton, peanuts
* Other field crops
* Cattle
* Dairy
* Hogs, poultry, other
* Vegetables
* Nursery and greenhouse
* All cash grains
* Specialty crops
* All other livestock
 
## 6. Updates and Revisions

**August 29, 2019**

A database update was released on August 27, 2019, containing revised data for 2017 and other changes (see full description on the ARMS data product <a href="https://www.ers.usda.gov/data-products/arms-farm-financial-and-crop-production-practices/update-and-revision-history/">here</a>).

* The “searching”, and “formatting” properties of the “info” section of the API query result have been removed in this version of the API.
* Each record of the category query result contains a new category “description” property.
* Each record of the report query result contains a new report “desc” property.
* Each record of the farmtype query result contains a new farmtype “desc” property.
* Each record of the surveydata query result contains a new “decimal_display” property, indicating the proper number of decimals to display for the value.
