# Placeable Locator API v0 #

This is a read-only REST API that enables remote applications to consume local search data and offer geospatial search capabilities. The API is available with two response formats: XML and JSON, allowing developers to use whichever format is most natural. This API directly maps to the web search capabilities, all functionality and data available on the web is available through this API.

## Search ##

Method: get 

Url:  `/search`

### Headers ###

Type: Accept

Required: true

Description: This sets which format you would like the response to be (xml/json).

Example:

	application/xml

or

	application/json    

### Parameters ###

#### q (as in q=80202) ####

Required: true  

Description: The default search parameter to search for a location. This must be URL encoded.

Example:  

	/search?q=Denver%20CO

You can also search by lat & long. Just pass the parameters as shown below:

	/search?q=latitude:39.770224029999994;longitude:-105.02012416 

The colons `:` and the semicolon `;` are required. 

#### sortOrder ####

Required: false

Options: "Distance, NameAZ, NameZA"

Description:
* Distance is the distance from the point where "q" is geocoded.
* NameAZ is the results ordered by name in descending order
* NameZA is the results ordered by name in ascending order   

Example:  

	/search?q=Denver%20CO&sortOrder=NameAZ    

#### filters  ####

Required: false

Options: These are specific to your data.                                               

Description:  

These are specific to your data and will filter in specific results. If your data has "atmLanguages" with the options of "english, french and spanish" and you want to filter in only "english" locations the URL would look like:  

	/search?q=Denver%20CO&filters=atmLanguages-_-english  

and if you'd like to filter in english and spanish the url would look like:  

	/search?q=Denver%20CO&filters=atmLanguages-_-english-_-spanish   

As you can see the `_-_` is the delimiter between filter/value pairs.

Example:                                                                  

	/search?q=Denver%20CO&filters=atmLanguages-_-spanish_-_atmLanguages-_-french   

### Response ###

### Response Code ###

If the search completes successfully with results, then the HTTP response code will be 200, 
when there are no locations within range of the search then the API HTTP response code will be 404, and the XML an empty result set

	<?xml version="1.0" encoding="UTF-8" ?>
	<results>
	</results>
	
If the search critera wasn't found the service will also respond with a 404 code, but the content will default to be HTML


#### XML ####

	<?xml version="1.0" encoding="UTF-8" ?>
	<results>
	<result>
	    <field name="city"><![CDATA[Denver]]></field>
	    <field name="state"><![CDATA[CO]]></field>
	    <field name="postal"><![CDATA[80202+1234]]></field>
	    <field name="displayName"><![CDATA[ATM: 16th & Treemont]]></field>
	    <field name="services"><![CDATA[]]></field>
	    <field name="name"><![CDATA[ATM: 16th & Treemont]]></field>
	    <field name="location"><![CDATA[39.74000975,-104.9922596]]></field>
	    <field name="latitude"><![CDATA[39.74000975]]></field>
	    <field name="streetAddress"><![CDATA[303 16th St. Suite 100]]></field>
	    <field name="lastModified"><![CDATA[Fri Jan 13 08:17:37 MST 2012]]></field>
	    <field name="atmLanguages"><![CDATA[Spanish, Korean, Chinese, French, Italian, Russian, Greek, Portuguese, Polish]]></field>
	    <field name="longitude"><![CDATA[-104.9922596]]></field>
	    <field name="id"><![CDATA[4640]]></field>                
	    <field name="numAtms"><![CDATA[1]]></field>
	    <field name="jcrId"><![CDATA[832ce5e3-ed4b-4fec-ab9e-c8c15aa3a12c]]></field>
	    <field name="detailsUrl"><![CDATA[co/denver/4640]]></field>
	    <field name="day1hrs"><![CDATA[8:00-5:30]]></field>
	    <field name="day2hrs"><![CDATA[8:00-5:30]]></field>
	    <field name="day3hrs"><![CDATA[8:00-5:30]]></field>
	    <field name="day4hrs"><![CDATA[8:00-5:30]]></field>
	    <field name="day5hrs"><![CDATA[8:00-5:30]]></field>
	    <field name="day6hrs"><![CDATA[9:00-12:00]]></field>
	    <field name="day7hrs"><![CDATA[Closed]]></field>
	    <field name="type"><![CDATA[ATM]]></field>
	    <field name="fieldOrder"><![CDATA[atmLanguages,city,services,day1hrs,day2hrs,day3hrs,day4hrs,day5hrs,day6hrs,day7hrs,detail.awards,detail.hoop,detail.otherservices,detail.services,displayName,id,latitude,longitude,name,numAtms,phone,postal,result.hoop,result.services,state,streetAddress,type]]></field>
	    <field name="distance"><![CDATA[2.2096865271850556E-4]]></field>
	    <field name="phone"><![CDATA[(303) 249-9117]]></field>
	    <!-- Custom Field -->
	    <field name="detail.otherservices"><![CDATA[{'Other Services':{'Insurance':'Home, Auto, Renters','Administrative':'Night Deposit, Safe Deposit Box'}}]]></field>
	    <field name="detail.hoop"><![CDATA[{'Hours':{'Mon':'10a - 10p','Tue':'10a - 10p','Wed':'10a - 10p','Thu':'10a - 10p','Fri':'10a - 10p','Sat':'11a - 9p','Sun':'Closed'}}]]></field>
	    <field name="detail.services"><![CDATA[{'Banking Services':['Deposits','Withdrawls']}]]></field>
	    <field name="detail.awards"><![CDATA[{'Awards':['Fastest Card Reader','Nicest Voice']}]]></field>
	    <field name="result.hoop"><![CDATA[{'Hours':{'7 days a week':'Open 24 hours'}}]]></field>
	    <field name="result.services"><![CDATA[{'Services':['Braille','Talking ATM']}]]></field>
	</result>
	...
	</results>

#### JSON ####

	{
	"hasMore":"true",
	"results":[
		{
			"name":"ATM: 16th & Treemont",
			"phone":"(303) 249-9117",
			"streetAddress":"303 16th St. Suite 100",
			"city":"Denver",
			"state":"CO",
			"postal":"80202+1234",
			"type":"ATM",
			"distance":"0.00",
			"detailsUrl":"/co/denver/4640?q=denver%2C+co&loc=Denver%2C+CO",
			"directions":"/co%2Fdenver/4640/directions?q=Denver%2C+CO"
		}
		...
	]
	}
