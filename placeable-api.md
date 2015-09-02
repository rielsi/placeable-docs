# Placeable Workbench Locator API v1

This is a read/write REST API that enables remote systems to add, edit, & remove locations within the Placeable Workbench application.


## Authorization

The API is secured using a pre-shared appKey & appId. Each request to the API must have these parameters, or the service will respond with an HTTP status code of 400.

<b>Example:</b>

	https://app.placeable.com/v1/boxes?appId:12345678&appKey:abcdefghijklmnopqrstuvwxyz000000
	
NOTE: The appKey & appId have been omitted from all other examples in the documentation for brevity and clarity.


## General Information

<b>Changing Multiple Locations</b>

For all POST, PATCH, & DELETE calls multiple updates are made by sending multiple calls. 

<b>HTTP vs. HTTPS</b>

Any of the calls outlined in this document can be made as HTTP or HTTPS.

<b>Definitions</b>
<ul>
<li>Collection: A single data set that belongs to a specific brand (Collection = Box).</li>
<li>Location: A single entity within a collection (Location = Item).</li>
</ul>

## View All Collections

<b>Method:</b> GET

<b>URL:</b>
	
	/v1/boxes
	

##View Specific Collection

<b>Method:</b> GET

<b>URL:</b>

	/v1/boxes/[collection id]

<b>Example:</b>

	/v1/boxes/543309812f1111620d0e21ba
	
## Basic Search

Search for locations based on specific keywords in the name, address, phone, & unique id fields. A search for “CO” would return every location in the state of Colorado and also any location with a street name of “Cook”, “County”, & “Common”. If you would like to return all of the locations within the collection leave the query value null.

<b>Method:</b> GET

<b>Required Parameters:</b>
<ul>
<li>q: The basic search query will allow a user to perform a keyword search against all fields within a collection. This search will return all locations that have a matching value to the search query in any of the available fields.</li>
</ul>
<b>Optional Parameters:</b>
<ul>
<li>offset: The number of locations that should be skipped. If two locations match a search query and offset is set to 1 then the first result will be skipped and only the second location will be returned.</li>
<li>limit: The maximum number of locations returned by the search. If 20 locations match a search query and limit is set to 10 then only the first 10 locations will be returned.</li>
</ul>
<b>URL:</b>

	/v1/boxes/[collection id]?q=[value]

<b>Example:</b>

	/v1/boxes/543309812f1111620d0e21ba/items?q="CO"&offset=0&limit=20


## Advanced Search

Search for locations based on specific keywords at the field level.

<b>Method:</b> GET

<b>Required Parameters:</b>
<ul>
<li>q: The advanced search query will allow a user to perform a keyword search against specific fields within a collection. This search will return all locations that have a matching value to the search query in the specific fields selected.  To build an advanced search query you will need to combine three data points (Field Name, Search Type, & Value). You can also combine multiple searches using a vertical bar.</li>
</ul>
<b>Optional Parameters:</b>
<ul>
<li>offset: The number of locations that should be skipped. If two locations match a search query and offset is set to 1 then the first result will be skipped and only the second location will be returned.</li>
<li>limit: The maximum number of locations returned by the search. If 20 locations match a search query and limit is set to 10 then only the first 10 locations will be returned.</li>
</ul>

<b>URL:</b>
  
    /collection id]/advanced/search?q=[{"operator":"[contains|equals_to|not_contains]","field":"[field name]","value":"[field value]","filter":"[and|or]"}]

<b>Example:</b>

    /543309812f1111620d0e21ba/advanced/search?q=[{"operator":"contains","field":"address.state","value":"CO","filter":"and"}]&offset=0&limit=20


## View Specific Location

<b>Method:</b> GET

<b>URL:</b>

	/v1/boxes/[collection id]/items/[location id]

<b>Example:</b>

	/v1/boxes/543309812f1111620d0e21ba/items/53e3c2a820000004091dae13


## Adding a New Location

<b>Method:</b> POST

