# Placeable Locator API v1

This is a read-only REST API that enables remote applications to consume local search data and offer geospatial search capabilities.


## Authorization

The API is secured using a pre-shared key. Each request to the API must have this as a parameter, or the service will respond with an HTTP status code of 400.

Method: GET

Parameter: key

Required: TRUE  

Example:

     /v1/search?q=fargo&key=c12226cc85f0662db6aa21d1b5c7b664adb41497

NOTE: This API key has been omitted from the other examples in the documentation for brevity and clarity.

To obtain your auth key, contact your account manager, or <support@placeable.com>


## Standard Search ##

Method: GET 

URL:  `/v1/search`

Examples:

    /v1/search?q=colorado+springs
    
or

    http://your.domain.com/v1/search?q=colorado+springs
    
    
NOTE: The example assume you have the API activated, and have data loaded for you site in Colorado Springs.

### Parameters ###

#### q (as in q=80202) ####

Required: TRUE  

Description: The default search parameter to search for a location. This must be URL encoded.

Example:  

	/search?q=Denver%20CO

You can also search by lat & long. Just pass the parameters as shown below:

	/search?q=latitude:39.770224029999994;longitude:-105.02012416 

The colons `:` and the semicolon `;` are required.   
    
   
### sortOrder

Required: FALSE

Options: “Distance, NameAZ, NameZA”


Description:
• Distance is the distance from the point where “q” is geocoded.
• NameAZ is the results ordered by name in descending order
• NameZA is the results ordered by name in ascending order


Example:

    /v1/search?q=Denver%20CO&sortOrder=NameAZ


### filters

Required: FALSE

These are specific to your data. If your data has "atmLanguages" with the options of "english, french and spanish" and you want to filter down to only "english" locations the URL would look like:  

	/search?q=Denver%20CO&filters=atmLanguages-_-english  

and if you'd like to filter in english and spanish the URL would look like:  

	/search?q=Denver%20CO&filters=atmLanguages-_-english-_-spanish   

The `_-_` is the delimiter between filter/value pairs.


### page

Required: FALSE

Options: Return a different page of the result sets.

Example:
Return the second page 

     /v1/search?q=Denver%20CO&sortOrder=NameAZ&page=2

### callback

Required: FALSE

Options: Return the json wrapped in the specified callback. The ContentType for all calls with a callback is 'aplication/javascript'.

Example:
Wraps the json in "test" callback 

     /v1/search?q=Denver%20CO&sortOrder=NameAZ&callback=test

## Advanced Search ##
  
 Method: GET
 
 URL: `/v1/search`
 
 Examples:
 
	/v1/search?t=80033&f=postal
     
 or
 
 	http://your.domain.com/v1/search?t=80033&f=postal
 
### Parameters ###
 
#### t (as in t=80033) ####
 
 Required: TRUE  
 
 Description: The term that you would like to search for. This must be URL encoded.
 
 Example:  
 
 	/v1/search?t=80033
 
#### f (as in f=postal) ####
 
 Required: TRUE  
 
 Description: The field that the term should be queried against. The field name must match the database and only fields that are made searchable by an administrator can be queried.
 
 Example:  
 
 	/v1/search?t=80033&f=postal
 	
 You can also include multiple fields in your search. Just pass the parameters as shown below:
 
 	v1/search?t=80033&f=postal,streetAddress
    
## Response Details ##

The response consists of location data, and results. The location section contains information about where the search was carried out, and the results section contains the list of results that were found plus some metadata about the result set.

__Location__

        "searched": {
            "adminDistrict": "CO", 
            "country": "United States", 
            "entityType": "Address", 
            "formattedAddress": "2601 Blake St, Denver, CO 80205", 
           	"locality": "Denver", 
            "latitude": 39.7603645324707, 
            "longitude": -104.987144470215, 
            "street": "2601 Blake St"
        }
        
| Key               | Value       |
|:------------------|:------------|
|adminDistrict      | State     | 
|country            | Country     | 
|entityType         | Type of location returned e.g address, city, state, country  |
|formattedAddress | Representaton of the address found | 
|locality | City|
|latitude | Latitude | 
|longtitude| Longtitude |
|street| Street|
  
  It is possible that some of the above value may be blank given the search supplied. For example you enter Colorado as the search, the search and locality will be blank 


