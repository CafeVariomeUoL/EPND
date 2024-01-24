# EPND

<h2 id="-specification-"> Specification </h2>

<h3 id="-catalogs-endpoint-"> Catalogs endpoint </h3>

> Method: POST

/catalogs endpoint returns the **__metadata of resources__**,  as response, a model compatible with the Resource Metadata Schema. Filters are provided as a part of the body while using a POST request to query resources. Available filters correspond also to dcat properties from the Resource Metadata Schema

<h4 id="-list-of-filters-and-permitted-values-for-the-catalogs-endpoint-"> List of filters and permitted values for the catalogs endpoint </h4>

> **Note**: Elements within arrays in **value** fields are treated as **ORs**

<table>
<thead>
        <th>Metadata Schema Concept</th>
        <th>Metadata Schema Term</th>
        <th>Beacon Filter Type</th>
        <th>ID</th>
        <th>Operator</th>
        <th>Permitted Values</th>
</thead>
<tbody>
    <tr>
        <td><b>Disease or Disorder</b></td>
        <td>dcat:theme</td>
        <td>Ontology</td>
        <td>A single value or an array of orphanet terms in CURIE syntax prefixed with `ordo:`<b>e.g. ordo:Orphanet_558 or [ordo:Orphanet_558, ordo:Orphanet_773]</b></td>
        <td colspan="2">NA</td>
    </tr>
    <tr>
        <td><b>Phenotype</b></td>
        <td>sio:SIO_010056</td>
        <td>Ontology</td>
        <td>A single value or an array of HPO terms prefixed with `HP:` <b>e.g. HP:0001251 or [HP:0001251, HP:0012250]</b></td>
        <td colspan="2">NA</td>
    </tr>
    <tr>
        <td rowspan="5"><b>Resource Types</b></td>
        <td rowspan="5">rdf:type</td>
        <td rowspan="5">Alphanumerical</td>
        <td rowspan="5">A single value or an array of values representing a resource type of the resource. It must be one of the types defined in EJP Resource Metadata Schema</td>
        <td rowspan="5">=</td>
        <td>ejprd:PatientRegistry</td>
    </tr>
    <tr><td>ejprd:Biobank</td></tr>
    <tr><td>ejprd:Guideline</td></tr>
    <tr><td>dcat:Dataset</td></tr>
    <tr><td>An array of any of the above</td></tr>
    <tr>
        <td><b>ID</b></td> 
        <td>NA</td>
        <td>Alphanumerical</td>
        <td>id</td>
        <td>=</td>
        <td>any String</td>
    </tr>
    <tr>
        <td><b>Name </b></td>
        <td>dct:title</td>
        <td>Alphanumerical</td>
        <td>The name of the resource</td>
        <td>=</td>
        <td>any String</td>
    </tr>
    <tr>
        <td><b>Description </b> </td>
        <td>dct:description</td>
        <td>Alphanumerical</td>
        <td>The description of the resource</td>
        <td>=</td>
        <td>any String</td>
    </tr>
</tbody>
</table>

