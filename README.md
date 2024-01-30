# REST API specification for querying EPND resources

> This API specification is defined in the context of the EPND project, complying with the latest [Beacon v2 Specification](https://github.com/ga4gh-beacon/beacon-v2).

<!-- In this work, we present API specification for querying RD patient registries, biobanks and similar resources at the safe record level (i.e, resources whose available assets are described by RD patient data). Resources that implement this specification would ideally collect data based on the set of common data elements for rare diseases registration, as recommended by the European commission Joint Research Centre. In this specification, where possible, we also make use of ontological terms recommended by the CDE semantic data model group. -->

<hr>

<h2 id="top"> Contents </h2>

<!-- * [Try out this API in Swagger](#-try-out-the-api-) -->
* [Specification](#-specification-)
* [Query Endpoints](#-query-endpoints-)
    <!-- * [Individuals endpoint](#-individuals-endpoint)
      * [List of filters](#-list-of-filters-and-permitted-values-for-the-individuals-endpoint-)
      * [Filters description](#-individuals-filters-description-)
      * [Example request & response](#-example-request-and-response-for-individuals-)
    * [Biosamples endpoint](#-biosamples-endpoint)
      * [List of filters](#-list-of-filters-and-permitted-values-for-the-biosamples-endpoint-)
      * [Filters description](#-biosamples-filters-description-)
      * [Example request & response](#-example-request-and-response-for-biosamples-) -->
    * [Catalogs endpoint](#-catalogs-endpoint-)
      * [List of filters](#-list-of-filters-and-permitted-values-for-the-catalogs-endpoint-)
      * [Filters description](#-catalogs-filters-description-)
      * [Example request & response](#-example-request-and-response-for-catalogs-)
<!-- * [Authentication using Header(s)](#-authentication-using-header-)
* [Understanding the query](#-understanding-the-query)
    * [Syntax & Usage of Beacon Query with Filters](#-syntax-and-usage)
        * [Multi-Filter (AND) query](#beacon-queries-using-multiples-of-the-same-type-of-filter-and-logical-operator-between-filters)
        * [Same Filter (AND) query](#beacon-queries-using-multiple-values-as-in-phenotype-or-disease-filters-or-logical-operator-between-filter-values)
        * [Multi-Filter (OR) query using **Arrays**](#beacon-queries-using-multiples-of-the-same-type-of-filter-or-logical-operator-between-filters)
    * [Partial query matches with warning messages](#partial-query-matches-with-warning-messages)
* [Understanding the response with ranges (for /individuals and /biospecimens)](#-understanding-the-response-with-ranges-for-individuals-and-biospecimens) -->
* [Informational Endpoints](#-informational-endpoints-)


<hr>
 
<!-- <h2 id="#-try-out-the-api-"> Try out the API </h2>

Latest version (v2.0) of this specification is available on Swagger here: https://app.swaggerhub.com/apis/DVS6_1/virtual-platform_beacon_api/v2.0#/ 

<hr> -->

<h2 id="-specification-"> Specification </h2>

The request and response conforms to the [Beacon Reference Framework](https://github.com/ga4gh-beacon/beacon-v2). This Specification defines two types of endpoints - **[The Query Endpoints](#-query-endpoints-)** and **[The Informational Endpoints](#-informational-endpoints-)**. 

[Query Endpoints](#-query-endpoints-) require the requester to provide a JSON body and send request using the POST method. This document defines query endpoints to query resources using filters -  [/catalogs](#-catalogs-endpoint-).

[Informational Endpoints](#-informational-endpoints-) are simple GET requests without needing a request body, and respond with information relavant to this Beacon Specification. These are: /info, /configuration, /entry_types, /filtering_terms and /map. A special /service-info endpoint (also a GET request), responds with metadata relevant to this Beacon using the [GA4GH ServiceInfo format](https://github.com/ga4gh-discovery/ga4gh-service-info/). 


<hr>

<h2 id="-query-endpoints-"> Query Endpoints </h2>

This specification defines POST endpoints to request information about resources. Each endpoint makes use of the [Filters](http://docs.genomebeacons.org/filters/) capability of the Beacon API.

<!-- <h3 id="-individuals-endpoint"> Individuals endpoint</h3>

> **HTTP Request Method : POST** -->

<!-- [/individuals](#-individuals-endpoint) endpoint returns the **__maximum value of individuals within a specified range__** from a RD resource. Filters are provided as a part of the body while using a HTTP POST request to query resources. 

Please **do not use HTTP GET method** to query the individuals endpoint, as it is **not permitted** per this specification, and will result in a 403 error response.

<h4 id="-list-of-filters-and-permitted-values-for-the-individuals-endpoint-"> List of filters and permitted values for the individuals endpoint </h4>

> **Note**: Elements within arrays in **value** fields are treated as **ORs**

<table>
<thead>
        <th>CDE Concept</th>
        <th>CDE Term</th>
        <th>Beacon Filter Type</th>
        <th>ID</th>
        <th>Operator</th>
        <th>Permitted Values</th>
</thead>
<tbody>
    <tr>
        <td rowspan="5">
            <b>Sex</b>
        </td>
        <td rowspan="5">
            obo:NCIT_C28421
        </td>
        <td rowspan="5">
            Alphanumerical
        </td>
        <td rowspan="5">
            NCIT_C28421
        </td>
        <td rowspan="5">
            =
        </td>
        <td>
            NCIT_C16576
        </td>
    </tr>
    <tr>
        <td>
            NCIT_C20197
        </td>
    </tr>
    <tr>
        <td>
            NCIT_C124294
        </td>
    </tr>
    <tr>
        <td>
            NCIT_C17998
        </td>
    </tr>
    <tr>
        <td>
            An array of any of the above
        </td>
    </tr>
    <tr>
        <td><b>Disease or Disorder</b>
        </td><td>obo:NCIT_C2991</td>
        <td>Ontology</td>
        <td>A single value or an array of orphanet terms. <b>e.g. Orphanet_558 or [Orphanet_558, Orphanet_773]</b></td>
        <td colspan="2">NA</td>
    </tr>
    <tr>
        <td><b>Phenotype</b></td>
        <td>sio:SIO_010056</td>
        <td>Ontology</td>
        <td>A single value or an array of HPO terms. <b>e.g. HP_0001251 or [HP_0001251, HP_0012250]</b></td>
        <td colspan="2">NA</td>
    </tr>
    <tr>
        <td><b>Causative Genes</b></td>
        <td>edam:data_2295</td>
        <td>Alphanumerical</td>
        <td>data_2295 </td>
        <td>=</td>
        <td>any HGNC gene symbol or array of HGNC symbols</td>
    </tr>
    <tr>
      <td><b>Age this year</b></td><td>obo:NCIT_C83164</td>
        <td>Numerical</td>
        <td>NCIT_C83164 </td>
        <td>=, &gt;=, &gt;, &lt;=, &lt;</td>
        <td>any integer</td>
    </tr>
    <tr>
      <td><b>Symptom Onset</b></td><td>obo:NCIT_C124353</td>
        <td>Numerical</td>
        <td>NCIT_C124353</td>
        <td>=, &gt;=, &gt;, &lt;=, &lt;</td>
        <td>any integer</td>
    </tr>
    <tr>
      <td><b>Age at diagnosis</b></td><td>obo:NCIT_C156420</td>
        <td>Numerical</td>
        <td>NCIT_C156420</td>
        <td>=, &gt;=, &gt;, &lt;=, &lt;</td>
        <td>any integer</td>
    </tr>
</tbody>
</table>

[ ^ Back to the top](#top)

<hr>

<h3 id="-individuals-filters-description-"> Individuals Filters Description </h3>

**Sex**: The biological sex of an individual patient.

**Disease or Disorder**: All rare diseases that have been diagnosed **within an individual**, to encompase, but not distinguish between all levels of diagnosis such as definitive, differential, provisional, etc.,

**Phenotype**: HPO terms of all phenotypes observed **within an individual**.

**Causative Genes**: All genes which have been deemed as causative of one or more of the diagnosed rare diseases **in an individual**, encompassing and not distinguishing between all certainty levels of casuality.

**Age this year**: Age at the end of the current year. -/+ will be added to all age queries when executed by the query engine at the resource. 

**Symptom Onset**: Age at the manifestation of a rare disease. For individuals with more than one rare disease, this filter will look at all age of manifestations independently. -/+ will be added to all age queries when executed by the query engine at the resource. 

**Age at diagnosis**: Age at the diagnosis of a rare disease. For individuals with more than one rare disease, this filter will look at all age of manifestations independently. -/+ will be added to all age queries when executed by the query engine at the resource.

[ ^ Back to the top](#top)

<hr>

<h3 id="-example-request-and-response-for-individuals-"> Example request and response for individuals </h3>

**EXAMPLE /individuals REQUEST**

```JSON
{
    "meta": {
        "apiVersion": "v2.0"
    },
    "query": {
        "filters": [
              {
                "id": "Orphanet_34587"
              },
              {
                "id": "data_2295",
                "value": "LAMP2",
                "operator": "="
              },
              {
                "id": "NCIT_C28421",
                "operator": "=",
                "value": "NCIT_C16576"
              }
        ],
        "requestedGranularity": "boolean"
    }
}
```

**EXAMPLE /individuals RESPONSE**


```JSON
{
  "meta": {
      "apiVersion": "v2.0",
      "beaconId": "Responding unique Beacon ID in reverse domain name notation",
      "returnedGranularity": "count"
  },
  "response": {
     "resultSets": [
      {
         "id": "Vivify",
         "type": "dataset", 
         "exists": true,
         "resultCount": 80,
         "info": {
            "resultCountDescription": {
               "minRange": 71,
               "maxRange": 80
            },
            "contactPoint": "admin",
            "contactEmail": "admin@cafevariome.org", 
            "contactURL": "rdnexusdev.molgeniscloud.org/cv2/"
         }      
      }
    ]
  },
  "responseSummary":{
    "exists": true,
    "numTotalResults": 80
  }
}
```

The "resultCount" attribute in the above response is the **maximum value of whatever range that result** is within, rather than giving out the actual count of individuals. More information on responding using ranges can be found [here](#ranges). 

The filter **SHOULD** be one of the terms from the [filters and permitted values table](#individuals). Please note that not all resources will support all of the filters. In such cases the response should include a [warning message in the 'info' part](#warning-response-example) indicating which requested filters are unsupported and these were not included in the query.

**EXAMPLE warning when unsupported filters are requested**


```JSON
 {
    "meta": {
        "beaconId": "org.rd-connect.beacon",
        "apiVersion": "v2.0.0-draft.4",
        "returnedGranularity": "count",
        "receivedRequestSummary": {
            "apiVersion": "v2.0",
            "requestedSchemas": [
                {
                    "entityType": "individuals",
                    "schema": "beacon-individual-v2.0.0-draft.4"
                }
            ],
            "filters": [
                [
                    {
                        "id": [
                            "Orphanet_730",
                            "Orphanet_2730"
                        ]
                    },
                    {
                        "id": "NCIT_C28421",
                        "operator": "=",
                        "value": "NCIT_C20197"
                    },
                    {
                        "id": "data_2295",
                        "operator": "=",
                        "value": "100"
                    },
                    {
                        "id": "NCIT_C83164",
                        "operator": ">=",
                        "value": "0"
                    },
                    {
                        "id": "NCIT_C124353",
                        "operator": ">=",
                        "value": "0"
                    },
                    {
                        "id": "NCIT_C156420",
                        "operator": ">=",
                        "value": "0"
                    }
                ]
            ]
        },
        "returnedSchemas": [
            {
                "entityType": "individuals",
                "schema": "beacon-individual-v2.0.0-draft.4"
            }
        ]
    },
    "responseSummary": {
        "exists": false,
        "numTotalResults": 0
    },
    "info": {
        "warnings": {
            "unsupportedFilters": [
                "NCIT_C83164",
                "NCIT_C124353",
                "NCIT_C156420"
            ]
        }
    },
    "response": {
        "resultSets": [
            {
                "id": "datasetBeacon",
                "type": "individuals",
                "exists": false,
                "resultCount": 0
            }
        ]
    }
}
```

<h3 id="-biosamples-endpoint"> Biosamples endpoint</h3>

> **HTTP Request Method : POST**

Similarly to the `/individuals` endpoint, `/biosamples` endpoint returns the **__maximum value of biosamples within a specified range__** from an RD resource (usually the resource is a Biobank). Queries are performed in the same way as for `/individuals` endpoint, adding filters in the body of the request.

Please **do not use HTTP GET method** to query the biosamples endpoint, as it is **not permitted** per this specification, and will result in a 403 error response.

<h4 id="-list-of-filters-and-permitted-values-for-the-biosamples-endpoint-"> List of filters and permitted values for the biosamples endpoint </h4>

> **Note**: Elements within arrays in **value** fields are treated as **ORs**

<table>
<thead>
        <th>Concept</th>
        <th>Ontological Term</th>
        <th>Beacon Filter Type</th>
        <th>ID</th>
        <th>Operator</th>
        <th>Permitted Values</th>
</thead>
<tbody>
    <tr>
        <td rowspan="5"><b>Sex</b></td>
        <td rowspan="5">obo:NCIT_C28421</td>
        <td rowspan="5">Alphanumerical</td>
        <td rowspan="5">NCIT_C28421</td>
        <td rowspan="5">=</td>
        <td>NCIT_C16576</td>
    </tr>
    <tr><td>NCIT_C20197</td></tr>
    <tr><td>NCIT_C124294</td></tr>
    <tr><td>NCIT_C17998</td></tr>
    <tr><td>An array of any of the above</td></tr>
    <tr>
        <td><b>Disease or Disorder</b></td>
        <td>obo:NCIT_C2991</td>
        <td>Ontology</td>
        <td>A single value or an array of orphanet terms. <b>e.g. Orphanet_558 or [Orphanet_558, Orphanet_773]</b></td>
        <td colspan="2">NA</td>
    </tr>
    <tr>
        <td><b>Year of birth</b></td>
        <td>obo:NCIT_C83164</td>
        <td>Numerical</td>
        <td>NCIT_C83164 </td>
        <td>=, &gt;=, &gt;, &lt;=, &lt;</td>
        <td>any integer</td>
    </tr>
    <tr>
        <td><b>Age at diagnosis</b></td>
        <td>obo:NCIT_C156420</td>
        <td>Numerical</td>
        <td>NCIT_C156420</td>
        <td>=, &gt;=, &gt;, &lt;=, &lt;</td>
        <td>any integer</td>
    </tr>
    <tr>
        <td rowspan="20"><b>Biospecimen Type</b></td>
        <td rowspan="20">obo:NCIT_C70713</td>
        <td rowspan="20">Alphanumerical</td>
        <td rowspan="20">NCIT_C70713</td>
        <td rowspan="20">=</td>
        <td>OBI_0000655 (blood specimen)</td>
    </tr>
    <tr><td>OBI_0002512 (bone marrow)</td></tr>
    <tr><td>OBIB_0000036 (buffy coat)</td></tr>
    <tr><td>CL_2000001 (peripheral blood mononuclear cell)</td></tr>
    <tr><td>OBI_0100016 (blood plasma specime)</td></tr>
    <tr><td>OBI_0100017 (blood serum)</td></tr>
    <tr><td>UBERON_0007795 (ascites fluid)</td></tr>
    <tr><td>OBI_0002502 (cerebrospinal fluid)</td></tr>
    <tr><td>OBI_0002507 (saliva)</td></tr>
    <tr><td>OBI_0002503 (feces)</td></tr>
    <tr><td>OBI_0000651 (urine)</td></tr>
    <tr><td>OBI_0002599 (swab)</td></tr>
    <tr><td>OBI_2000009 (bodily fluid specimen)</td></tr>
    <tr><td>OBI_1200000 (FFPE specimen)</td></tr>
    <tr><td>OBI_0000922 (frozen specimen)</td></tr>
    <tr><td>OBI_0001472 (specimen with known storage state)</td></tr>
    <tr><td>OBI_0001051 (DNA extract)</td></tr>
    <tr><td>OBI_0000880 (RNA extract)</td></tr>
    <tr><td>OBI_0001479 (specimen from organism)</td></tr>
    <tr><td>An array of any of the above</td></tr>    
</tbody>
</table>

[ ^ Back to the top](#top)

<hr>

<h3 id="-biosamples-filters-description-"> Biosamples Filters Description </h3>

**Sex**: The biological sex of the person the biosample belongs to.

**Disease or Disorder**: All rare diseases that have been diagnosed **from the biosample**, to encompase, but not distinguish between all levels of diagnosis such as definitive, differential, provisional, etc.,

**Year of birth**: The year of birth of the person who the biosample belongs to

**Age at diagnosis**: Age at the diagnosis of a rare disease. For biosamples with more than one rare disease, this filter will look at all age of manifestations independently. -/+ will be added to all age queries when executed by the query engine at the resource.

**Biospecimen Type**: One or more biospecimen type. The list is a list of ontology terms taken from MIABIS

[ ^ Back to the top](#top)

<hr>

<h3 id="-example-request-and-response-for-biosamples-"> Example request and response for biosamples </h3>

**EXAMPLE /biosamples REQUEST**

```JSON
{
    "meta": {
        "apiVersion": "v2.0"
    },
    "query": {
        "filters": [
              {
                "id": "Orphanet_34587"
              },
              {
                "id": "obo:NCIT_C70713",
                "operator": "=",
                "value": "OBI_0000655"
              }
        ],
        "requestedGranularity": "count"
    }
}
```

**EXAMPLE /biosamples RESPONSE**


```JSON
{
    "meta": {
        "beaconId": "biobank beacon",
        "apiVersion": "v2.0.0",
        "returnedGranularity": "count",
        "receivedRequestSummary": {
            "apiVersion": "2.0",
            "filters": [
                {
                    "id": "Orphanet_34587"
                },
                {
                    "id": "obo:NCIT_C70713",
                    "operator": "=",
                    "value": "OBI_0000655"
                }
            ],
            "requestedGranularity": "count",
            "testMode": false
        },
        "returnedSchemas": [
            {
                "entityType": "biosample",
                "schema": "beacon-biosample-v2.0.0",
                "name": "Default schema for a biological sample",
                "url": "https://raw.githubusercontent.com/ga4gh-beacon/beacon-v2-Models/main/BEACON-V2-Model/biosamples/defaultSchema.json",
                "version": "v2.0.0"
            }
        ]
    },
    "responseSummary": {
        "exists": true,
        "numTotalResults": 100
    },
}
```
Notes about the `resultCount` and the filters for the `/individuals` endpoint apply also for `biospecimens`

[ ^ Back to the top](#top) -->

<hr>
<h3 id="-catalogs-endpoint-"> Catalogs endpoint </h3>

> Method: POST

[/catalogs](#-catalogs-endpoint-) endpoint returns the **__metadata of EPND resources__** as response. Filters are provided as a part of the body while using a POST request to query resources.

<h4 id="-list-of-filters-and-permitted-values-for-the-catalogs-endpoint-"> List of filters and permitted values for the catalogs endpoint </h4>

> **Note**: Elements within arrays in **value** fields are treated as **ORs**

<table>
<thead>
        <th style="background-color: #f2f2f2;">Metadata Schema Concept</th>
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
        <td>epnd:PatientRegistry</td>
    </tr>
    <tr><td>epnd:Biobank</td></tr>
    <tr><td>epnd:Guideline</td></tr>
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

<h3 id="-catalogs-filters-description-"> Catalogs endpoint Filters Description </h3>

**Disease or Disorder**: All  diseases that are associated **within a catalog**. It corresponds to the `dcat:theme` property of the Resource Metadata Schema. The values follow CURIE syntax and use the `ordo:` prefix.

**Phenotype**: HPO terms of all phenotypes observed **within a catalog** of rare disease resources. The values follow CURIE syntax and use the `HP:` prefix. 

**ID**: The resource identifier ID **within the catalog**. It corresponds to the identifier of the RDF resource

**Name**: The name of the resource in the **catalog**. It corresponds to the `dct:title` of the Resource Metadata Schema

**Description**: The description of the resource in the **catalog**. It corresponds to the `dct:description` property of the Resource Metadata Schema

**Organisation**: The organisation that owns the resouce. It corresponds to the dct:publisher property. 

**Resource Types**: Types of resources **within the catalog**. Permitted values for this filter are the type of resources in the Resource Metadata Schema:  `epnd:PatientRegistry`, `epnd:Biobank`, `epnd:Guideline`, `dcat:Datasest` or an array of any of these values.

[ ^ Back to the top](#top)

<hr>

<!-- <h3 id="catalogs-response">Catalogs Response</h3>

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

<h3 id="-example-request-and-response-for-catalogs-"> Example request and response for catalogs </h3> -->

**EXAMPLE /catalogs REQUEST**

```JSON
{ 
 "meta":{
      "apiVersion": "v2.0.1"
 },
 "query": {
      "filters": [
        {}
      ],
      "requestedGranularity": "record"
    }
}
```


Requested granularity for the response. It do not have to respond with the requested granularity, e.g. may respond with count results although record level granularity had been requested but indicate the granularity of the response in the response's metadata. Allowed values -{ boolean,count,aggregated,record} with default boolean value.

The following is an example response 

**EXAMPLE /catalogs RESPONSE**
```JSON
{
    "meta": {
        "beaconId": "org.example.beacon.v2",
        "apiVersion": "v2.0.1",
        "returnedGranularity": "record",
        "receivedRequestSummary": {
            "apiVersion": "v2.0.1",
            "requestedSchemas": [],
            "filters": [
                {}
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
            {}
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
                "results": [
                    {}
                ]
            }
        ]
    }
}
```

[ ^ Back to the top](#top)

<hr>

The "resultCount" attribute in the above response is the **maximum value of whatever range that result** is within, rather than giving out the actual count of individuals. More information on responding using ranges can be found [here](#ranges). 

The filter **SHOULD** be one of the terms from the [filters and permitted values table](#individuals). Please note that not all resources will support all of the filters. In such cases the response should include a [warning message in the 'info' part](#warning-response-example) indicating which requested filters are unsupported and these were not included in the query.

**EXAMPLE warning when unsupported filters are requested**


```JSON
{
    "meta": {
        "beaconId": "org.example.beacon.v2",
        "apiVersion": "v2.0.2",
        "returnedGranularity": "record",
        "receivedRequestSummary": {
            "apiVersion": "v2.0.0",
            "requestedSchemas": [],
            "filters": [
                {}
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
            }
        ]
    },
    "responseSummary": {
        "exists": true,
        "numTotalResults": 1
    },
    "beaconHandovers": [],
    "info": {
        "warnings": {
            "unsupportedFilters": [
                "NCIT_C83164",
                "NCIT_C124353",
                "NCIT_C156420"
            ]
        }
    },
    "response": {
        "resultSets": [
            {
                "resultsCount": 1,
                "results": [
                    {}
                ]
            }
        ]
    }
}
```




<!-- <h2 id="-authentication-using-header-"> Authentication using Header </h2>

Since the specification allows for record level queries of individuals, additional information is required in the request header to verify the requester is authorised:

<table>
<thead>
<th>Header Attribute</th>
<th>Header Value</th>
<th>Purpose</th>
</thead>
<tbody>
<tr><td>auth-key</td><td>Token provided by resource</td><td>Indicates requester is authorised (required)</td></tr>
<tr><td>auth-token</td><td>Bearer token, True, False</td><td>Indicates requesting user's logged in status (optional)</td></tr>
<tr><td>authentication-url</td><td>Bearer token authentication provider</td><td>Enables validation of bearer token (optional)</td></tr>
</tbody>
</table>

> **Note:** Presence of a bearer token is equivalent to auth-token:True

> **Note:** Implementers can provide distinct auth-keys to each requester or a single auth-key to all requesters.

To see this live in action in Swagger UI, see [Authentication in Swagger](#-authentication-using-header-for-swagger-).

[ ^ Back to the top](#top) -->

<!-- <hr> -->

<!-- <h2 id="-understanding-the-query-"> Understanding the query </h2>

<h3 id="-syntax-and-usage-"> Syntax and Usage: </h3>

> Method: POST

There are 3 types of Beacon queries that this specification currently supports: 
1. [Alphanumerical Query](http://docs.genomebeacons.org/filters/#exact-value-query)
2. [Numerical Query](http://docs.genomebeacons.org/filters/#numerical-value-query)
3. [Ontology Query](http://docs.genomebeacons.org/filters/#simple-curie-based-filters-query)

The following JSON body depicts the typical usage of filters to query resources. 
   * Alphanumeric filters **require** the use of id, operator(=) and value properties. 
   * Ontology filters only require the 'id' property to query resources.
   * Numeric filters also require id, operator and value properties. These filters differ from the alphanumeric filters as it is also possible to use comparison operators (>=,>,<=,<) along with '='.

These usage rules are illustrated using a general syntax as below:

```JSON
{
  "meta": {
    "apiVersion": "v2.0"
  },
  "query": {
    "filters": [
      {
        "id": "AlphanumericFilter_id",
        "operator": "=",
        "value":"AlphanumericFilter_value"
        
      },
      {
        "id": "AlphanumericFilter_id",
        "operator": "=",
        "value":"AlphanumericFilter_value"
      },
      { 
        "id": "OntologyFilter_value"
      }
    ],
     "requestedGranularity": "boolean"
  }
}
```

[ ^ Back to the top](#top)

<hr>

As shown above, different types of filters can be sent in a single query. These are further elucidated below. 

<h4 id="beacon-queries-using-multiples-of-the-same-type-of-filter-and-logical-operator-between-filters"> Beacon queries using multiple filters (AND logical operator between filters):</h4>

```JSON
{
"query": {
      "filters": [
        {
          "id": "Orphanet_34587" 
        },
        {
          "id": "data_2295",
          "operator": "=",
          "value": "LAMP2"
        }
      ]
    }
}
```

This filter is asking for individuals that have been diagnosed with Danon disease (Orphanet_34587) **and** where LAMP2 gene has been identified as causative. These filters are handled independently, this means that individuals with Danon disease where LAMP2 has been identified as a causative gene, specifically for Danon disease, will match the query. It also means that individuals with Danon disease and where LAMP2 has been identified as a causative gene for a second rare disease, other than Danon disease, will also match this query.


[ ^ Back to the top](#top)

<hr>

<h4 id="beacon-queries-using-multiple-values-as-in-phenotype-or-disease-filters-and-logical-operator-between-filter-values">Beacon queries using multiples of the same type of filter (AND logical operator between filters):</h4>

To query for individuals with more than one instance of any of the filters you can send multiple of the same filter, such as in the below example:

> Ontology Filter Example:

```JSON
{
"query": 
    {
      "filters": [
        {
          "id": "Orphanet_34587"
        },
        {
          "id": "Orphanet_1653"
        }
      ]
   }
}
```
This query is looking for individuals with Danon disease ("Orphanet_34587") AND Dentin dysplasia ("Orphanet_1653").

> Alphanumeric Filter example: 

```JSON
{
"query": 
    {
      "filters": [
        {
          "id": "Available Materials",
          "operator": "=",
          "value": "RNA sequence"          
        },
        {
          "id": "Available Materials",
          "operator": "=",
          "value": "Whole Genome Sequence"
        }
      ]
   }
}
```
This query is looking if there are any individuals with RNA sequence information AND Whole Genome Sequence information available.

[ ^ Back to the top](#top)

<hr>

<h4 id="beacon-queries-using-multiples-of-the-same-type-of-filter-or-logical-operator-between-filters">Beacon queries using multiples of the same type of filter (OR logical operator between filters):</h4>

> Ontology Filter using Array Example:

```JSON
{
"query": 
    {
      "filters": [
        {
          "id": ["Orphanet_34587","Orphanet_1653"]
        }
      ]
   }
}
```
This query is looking for individuals either with Danon disease (Orphanet_34587) OR Dentin dysplasia (Orphanet_1653).

> **Note**: There are no OR operators available **between** filters with beacon queries.

All of the defined filters are optional, the user can provide as many or as few as wanted and the resource does not have to implement all filters.

If a user sends a query with a filter not supported by a resource, then the resource should complete the query but ignore the unsupported filter(s) and respond as usual, but with a warning noting that certain filters were ignored as they are unsupported.

The warning messages will be provided within the [`info`](#partial-query-matches-with-warning-messages)  section of the Beacon.

[ ^ Back to the top](#top) -->

<!-- <hr> -->

<!-- <h2 id="partial-query-matches-with-warning-messages"> Partial Request & Response with warning message Example </h2>

**EXAMPLE REQUEST**

```JSON
{
  "meta":{
      "apiVersion": "v2.0"
  },
  "query":{
    "filters": [
        {
          "id": "Orphanet_34587"
        },
        {
          "id": "data_2295",
          "value": "LAMP2",
          "operator": "="
        },
        {
          "id": "NCIT_C28421",
          "operator": "=",
          "value": "NCIT_C16576"
        }
    ],
     "requestedGranularity": "boolean"
  }
}
```

This request is asking for females with Danon disease with LAMP2 being identified as a causative gene.

This request is sent to a resource which does not hold information about causative genes but does hold information about diagnoses and sex, an example response which could be returned is outlined below:

**EXAMPLE WARNING RESPONSE**

```JSON
{
  "meta": {
      "apiVersion": "v2.0",
      "beaconId": "Responding unique Beacon ID in reverse domain name notation",
      "returnedGranularity":"count"
  },
  "response": {
     "resultSets": [
      {
         "id": "Vivify",
         "type": "dataset",
         "exists": true,
         "resultCount": 20,
         "info": {
            "resultCountDescription": {
               "minRange": 11,
               "maxRange": 20
            },
            "contactPoint": "admin",
            "contactEmail": "admin@cafevariome.org",
            "contactURL": "rdnexusdev.molgeniscloud.org/cv2/"
         }   
      }
     ]
  },
  "responseSummary":{
    "exists": "true",
    "numTotalResults": 20
  },
  "info": { 
    "warnings":{
      "unsupportedFilters": [
        "data_2295"
      ]
    }
  }
}
```

This response provides a warning message within the info section advising of unsupported filters which were ignored when the query was processed by the resources query engine. Please see the info part of the [IndividualResponse](https://app.swaggerhub.com/apis/DVS6_1/virtual-platform_beacon_api/v2.0#/IndividualResponse) schema on swagger. 


[ ^ Back to the top](#top)

<hr>

<h2 id="-understanding-the-response-with-ranges-for-individuals-and-biospecimens"> Understanding the response with ranges (for /individuals and /biospecimens)</h2>

In the examples of the response for the `/individuals` and `/biosamples` endpoints, information of the resultant dataset matching the query is provided within the **`resultSets`** attribute. 

To provide flexibility for implementers between using a range, the `info` section of each resultant dataset in `resultSets` need to conform to the following standardised structure:

```JSON
"info": {
   "resultCountDescription": {
      "minRange": N,
      "maxRange": N
   },
   "contactPoint": "Person/point of contact",
   "contactEmail": "Email for contact regarding this dataset/resource", 
   "contactURL": "URL of the implementer"
}
```

> **Note**: Here, N is an integer where the implementer can respond with **"minRange" & "maxRange"** (if employing a range) - the maximum value of whatever range that result is within, whereupon the "maxRange" value should match the "resultCount" value and the "numTotalResults" value.

**Example response employing a range:**

```JSON
{
  "meta": {
      "apiVersion": "v2.0",
      "beaconId": "Responding unique Beacon ID in reverse domain name notation",
      "returnedGranularity": "count"
  },
  "response": {
     "resultSets": [
      {
         "id": "Vivify",
         "type": "dataset", 
         "exists": true,
         "resultCount": 80,
         "info": {
            "resultCountDescription": {
               "minRange": 71,
               "maxRange": 80
            },
            "contactPoint": "admin",
            "contactEmail": "admin@cafevariome.org", 
            "contactURL": "rdnexusdev.molgeniscloud.org/cv2/"
         }      
      }
    ]
  },
  "responseSummary":{
    "exists": true,
    "numTotalResults": 80
  }
}
```

In this example, the result could be a count of individuals between 71 to 80 (the resource only responds with ranges of size 10).


[ ^ Back to the top](#top) -->

<hr>



<h2 id="-informational-endpoints-"> Informational Endpoints (Mandatory to be implemented for all the resources )</h2>

This specification defines GET endpoints to request information about resources.

<h3 id="-info-endpoint"> Info endpoint</h3>

> **HTTP Request Method : GET**

/info returns the information about the Beacon.

<h3 id="-example-request-and-response-for-info-"> Example response for info </h3>




```JSON

{
  "meta": {
    "apiVersion": "v2.0.1",
    "beaconId": "org.example.beacon.v2",
    "returnedSchemas": [
      {}
    ]
  },
  "response": {
    "alternativeUrl": "https://example.org/beacon/authenticated",
    "apiVersion": "v2.0.1",
    "createDateTime": "2014-07-19",
    "description": "string",
    "environment": "prod",
    "id": "org.example.beacon.v2",
    "info": {},
    "name": "string",
    "organization": {
      "address": "string",
      "contactUrl": "string",
      "description": "string",
      "id": "string",
      "info": {},
      "logoUrl": "string",
      "name": "string",
      "welcomeUrl": "string"
    },
    "updateDateTime": "2014-07-19",
    "version": "v2.0.1",
    "welcomeUrl": "https://example.org/wiki/Main_Page"
  }
}

```

<h3 id="service-info-endpoint"> Service-info endpoint</h3>

> **HTTP Request Method : GET**

/service-info returns the information about the basic metadata concerning its service, based on the [reference specification](https://github.com/ga4gh-discovery/ga4gh-service-info/).

<h3 id="-example-request-and-response-for-service-info"> Example response for service-info </h3>




```JSON
{
  "id": "org.ga4gh.myservice",
  "name": "My project",
  "type": {
    "group": "org.ga4gh",
    "artifact": "beacon",
    "version": "1.0.0"
  },
  "description": "This service provides...",
  "organization": {
    "name": "My organization",
    "url": "https://example.com"
  },
  "contactUrl": "mailto:support@example.com",
  "documentationUrl": "https://docs.myservice.example.com",
  "createdAt": "2019-06-04T12:58:19Z",
  "updatedAt": "2019-06-04T12:58:19Z",
  "environment": "test",
  "version": "1.0.0"
}
```
<h3 id="configuration-endpoint">Configuration endpoint</h3>

> **HTTP Request Method : GET**

/configuration returns Beacon configuration.

<h3 id="-example-request-and-response-for-configuration"> Example response for service-info </h3>



```JSON
{
  "meta": {
    "apiVersion": "v2.0.1",
    "beaconId": "org.example.beacon.v2",
    "returnedSchemas": [
      {}
    ]
  },
  "response": {
    "$schema": "string",
    "entryTypes": {
      "property1": {
      "id": "string",
      "name": "string",
      "ontologyTermForThisType": {},
      "partOfSpecification": "Beacon v2.0",
      "defaultSchema": {},
      },
      "property2": {
      "id": "string",
      "name": "string",
      "ontologyTermForThisType": {},
      "partOfSpecification": "Beacon v2.0",
      "defaultSchema": {},
       
      }
    },
    "maturityAttributes": {
      "productionStatus": "DEV"
    },
    "securityAttributes": {
      "defaultGranularity": "boolean",
      "securityLevels": [
        null
      ]
    }
  }
}
```
<h3 id="entry-types-endpoint">Entry-types endpoint</h3>

> **HTTP Request Method : GET**

/entry-types returns list of entry types in this Beacon.

<h3 id="example-request-and-response-for-entry-types"> Example response for entry-types </h3>





```JSON
{
  "meta": {
    "apiVersion": "v2.0.1",
    "beaconId": "org.example.beacon.v2",
    "returnedSchemas": [
      {}
    ]
  },
  "response": {
    "entryTypes": {
      "property1": {
        "id": "",
        "name": "",
        "ontologyTermForThisType": {
                },
                "partOfSpecification": "v2.0",
                "defaultSchema": {
                }
      },
      "property2": {
        "id": "",
        "name": "",
        "ontologyTermForThisType": {
                },
                "partOfSpecification": "v2.0",
                "defaultSchema": {
                }
      },
    }
  }
}
```
<h3 id="filtering_terms-endpoint">Filtering_terms endpoint</h3>

**HTTP Request Method : GET**

/filtering_terms returns information about available individual filters for this beacon's entry types.

<h3 id="example-request-and-response-for-filtering_terms"> Example response for filtering_terms </h3>


```JSON
{
  "meta": {
    "apiVersion": "v2.0.1",
    "beaconId": "org.example.beacon.v2",
    "returnedSchemas": [
      {}
    ]
  },
  "response": {
    "filteringTerms": [
      {}
    ],
    "resources": [
      {}
    ]
  }
}
``` 

<h3 id="map-endpoint">Map endpoint</h3>

> **HTTP Request Method : GET**

/map returns the Beacon map with information related to the list of endpoints included in this Beacon instance.

<h3 id="example-request-and-response-for-map"> Example response for map </h3>



```JSON
{
  "meta": {
    "apiVersion": "v2.0.1",
    "beaconId": "org.example.beacon.v2",
    "returnedSchemas": [
      {}
    ]
  },
  "response": {
    "$schema": "string",
    "endpointSets": {
      "property1": {
       "entryType": "",
       "rootUrl": "",
       "filteringTermsUrl": ""
      },
      "property2": {
       "entryType": "",
       "rootUrl": "",
       "filteringTermsUrl": ""
      }
    }
  }
}
```

[ ^ Back to the top](#top)

<!-- <hr>

<h2 id="swagger-auth"> Authentication using Header for Swagger </h2>

In Swagger, to query using both the /individuals endpoint & /catalogs endpoint (which are POST requests), you have to authorize the query using the **Authorize** button (extreme right, beside Servers dropdown in Swagger UI). 

![image](https://user-images.githubusercontent.com/24955128/203320000-a9cbc5a5-4c49-4a2b-8666-4e0cb17a5a62.png)

![image](https://user-images.githubusercontent.com/24955128/205334330-c90f760d-5d1e-4685-8640-030787233454.png)

Use one of the authentication token provided to perform record level queries.
![image](https://user-images.githubusercontent.com/24955128/205334443-fce92738-a515-4f0e-8f9f-b701561c283b.png)

> Developers Note: No matter the user agent being used to query (i.e., SwaggerUI, Postman, cURL, etc.,), the authentication header **auth-key is required**. 

[ ^ Back to the top](#top)

<hr> -->
