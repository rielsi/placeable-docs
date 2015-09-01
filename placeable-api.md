# Placeable Workbench Locator API v1

This is a read/write REST API that enables remote systems to add, edit, & remove locations within the Placeable Workbench application.


## Authorization

The API is secured using a pre-shared appKey & appId. Each request to the API must have these parameters, or the service will respond with an HTTP status code of 400.


## General Information

<b>Changing Multiple Locations</b>

For all POST, PATCH, & DELETE calls multiple updates are made by sending multiple calls. 

<b>HTTP vs. HTTPS</b>

Any of the calls outlined in this document can be made as HTTP or HTTPS.

<b>Definitions</b>

Collection: A single data set that belongs to a specific brand (Collection = Box).

Location: A single entity within a collection (Location = Item).


## View All Collections

See the collections that are connected to your account.

<b>Method:</b> GET

<b>URL:</b>
	
	/v1/boxes

<b>Example:</b>

	/v1/boxes?appId:12345678&appKey:abcdefghijklmnopqrstuvwxyz000000

NOTE: The appKey & appId have been omitted from all other examples in the documentation for brevity and clarity.


##View Specific Collection

See information about a single collection.

<b>Method:</b> GET

<b>URL:</b>

	/v1/boxes/[id]

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

	/v1/boxes/[id]?q=[value]

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
  
    /[id]/advanced/search?q=[{"operator":"[contains|equals_to|not_contains]","field":"[field name]","value":"[field value]","filter":"[and|or]"}]

<b>Example:</b>

    /543309812f1111620d0e21ba/advanced/search?q=[{"operator":"contains","field":"address.state","value":"CO","filter":"and"}]&offset=0&limit=20


## Adding a New Location

Create a new location within a specific collection.

<b>Method:</b> POST

<b>Headers:</b>
<ul>
<li>Content-Type – application/json</li>
</ul>

<b>URL:</b>

	/v1/boxes/[id]/items
	
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

<b>Example:</b>

	/boxes/543309812f1111620d0e21ba/items