<b>Headers:</b>
<ul>
<li>Content-Type – application/json</li>
</ul>

<b>URL:</b>

	/v1/boxes/[collection id]/items
	
<b>Body:</b>

	{ 
	"address":{ 
		"street":"",
		"street2":"",
		"city":"",
		"state":"",
		"postal":"",
		"country":""
	},
	"locationId":"",
	"geocode":{ 
		"lat":0,
		"long":0,
		"accuracy":""
	},
	"phone":{ 
		"main":"",
		"mobile":"",
		"alternate":"",
		"fax":""
	},
	"urls":{ 
		"facebook":"",
		"google":"",
		"bing":"",
		"yahoo":"",
		"yelp":"",
		"local":"",
		"factual":"",
		"foursquare":"",
		"yellowpages":""
	},
	"photos":{ 
	},
	"metaData":{ 
	},
	"userFields":{ 
	},
	"name":"Placeable",
	"clientRefId":"",
	"description":"",
	"hoop":"",
	"paymentTypes":"",
	"languages":"",
	"services1":"",
	"pinPlacementConfirmed":false,
	"email":"",
	"openClosed":""
	}

<b>Notes:</b>
<ul>
<li>All of the fields outlined above must be included with the POST call. The only field that requires a value is "pinPlacementConfirmed". Unless otherwise instructed by your account manager, this field should be set to "false".</li>
<li>When setting the hoop (hours of operation) for a location it is best to send the data in Google's hour format. Information about these formats can be found <a href="https://support.google.com/business/answer/3370250?vid=1-635767449932514491-1285679727#hours" target="_blank">here</a>.</li>
</ul>

<b>Example:</b>

	/boxes/543309812f1111620d0e21ba/items


## Updating an Existing Location

<b>Method:</b> PATCH

<b>URL:</b>

	/v1/boxes/[collection id]/items/[location id]

<b>Body:</b>

	{ 
		"op" : "update",
		"path": "[field name]", 
		"value" : "[value]"
	}

<b>Example:</b>

	/boxes/543309812f1111620d0e21ba/items/53e3c2a820000004091dae13


## Deleting an Existing Location

<b>Method:</b> DELETE

<b>URL:</b>

	/v1/boxes/[collection id]/items/[location id]
	
<b>Example:</b>

	/v1/boxes/543309812f1111620d0e21ba/items/53e3c2a820000004091dae13


## Response Details