### Paging

Under the results section of the API response there information about paging. The resultCount is the total number of records returned by the search, and the pageCount is the total number of pages. The user can request a different page from the result set by querying with the page parameter (explained above)


### No Results found


Below is an example of a search which returned zero results

		{
		    "location": {
		        "candidates": [], 
		        "searched": {
		            "adminDistrict": "AK", 
		            "country": "United States", 
		            "entityType": "PopulatedPlace", 
		            "formattedAddress": "Nome, AK", 
		            "latitude": 64.49947357177734, 
		            "locality": "Nome", 
		            "longitude": -165.40579223632812, 
		            "street": ""
		        }
		    }, 
		    "results": {
		        "resultCount": 0, 
		        "results": []
		    }
		}
		

## Errors
Below are the different types of errors that can occur if the right information is not provided to the api.

| Status Code               | Description       |
|:------------------|:------------|
|400      | No auth key provided, or bad request, or json is not allowed for the site    | 
|401      | Unauthorized api key.
|404      | Couldn't find  search origin given the q parameter, or unknown location |

### No Auth Key Provided (400)
		{
		    "error": "Key is required to search the API."
		}

### Json not allowed (400)
		{
		    "error": ""Json is not allowed for 'site'.""
		}

### Invalid Key Provided(401)
		{
		    "error": "This key ('invalid key name') has not been authorized"
		}
		
### Unknown Location(404)
		{
		    "error": "Unknown location"
		}		
		
## Full Response

  Below is a full response, for each result any flexible fields which have been loaded during the dataload will be passed directly through.

		    {
			    "location": {
			        "candidates": [], 
			        "searched": {
			            "adminDistrict": "CO", 
			            "country": "United States", 
			            "entityType": "Address", 
			            "formattedAddress": "2601 Blake St, Denver, CO 80205", 
			            "latitude": 39.7603645324707, 
			            "locality": "Denver", 
			            "longitude": -104.987144470215, 
			            "street": "2601 Blake St"
			        }
			    }, 
			    "results": {
			        "distanceUnit": "miles", 
			        "pageCount": 1, 
			        "resultCount": 1, 
			        "sortOrder": "Distance",
			        "results": [
			            {
			                "id": "81341",
			                "city": "DENVER", 
			                "detail.languages": [
			                    "English", 
			                    " Spanish"
			                ], 
			                "detail.services": [
			                    "Bulk Mail Acceptance", 
			                    "Bulk Mail Account Balance", 
			                    "Bulk Mail New Permit", 
			                    "Burial Flags", 
			                    "Business Reply Mail Account Balance", 
			                    "Business Reply Mail New Permit", 
			                    "Duck Stamps"
			                ], 
			                "detailsUrl": "co/denver/81341", 
			                "distance": 1.5261252679006334, 
			                "email": "", 
			                "fax": "303-951-3484", 	           
			                "custom.languages": {
			                    "ENG": "English", 
			                    "SPA": "Spanish"
			                }, 
			                
			                "languages": "English, Spanish", 
			                "lastModified": "Wed, 28 Aug 2013 05:36:43 245 -0600", 
			                "latitude": "39.759108", 
			                "location": "39.759108,-105.015829", 
			                "longitude": "-105.015829", 
			                "name": "Mile High", 
			                "phone": " 303-571-0722", 
			                "result.languages": [
			                    "English", 
			                    " Spanish"
			                ], 
			                "result.services": [
			                    "Bulk Mail Acceptance", 
			                    "Bulk Mail Account Balance", 
			                    "Bulk Mail New Permit", 
			                    "Burial Flags", 
			                    "Business Reply Mail Account Balance", 
			                    "Business Reply Mail New Permit", 
			                    "Duck Stamps"
			                ], 
			                "services":  "Bulk Mail Acceptance,Bulk Mail Account Balance,Bulk Mail New Permit,Burial Flags,Business Reply Mail Account Balance,Business Reply Mail New Permit,Duck Stamps",
			                "state": "CO", 
			                "streetAddress": "450 W 14TH AVE", 
			                "streetAddress2": "", 
			                "type": "Branch", 
			                "webUrl": "http://geocities.com/mileHighLocation", 
			                "zip": "80204-9998"
			            }
			        ]
			        
			    }
			}


