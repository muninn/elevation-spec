# Elevation Service Specification

## Gross Specification

| Feature | Status |
| ------- | :----: |
| Works through Apache. | Done |
| Uses HTTP GET from an endpoint. | Done |
| If /proc/loadavg > 5 then return ```503 Service Unavailable```. | Not Done |
| HTTP cache headers are based on the latest date of the data sources. | Not Done |

## Data Sources 

| Source   | Currently Included |
| :------: | :----------------: |
| [SRTM](http://gis-lab.info/data/srtm-tif/)                                                             | Yes |
| [Nova Scotia DEM](http://novascotia.ca/natr/meb/download/dp055.asp)                                    | No |
| [NYC DEM](https://data.cityofnewyork.us/City-Government/1-foot-Digital-Elevation-Model-DEM-/dpc8-z3jc) | No |
| [Canadian Elevation Data API](http://geogratis.gc.ca/site/eng/elevation)                               | No |

## Request Parameters

| Name            | Description                        | Example                          | Status   |
| :-------------: | :--------------------------------: | :------------------------------: | :------: |
| bbox            | ```minlong,minlat,maxlong,maxlat```| ```bbox=4.06,50.86,5.13,51.33``` | Done     |
| format          | ```rdf/xml|n3|ntriples|xyz|png```  | ```format=rdf/xml```             | Done     |
| scale           | Distance between points in metres. | ```scale=1```                    | Not Done |

Note: format arguments are either ```format=rdf/xml|n3|xyz|png``` or controlled through an HTTP ```Accept``` header. Any error on this parameter is reported as ```406 Not Acceptable```.

## Provenance Information

RDF Provenance information has to be created for each data source.

Example:

```RDF/XML
<prov:Collection rdf:resource="http://localhost/answer/xx">
  <rdf:type rdf:resource="void:Dataset"/>
  <prov:wasGeneratedBy>
    <prov:SoftwareAgent rdf:about="http://localhost/api/xx" >
      <prov:wasGeneratedBy rdf:about="http://localhost/api/about" />
      <rdfs:label xml:lang="en">Elevation Server API process ID xxxxx</rdfs:label>
      <prov:startedAtTime>2014-07-14T01:01:01Z</prov:startedAtTime>
      <prov:endedAtTime>2014-07-14T01:01:03Z</prov:endedAtTime>
      <prov:wasInformedBy rdf:resource="http://localhost/api/datasets/A"/>
      <prov:wasInformedBy rdf:resource="http://localhost/api/datasets/B"/>
      <prov:wasInformedBy rdf:resource="http://localhost/api/datasets/C"/>
    </prov:SoftwareAgent>
  </prov:wasGeneratedBy>
  <prov:hadMember>
    <!-- This point extracted from another dataset. -->
    <geo:Point rdf:resource="http://localhost/answer/point/xx">
      <geo:long>....</geo:long>
      <geo:lat>....</geo:lat>
      <prov:wasInformedBy rdf:resource="http://localhost/api/datasets/A"/>
    </geo:Point>
  </prov:hadMember>
  <prov:hadMember>
    <!-- This point extracted inferred by an algorithm and another dataset. -->
    <geo:Point rdf:resource="http://localhost/answer/point/xx">
      <geo:long>....</geo:long>
      <geo:lat>....</geo:lat>
      <prov:wasGeneratedBy rdf:resource="http://localhost/api/xx"/>
      <prov:wasInformedBy rdf:resource="http://localhost/api/datasets/B"/>
    </geo:Point>
  </prov:hadMember>
  ...
</prov:Collection>
```

## Permanent Metadata

Permanent metadata hangs off the endpoint as a static document at ```http://.../endpoint/about```.

Example:

```RDF/XML
<prov:Activity rdf:about="http://localhost/api/about">
  <rdfs:label xml:lang="en">Elevation Server API Process</rdfs:label>
  <rdf:type rdf:resource="http://www.w3.org/ns/prov#Activity"/>
  <rdf:type rdf:resource="http://purl.oclc.org/NET/ssnx/ssn#Process"/>
  <prov:wasAssociatedWith rdf:resource="http://rdf.muninn-project.org/ww1/2011/11/11/Organization/Muninn"/>
  <foaf:name xml:lang="en">Elevation Server API Process</foaf:name>
  <doap:Version>2.0</doap:Version>
  <rdf:type rdf:resource="http://www.w3.org/ns/prov#SoftwareAgent"/>
  <rdf:type rdf:resource="http://usefulinc.com/ns/doap#Project"/>
  <doap:service-endpoint rdf:resource="http://localhost/api"/>
  <doap:developer rdf:resource="http://localhost/markfarrell"/>
  <doap:homepage rdf:resource="http://localhost/"/>
  <doap:blog rdf:resource="http://blog.muninn-project.org/"/>
  <doap:description xml:lang="en">The Elevation Server API will ...</doap:description>
  <cc:attributionName xml:lang="en">The Muninn Project</cc:attributionName>
  <cc:attributionURL rdf:resource="http://www.muninn-project.org/"/>
</prov:Activity>
```
