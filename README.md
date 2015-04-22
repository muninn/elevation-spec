# Elevation Service Specification

## Gross Specs

* Works through Apache.
* If /proc/loadavg > 5, then return 503 Service Unavailable
* Uses HTTP GET from an endpoint 'http://.../endpoint'

* Arguments are BBOX=4.069061279296875,50.86924482345238,5.13885498046875,51.33661771600667 (Long1,Lat1,Long2,Lat2).
* Arguments are Scale=x where x is the distance in meters between grid points.
* Format Arguments are either: format=rdf,n3,png or controlled through 'Accept:' http header. Any error on this parameter is reported as "406 Not Acceptable".
* HTTP cache headers are based on the latest date of the data sources.
* Source data has to be both STRM and other API's (TBD)

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
            <prov:SoftwareAgent>
            </prov:wasGeneratedBy>
            <prov:hadMember>
                <!-- This point extracted from another dataset. -->
                <geo:Point rdf:resource="http://localhost/answer/point/xx"/>
                <geo:long>....</geo:long>
                <geo:lat>....</geo:lat>
                <prov:wasInformedBy rdf:resource="http://localhost/api/datasets/A"/>
            </geo:Point>
        </prov:hadMember>
        <prov:hadMember>
            <!-- This point extracted inferred by an algorithm and another dataset. -->
            <geo:Point rdf:resource="http://localhost/answer/point/xx"/>
            <geo:long>....</geo:long>
            <geo:lat>....</geo:lat>
            <prov:wasGeneratedBy rdf:resource="http://localhost/api/xx"/>
            <prov:wasInformedBy rdf:resource="http://localhost/api/datasets/B"/>
        </geo:Point>
    </prov:hadMember>
</prov:SoftwareAgent>undefined</prov:Collection>
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
