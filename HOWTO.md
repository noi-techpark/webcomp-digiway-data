# How to

Repository for the new Webcomponent to Display Digiway Data.  
Note: Please pass to all calls done to any Open Data Hub Api a parameter with 'origin=webcomp-digiway-data'

Old webcomponent can be found here
https://webcomponents.opendatahub.testingmachine.eu/webcomponent/digiway-data-map

## Datasets to display

### Content Api

#### Source: 'civis.geoserver'
 - cyclewaystyrol (Cycleways of North Tyrol)
 - hikingtrails  (HikingTrails of South Tyrol)
 - intermunicipalcyclingroutes (Intermunicipality Cycling Routes of South Tyrol)
 - mountainbikeroutes (MountainbikeRoutes of South Tyrol)
 
#### Source: 'siat.provincia.tn.it'
 - elementi_cicloviari_v (CyclingRoutes of Trento)
 - mtb_percorsi_v (MountainbikeRoutes of Trento)
 - sentieri_della_sat (HikingTrails of Trento)

#### Source: 'dservices3.arcgis.com'
 - accessibletrails_austria (Hikingtrails of Austria (Accessible))
 - hikintrail_e5 (HikingTrail E5) 
 - radrouten_tirol (Cycleways of North Tyrol)

#### Source: 'digiway.zoho'
 - Announcements (Trail Closures of Trento)

#### Source: 'Province BZ'
 - Weatherforecast of South Tyrol

### TimeSeries Api

#### Source: 'PeopleCounter'
  - Sensor Peoplecounter

## Content REST Api

Requests of all data by source:  
  
Get Source Civis Geoserver Data
`https://tourism.api.opendatahub.testingmachine.eu/v1/ODHActivityPoi?source=civis.geoserver`  
Get SIAT TN Data  
`https://tourism.api.opendatahub.testingmachine.eu/v1/ODHActivityPoi?source=siat.provincia.tn.it`  
GET DSERVICES3 ARCGIS Data  
`https://tourism.api.opendatahub.testingmachine.eu/v1/ODHActivityPoi?source=dservices3.arcgis.com`  
GET Announcements  
`https://tourism.api.opendatahub.testingmachine.eu/v1/Announcement?source=digiway.zoho`  
  
The REST Api Endpoints gives GPS Points for each requested resource.  
If there are Geometries available they are listed in
```javascript
"GpsTrack": [
{
"Id": "radrouten_tirol_12",
"Type": "Track",
"Format": "geojson",
"GpxTrackUrl": "https://tourism.api.opendatahub.testingmachine.eu/v1/GeoShape/radrouten_tirol_12",
"GpxTrackDesc": null
}
]
```
That means the geometry is stored in another Endpoint and has to be retrieved.

Geometries Call:  
```https://api.tourism.testingmachine.eu/v1/GeoShape/{ID}```  
The Geometries can be retrieved on the GeoShapes Endpoint
  
  
Detail Call:  
Simply pass the ID of the Object and get the Detail
`https://tourism.api.opendatahub.testingmachine.eu/v1/ODHActivityPoi/{ID}`  
`https://tourism.api.opendatahub.testingmachine.eu/v1/Announcement/{ID}`


>The whole Digiway Data on the Endpoints ODHActivityPoi and GeoShape are obsolete let us use the new Endpoint `SpatialData`, see Opendatahub Geo Api.
  
## Timeseries REST Api
  
People Counter 
```https://mobility.api.opendatahub.testingmachine.eu/v2/flat,node/PeopleCounter/*/latest?origin=webcomp-tourism-digiway```
  
## Opendatahub Geo Api
  
To avoid performance Issues and to not have Quota Issues let's use the Geo Api to display the data.
> [!IMPORTANT] The REST Api is only for displaying Detail Data, the List Calls are replaced by the Geo Api.
  
Repository:  
`https://github.com/noi-techpark/opendatahub-geo-api`


Endpoint: `https://geo.api.opendatahub.testingmachine.eu`  
Swagger: `https://geo.api.opendatahub.testingmachine.eu/swagger`  
Examples: `https://geo.api.opendatahub.testingmachine.eu/examples/`  
  
To visualize the Data on the map as points or their Geometries we recommend to use the Geo Api.   
To visualize the Detail of each Data we recommend to use the REST Api.
  
I prepared examples on how to display the data.  

Check out the README of the Repo for using the geo api.
  
>For HikingTrails / Cycleways etc... we now use the new Endpoint 'SpatialData' instead of the 'ODHActivityPoi/Geoshape' Endpoints.
The Announcement Endpoint remains the same.
  
We have 3 `operationmodes` available  
'points'  
'tracks'  
'pointsandtracks'  
--> by default Tracks are visible at zoomlevel 12 but this can be modified with the parameter `displaytracksonzoomlevel`
  
>WeatherForecast Endpoint is only available on the REST Api because it is served directly from the province and cannot be processed as VectorTiles
TimeSeries.  
People Counter is only availabile on the Timeseries REST Api (WIP in the near future Geo Api will support also Timeseries)
  
There are 2 datasets on the SpatialData Endpoint which are very large (~600k)   
source `euregio.roadnetwork`  
source `euregio.routes`
 
It is also possible to retrieve them but service may be slow and some caching has to be introduced (WIP)