<b>Collection:</b>

	{
		"name": "Alex Cummings-1111111111111",
		"dataMapping": [
			"543309812f1111620d0e21b9"
		],
		"id": "543309812f1111620d0e21bb",
		"mappingName": "Placeable_Locations.csv",
		"syndicationConfigs": [],
		"config": {
			"fileId": "543309892f0000810c0e21bb"
		},
		"mappingSet": true,
		"syndicated": false,
		"validated": true,
		"transformed": false,
		"active": true,
		"uploaded": true,
		"dateCreated": 1412630913100,
		"lastUpdated": 1412630913100,
		"brandName": "Placeable",
		"collectionGroupName": "Placeable",
		"placeableKey": "",
		"placeableSite": "",
		"currentEditRecipe": "",
		"currentPinPlacementRunId": "",
		"metaData": {
			"loading": "false",
			"searchType": "brand"
	}
	
<b>Location:</b>

	{
      "name": "Placeable",
      "clientRefId": "55e637ae25000057000f5063",
      "locationId": "12345",
      "description": "",
      "id": "55e637c1250000ad000f50ea",
      "address": {
        "street": "2601 Blake St",
        "street2": "Ste 301",
        "city": "Denver",
        "state": "CO",
        "postal": "80205",
        "country": "US"
      },
      "geocode": {
        "lat": 39.760256,
        "long": -104.987145,
        "accuracy": "PROVIDED"
      },
      "phone": {
        "main": "(855) 433-7133",
        "mobile": "",
        "alternate": "",
        "fax": ""
      },
      "urls": {
        "facebook": "",
        "google": "",
        "bing": "",
        "yahoo": "",
        "yelp": "",
        "local": "",
        "foursquare": "",
        "factual": "",
        "yellowpages": ""
      },
      "photos": {},
      "userFields": {},
      "hoop": "",
      "paymentTypes": "",
      "languages": "",
      "email": "",
      "services1": "",
      "openClosed": "",
      "metaData": {
        "thirdPartyScore": "99",
        "locationScore": "99",
        "facebookActivity": "0",
        "pinSpreadScore": "5",
        "pinSpreadBucket": "good",
        "zipTransformation": "true",
        "yellowpagesActivity": "0",
        "googleActivity": "0",
        "foursquareActivity": "0",
        "pinScore": "0",
        "yelpActivity": "0",
        "pinBucket": "poor",
        "pinPlacementScore": "0",
        "completenessAverage": "99",
        "pinPlacement": "true",
        "activityTotal": "1"
      },
      "pinPlacementConfirmed": true,
      "score": 99,
      "pins": [
        {
          "lat": 0,
          "long": 0,
          "source": "facebook",
          "name": "",
          "address": {
            "street": "",
            "street2": "",
            "city": "",
            "state": "",
            "postal": "",
            "country": ""
          },
          "phone": {
            "main": "",
            "mobile": "",
            "alternate": "",
            "fax": ""
          },
          "rating": 0,
          "distance": 10545342,
          "responseCode": 200,
          "responseTxt": "",
          "success": false,
          "url": "",
          "stats": {
          },
          "website": ""
        },
        {
          "lat": 0,
          "long": 0,
          "source": "factual",
          "name": "",
          "address": {
            "street": "",
            "street2": "",
            "city": "",
            "state": "",
            "postal": "",
            "country": ""
          },
          "phone": {
            "main": "",
            "mobile": "",
            "alternate": "",
            "fax": ""
          },
          "rating": 0,
          "distance": 5064,
          "responseCode": 200,
          "responseTxt": "",
          "success": true,
          "url": "",
          "stats": {
          },
          "website": ""
        },
        {
          "lat": 0,
          "long": 0,
          "source": "foursquare",
          "name": "",
          "address": {
            "street": "",
            "street2": "",
            "city": "",
            "state": "",
            "postal": "",
            "country": ""
          },
          "phone": {
            "main": "",
            "mobile": "",
            "alternate": "",
            "fax": ""
          },
          "rating": 0,
          "distance": 10545342,
          "responseCode": 200,
          "responseTxt": "",
          "success": false,
          "url": "",
          "stats": {
          },
          "website": ""
        },
        {
          "lat": 0,
          "long": 0,
          "source": "google",
          "name": "",
          "address": {
            "street": "",
            "street2": "",
            "city": "",
            "state": "",
            "postal": "",
            "country": ""
          },
          "phone": {
            "main": "",
            "mobile": "",
            "alternate": "",
            "fax": ""
          },
          "rating": 0,
          "distance": 5085,
          "responseCode": 200,
          "responseTxt": "",
          "success": true,
          "url": "",
          "stats": {
          },
          "website": ""
        },
        {
          "lat": 0,
          "long": 0,
          "source": "yellowpages",
          "name": "",
          "address": {
            "street": "",
            "street2": "",
            "city": "",
            "state": "",
            "postal": "",
            "country": ""
          },
          "phone": {
            "main": "",
            "mobile": "",
            "alternate": "",
            "fax": ""
          },
          "rating": 0,
          "distance": 10545342,
          "responseCode": 200,
          "responseTxt": "",
                    "success": false,
          "url": "",
          "stats": {
          },
          "website": ""
        },
        {
          "lat": 0,
          "long": 0,
          "source": "yelp",
          "name": "",
          "address": {
            "street": "",
            "street2": "",
            "city": "",
            "state": "",
            "postal": "",
            "country": ""
          },
          "phone": {
            "main": "",
            "mobile": "",
            "alternate": "",
            "fax": ""
          },
          "rating": 0,
          "distance": 10545342,
          "responseCode": 200,
          "responseTxt": "",
          "success": true,
          "url": "",
          "stats": {
          },
          "website": ""
        }
      ],
      "errors": [],
      "compare": {
        "locationId": "55e637c1250000ad000f50ea",
        "clientId": "55e637ae25000057000f5063",
        "diffs": [
          {
            "api": "facebook",
            "compares": [
              {
                "source": "facebook",
                "field": "address.street",
                "value": "2601 Blake St",
                "sourceValue": ""
              },
              {
                "source": "facebook",
                "field": "address.city",
                "value": "Denver",
                "sourceValue": ""
              },
              {
                "source": "facebook",
                "field": "address.state",
                "value": "CO",
                "sourceValue": ""
              },
              {
                "source": "facebook",
                "field": "address.postal",
                "value": "80205",
                "sourceValue": ""
              },
              {
                "source": "facebook",
                "field": "phone.main",
                "value": "(855) 433-7133",
                "sourceValue": ""
              },
              {
                "source": "facebook",
                "field": "brand",
                "value": "Placeable",
                "sourceValue": ""
              }
            ],
            "found": false,
            "rating": 0
          },
          {
            "api": "factual",
            "compares": [
              {
                "source": "factual",
                "field": "address.street",
                "value": "2601 Blake St",
                "sourceValue": "2601 Blake Ave"
              }
            ],
            "found": true,
            "rating": 0
          },
          {
            "api": "foursquare",
            "compares": [
              {
                "source": "foursquare",
                "field": "address.street",
                "value": "2601 Blake St",
                "sourceValue": ""
              },
              {
                "source": "foursquare",
                "field": "address.city",
                "value": "Denver",
                "sourceValue": ""
              },
              {
                "source": "foursquare",
                "field": "address.state",
                "value": "CO",
                "sourceValue": ""
              },
              {
                "source": "foursquare",
                "field": "address.postal",
                "value": "80205",
                "sourceValue": ""
              },
              {
                "source": "foursquare",
                "field": "phone.main",
                "value": "(855) 433-7133",
                "sourceValue": ""
              },
              {
                "source": "foursquare",
                "field": "brand",
                "value": "Placeable",
                "sourceValue": ""
              }
            ],
            "found": false,
            "rating": 0
          },
          {
            "api": "google",
            "compares": [
              {
                "source": "google",
                "field": "brand",
                "value": "Placeable",
                "sourceValue": "Placeable-Denver"
              }
            ],
            "found": true,
            "rating": 0
          },
          {
            "api": "yellowpages",
            "compares": [
              {
                "source": "yellowpages",
                "field": "address.street",
                "value": "2601 Blake St",
                "sourceValue": ""
              },
              {
                "source": "yellowpages",
                "field": "address.city",
                "value": "Denver",
                "sourceValue": ""
              },
              {
                "source": "yellowpages",
                "field": "address.state",
                "value": "CO",
                "sourceValue": ""
              },
              {
                "source": "yellowpages",
                "field": "address.postal",
                "value": "80205",
                "sourceValue": ""
              },
              {
                "source": "yellowpages",
                "field": "phone.main",
                "value": "(855) 433-7133",
                "sourceValue": ""
              },
              {
                "source": "yellowpages",
                "field": "brand",
                "value": "Placeable",
                "sourceValue": ""
              }
            ],
            "found": false,
            "rating": 0
          },
          {
            "api": "yelp",
            "compares": [
              {
                "source": "yelp",
                "field": "address.street",
                "value": "2601 Blake St",
                "sourceValue": "2601 Blake Ave"
              }
            ],
            "found": true,
            "rating": 0
          }
        ],
        "created": 1441151804380
      },
      "created": 1441150914433,
      "lastUpdated": 1441151804382,
      "crowdsourceKey": "",
      "matchStats": {
        "factual": "match",
        "google": "match",
        "yellowpages": "match",
        "foursquare": "match",
        "facebook": "match",
        "yelp": "match"
      },
      "bucket": "good"
    }
    
    