[ ^ Back to the top](#top)

<hr>

<h3 id="-catalogs-filters-description-"> Catalogs Filters Description </h3>

**Disease or Disorder**: All rare diseases that are associated **within a catalog**. It corresponds to the `dcat:theme` property of the Resource Metadata Schema. The values follow CURIE syntax and use the `ordo:` prefix.

**Phenotype**: HPO terms of all phenotypes observed **within a catalog** of rare disease resources. The values follow CURIE syntax and use the `HP:` prefix. 

**ID**: The resource identifier ID **within the catalog**. It corresponds to the identifier of the RDF resource

**Name**: The name of the resource in the **catalog**. It corresponds to the `dct:title` of the Resource Metadata Schema

**Description**: The description of the resource in the **catalog**. It corresponds to the `dct:description` property of the Resource Metadata Schema

**Organisation**: The organisation that owns the resouce. It corresponds to the dct:publisher property. 

**Resource Types**: Types of resources **within the catalog**. Permitted values for this filter are the type of resources in the Resource Metadata Schema:  `ejprd:PatientRegistry`, `ejprd:Biobank`, `ejprd:Guideline`, `dcat:Datasest` or an array of any of these values.

[ ^ Back to the top](#top)

<hr>

<h3 id="catalogs-response">Catalogs Response</h3>

The response is a Beacon Collection response that corresponds to a Resource described by the Resource Metadata Schema. Depending on the resource type, the properties may slighlty differ: for example some resource types can have properties that others don't have. Notice that an important field in all resources is the `@context` that specifies the semantics of the properties returned. It must be the [link](https://raw.githubusercontent.com/ejp-rd-vp/vp-api-specs/main/json-ld-contexts/ejprd-context.json) to the `json-ld-contexts/ejprd-context.json` file  in this repository. The schemas for each specific resource are in the `/schemas` directory.
In the meta section of the response, the `returnedSchemas` object must specify the correct json schema for the resource. An example is:

```JSON
"returnedSchemas": [
    {
        "entityType": "resources",
        "schema": "ejprd-biobank-registry-v1.0.0",
        "name": "EJPRD schema for biobank and patient registry",
        "url": "https://raw.githubusercontent.com/ejp-rd-vp/vp-api-specs/main/schemas/biobank-registry-schema.json",
        "version": "v1.0.0"
    }
]
```

<h3 id="-example-request-and-response-for-catalogs-"> Example request and response for catalogs </h3>

**EXAMPLE /catalogs REQUEST**

```JSON
{ 
 "meta":{
      "apiVersion": "v2.0"
 },
 "query": {
      "filters": [
        {
          "id": "ordo:Orphanet_730"
        },
        {
          "id": "rdf:type",
          "operator": "=",
          "value": "ejprd:Biobank"

        }
      ],
      "requestedGranularity": "record"
    }
}
```

The following is an example response 

**EXAMPLE /catalogs RESPONSE**
```JSON
{
    "meta": {
        "beaconId": "ejprd.beacon.directory.bbmri-eric.eu",
        "apiVersion": "v2.0.0",
        "returnedGranularity": "record",
        "receivedRequestSummary": {
            "apiVersion": "2.0",
            "requestedSchemas": [],
            "filters": [
                {
                    "id": "ordo:Orphanet_730"
                },
                {
                "id": "rdf:type",
                "operator": "=",
                "value": "ejprd:Biobank"

                }
            ],
            "requestParameters": {},
            "includeResultsetResponses": "HIT",
            "pagination": {
                "skip": 0,
                "limit": 50
            },
            "requestedGranularity": "record",
            "testMode": false
        },
        "returnedSchemas": [
            {
                "entityType": "resources",
                "schema": "ejprd-resources-v1.0.0",
                "name": "EJPRD schema for resources",
                "url": "https://raw.githubusercontent.com/ejp-rd-vp/vp-api-specs/main/schemas/biobank-registry-schema.json",
                "version": "v1.0.0"
            }
        ]
    },
    "responseSummary": {
        "exists": true,
        "numTotalResults": 1
    },
    "beaconHandovers": [],
    "response": {
        "resultSets": [
            {
                "resultsCount": 1,
                "results": [{
                    "@context": "https://raw.githubusercontent.com/ejp-rd-vp/vp-api-specs/main/json-ld-contexts/ejprd-context.json",
                    "@id": "biobank-1:collection:collection-1",
                    "@type": "ejprd:Biobank",
                    "title": "Rare Disease Biobank",
                    "logo": "http://raredisease.biobank.eu/logo.png",
                    "description": "Rare disease biobank with data about muscular distrophy",
                    "populationCoverage": "European",
                    "theme": "ordo:Orphanet_730",
                    "vpConnection": "ejprd:VPContentDiscovery",
                    "landingPage": ["http://biobank.raredisease.org"],
                    "personalData": "true",
                    "language": "EN",
                    "publisher": {
                        "@id": "biobank-1",
                        "title": "Biobank hosting collection",
                        "description": "The biobank that hosts the collection",
                        "spatial": {
                            "title": "Italy"
                        }
                    }
                }]
            }
        ]
    }
}
```

[ ^ Back to the top](#top)

<hr>