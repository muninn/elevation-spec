# Elevation Service Specification

## Gross Specs

* Works through Apache.
* If /proc/loadavg > 5, then return 503 Service Unavailable
* Uses HTTP GET from an endpoint 'http://.../endpoint'
* Permanent metadata hangs off the enpoint as a static document 'http://.../endpoint/about'
* Arguments are BBOX=4.069061279296875,50.86924482345238,5.13885498046875,51.33661771600667 (Long1,Lat1,Long2,Lat2).
* Arguments are Scale=x where x is the distance in meters between grid points.
* Format Arguments are either: format=rdf,n3,png or controlled through 'Accept:' http header. Any error on this parameter is reported as "406 Not Acceptable".
* HTTP cache headers are based on the latest date of the data sources.
* Source data has to be both STRM and other API's (TBD)
* RDF Provenance information has to be created for each data source. (TBD)

