# REST API specification for querying EPND resources

> This API specification is defined in the context of the EPND project, complying with the latest [Beacon v2 Specification](https://github.com/ga4gh-beacon/beacon-v2).


<hr>

<h2 id="top"> Contents </h2>

* [Try out this API in Swagger](#-try-out-the-api-)
* [Specification](#-specification-)
* [Query Endpoints](#-query-endpoints-)
    * [Study endpoint](#-s-endpoint-)
      * [List of filters](#-list-of-filters-and-permitted-values-for-the-study-endpoint-)
      <!-- * [Filters description](#-study-filters-description-) -->
      * [Example request & response](#-example-request-and-response-for-study-)
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
 
<h2 id="#-try-out-the-api-"> Try out the API </h2>

Latest version (v2.0) of this specification is available on Swagger here: https://app.swaggerhub.com/apis/deepthivs/EPND/v2.0#/Query%20Endpoints/study_request

<hr>

<h2 id="-specification-"> Specification </h2>

The request and response conforms to the [Beacon Reference Framework](https://github.com/ga4gh-beacon/beacon-v2). This Specification defines two types of endpoints - **[The Query Endpoints](#-query-endpoints-)** and **[The Informational Endpoints](#-informational-endpoints-)**. 

[Query Endpoints](#-query-endpoints-) require the requester to provide a JSON body and send request using the POST method. This document defines query endpoints to query resources using filters -  [/study](#-study-endpoint-).

[Informational Endpoints](#-informational-endpoints-) are simple GET requests without needing a request body, and respond with information relavant to this Beacon Specification. These are: /info, /configuration, /entry_types, /filtering_terms and /map. A special /service-info endpoint (also a GET request), responds with metadata relevant to this Beacon using the [GA4GH ServiceInfo format](https://github.com/ga4gh-discovery/ga4gh-service-info/). 


<hr>

<h2 id="-query-endpoints-"> Query Endpoints </h2>

This specification defines POST endpoints to request information about resources. Each endpoint makes use of the [Filters](http://docs.genomebeacons.org/filters/) capability of the Beacon API.


<hr>
<h3 id="-study-endpoint-"> Study endpoint </h3>

> Method: POST

[/study](#-study-endpoint-) endpoint returns the **cohorts of EPND resources** as response. Filters are provided as a part of the body while using a POST request to query resources.

<h4 id="-list-of-filters-and-permitted-values-for-the-study-endpoint-"> List of filters and permitted values for the study endpoint </h4>

> **Note**: Elements within arrays in **value** fields are treated as **ORs** and always use logical **AND** between the query parameters ,i.e all conditions in the query have to be met.


<table>
    <thead>
        <tr>
            <th>Metadata Schema Concept</th>
            <th>Beacon Filter Type</th>
            <th>ID</th>
            <th>Operator</th>
            <th>Query Values</th>
            <th>Expansion</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="13"><b>Available Diseases</b></td>
            <td rowspan="13">Alphanumerical</td>
            <td rowspan="13">NCIT_C2991</td>
            <td rowspan="13">=</td>
            <td>CG</td>
            <td>Cognitively normal</td>
        </tr>
        <tr><td>AD</td><td>Alzheimer's disease</td></tr>
        <tr><td>PD</td><td>Parkinson's disease</td></tr>
        <tr><td>DLB</td><td>Dementia with Lewy bodies</td></tr>
        <tr><td>iRBD</td><td>Isolated REM sleep behavior disorder </td></tr>
        <tr><td>CAA</td><td>Cerebral amyloid angiopathy </td></tr>
        <tr><td>FTD</td><td>Frontotemporal dementia </td></tr>
        <tr><td>ALS</td><td>Amyotrophic lateral sclerosis </td></tr>
        <tr><td>PSP</td><td>Progressive supranuclear palsy </td></tr>
        <tr><td>CBD</td><td>Corticobasal degeneration </td></tr>
        <tr><td>MSA</td><td>Multiple system atrophy </td></tr>
        <tr><td>HD</td><td>Huntington's disease</td></tr>
        <tr><td>AT</td><td>Ataxia</td></tr>
    <tr>
        <td rowspan="7"><b>Available Samples</b>
        </td>
        <td rowspan="7">Alphanumerical</td>
        <td rowspan="7">NCIT_C43412</td>
        <td rowspan="7">=</td>
        <td>
        CSF
        </td>
    </tr>
    <tr><td>Serum</td></tr>
    <tr><td>Plasma</td></tr>
    <tr><td>DNA</td></tr>
    <tr><td>Saliva</td></tr>
    <tr><td>Faeces</td></tr>
     <tr><td>Urine</td></tr>
    <tr>
        <td rowspan="4"><b>Available Imaging</b></td>
        <td rowspan="4">Alphanumerical</td>
        <td rowspan="4">NCIT_C164232</td>
        <td rowspan="4">=</td>
        <td>MRI</td>
    </tr>
    <tr><td>PET-Amyloid</td></tr>
    <tr><td>PET-Tau</td></tr>
    <tr><td>DaT Scan</td></tr>
    <tr>
        <td rowspan="2">
            <b>Cohort Design</b>
        </td>
        <td rowspan="2">
            Custom
        </td>
        <td rowspan="2">
            cognitive_data
        </td>
        <td rowspan="2">
            =
        </td>
        <td>
            Cross-sectional
        </td>
    </tr>
    <tr>
        <td>
            Logitudinal
        </td>
    </tr>
    <tr>
      <td><b>Number of participants</b></td>
        <td>Custom</td>
        <td>number_of_participants</td>
        <td>>,<,=</td>
        <td>any integer</td>
    </tr>
    <tr>
      <td><b>Country</b></td>
        <td>Alphanumerical</td>
        <td>ISO_3166-1</td>
        <td>=</td>
        <td>https://www.iso.org/obp/ui/#search</td>
    </tr>
  

</tbody>
</table>


[ ^ Back to the top](#top)
<hr>



<h3 id="-study-filters-description-"> Study endpoint Filters Description </h3>

**Available Diseases**: A single value or an array of diseases, eg : AD or [CG,AD] .

**Available Samples**: A single value or an array of samples, eg : Serum or [Serum,Plasma] .

**Available Imaging**: A single value or an array of imaging technique, eg : MRI or [MRI,PET-Amyloid] .

**Cohort Design**: A single value or an array of cohort design, eg : Cross-sectional or [Cross-sectional,Longitudinal] .

**Number of Participants**: The number of participants in the cohort . eg : >10 ,<20 or =100.

**Country**: A single value or an array of two-letter country codes eg : AF OR [AF,AT]

[ ^ Back to the top](#top)

<hr>

<h3 id="-example-request-and-response-for-study-"> Example request and response for study </h3>

**EXAMPLE /study REQUEST**

```JSON
{ 
 "meta":{
      "apiVersion": "v2.0.1"
 },
 "query": {
      
"filters": [
             {
               "id": "NCIT_C2991",
               "operator": "=",
               "value": ["CG","AD"]
             },
             {
               "id": "NCIT_C43412",
               "operator": "=",
               "value": ["Serum","Plasma"]
             },
             {
               "id": "NCIT_C164234",
               "operator": "=",
               "value": "MRI"
             }, 
             {
               "id": "cognitive_data",
               "operator": "=",
               "value": "Cross-sectional"
             },
             {
               "id": "number_of_participants",
               "operator": ">",
               "value": "100"
             },
             {
               "id": "number_of_participants",
               "operator": "<",
               "value": "1000"
             },
             {
               "id": "ISO_3166-1",
               "operator": "=",
               "value": ["AT","BH"]
             }
       ],
      "requestedGranularity": "record"
    }
}
```


Requested granularity for the response. It do not have to respond with the requested granularity, e.g. may respond with count results although record level granularity had been requested but indicate the granularity of the response in the response's metadata. Allowed values -{ boolean,count,aggregated,record} with default boolean value.

The following is an example response 

**EXAMPLE /study RESPONSE**
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
        "numTotalResults": 2
    },
    "response": {
        "resultSets": [
        {
            "exists": false,
            "id": "datasetA",
            "type": "dataset"
        },
        {
            "exists": true,
            "id": "datasetB",
            "results": [
                {
                    "id": "BEex3",
                    "name": "Basic Element example three"
                },
                {
                    "id": "BEex4",
                    "name": "Basic Element example four"
                }
            ],
            "resultsCount": 2,
            "type": "dataset"
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
        "numTotalResults": 2
    },
    "info": {
        "warnings": {
            "unsupportedFilters": [
                
            ]
        }
    },
    "response": {
        "resultSets": [
        {
            "exists": false,
            "id": "datasetA",
            "type": "dataset"
        },
        {
            "exists": true,
            "id": "datasetB",
            "results": [
                {
                    "id": "BEex3",
                    "name": "Basic Element example three"
                },
                {
                    "id": "BEex4",
                    "name": "Basic Element example four"
                }
            ],
            "resultsCount": 2,
            "type": "dataset"
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

/info MUST return information (metadata) about the Beacon service and the organization supporting it

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

/configuration returns configuration aspects and the definition of the entry types (e.g. genomic variants, biosamples, cohorts) implemented in that specific Beacon server or instance.

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

/filtering_terms returns returns a list of the filtering terms accepted by that Beacon instance.

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

In Swagger, to query using both the /individuals endpoint & /study endpoint (which are POST requests), you have to authorize the query using the **Authorize** button (extreme right, beside Servers dropdown in Swagger UI). 

![image](https://user-images.githubusercontent.com/24955128/203320000-a9cbc5a5-4c49-4a2b-8666-4e0cb17a5a62.png)

![image](https://user-images.githubusercontent.com/24955128/205334330-c90f760d-5d1e-4685-8640-030787233454.png)

Use one of the authentication token provided to perform record level queries.
![image](https://user-images.githubusercontent.com/24955128/205334443-fce92738-a515-4f0e-8f9f-b701561c283b.png)

> Developers Note: No matter the user agent being used to query (i.e., SwaggerUI, Postman, cURL, etc.,), the authentication header **auth-key is required**. 

[ ^ Back to the top](#top)

<hr> -->
