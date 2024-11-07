# REST API specification for querying EPND resources

> This API specification is defined in the context of the EPND project, complying with the latest [Beacon v2 Specification](https://github.com/ga4gh-beacon/beacon-v2).


<hr>

<h2 id="top"> Contents </h2>

* [Try out this API in Swagger](#-try-out-the-api-)
* [Specification](#-specification-)
* [Query Endpoints](#-query-endpoints-)
    * [Study endpoint](#-s-endpoint-)
      * [List of filters](#-list-of-filters-and-permitted-values-for-the-study-endpoint-)
      * [Filters description](#-study-filters-description-)
      * [Example request & response](#-example-request-and-response-for-study-)
* [Understanding the query](#-understanding-the-query-)
    * [Syntax & Usage of Beacon Query with Filters](#-syntax-and-usage-)
        * [Multi-Filter (AND) query](#beacon-queries-using-multiples-of-the-same-type-of-filter-and-logical-operator-between-filters)
        * [Same Filter (AND) query](#beacon-queries-using-multiple-values-as-in-phenotype-or-disease-filters-and-logical-operator-between-filter-values)
        * [Multi-Filter (OR) query using **Arrays**](#beacon-queries-using-multiples-of-the-same-type-of-filter-or-logical-operator-between-filters)
    <!-- * [Partial query matches with warning messages](#partial-query-matches-with-warning-messages) -->
<!-- * [Understanding the response with ranges (for /individuals and /biospecimens)](#-understanding-the-response-with-ranges-for-individuals-and-biospecimens) -->
* [Informational Endpoints](#-informational-endpoints-)


<hr>
 
<h2 id="#-try-out-the-api-"> Try out the API </h2>

Latest version (v2.0) of this specification is available on Swagger here: https://app.swaggerhub.com/apis/deepthivs/EPND/v1.0

<hr>

<h2 id="-specification-"> Specification </h2>

The request and response conforms to the [Beacon Reference Framework](https://github.com/ga4gh-beacon/beacon-v2). This Specification defines two types of endpoints - **[The Query Endpoints](#-query-endpoints-)** and **[The Informational Endpoints](#-informational-endpoints-)**. 

[Query Endpoints](#-query-endpoints-) require the requester to provide a JSON body and send request using the POST method. This document defines query endpoints to query resources using filters -  [/study](#-study-endpoint-).

[Informational Endpoints](#-informational-endpoints-) are simple GET requests without needing a request body, and respond with information relavant to this Beacon Specification. These are: /info, /configuration, /entry_types, /filtering_terms and /map. A special /service-info endpoint (also a GET request), responds with metadata relevant to this Beacon using the [GA4GH ServiceInfo format](https://github.com/ga4gh-discovery/ga4gh-service-info/). 


<hr>

<h2 id="-query-endpoints-"> Query Endpoints </h2>

This specification defines POST endpoints to request information about resources. Each endpoint makes use of the [Filters](http://docs.genomebeacons.org/filters/) capability of the Beacon API.

<hr>
<h3 id="-study-endpoint-"> Datasets endpoint </h3>

> Method: POST

[/datasets](#-study-endpoint-) endpoint returns the **datasets of EPND resources** as response. Filters are provided as a part of the body while using a POST request to query resources.

<h4 id="-list-of-filters-and-permitted-values-for-the-study-endpoint-"> List of filters and permitted values for the datasets endpoint </h4>

> **Note**: Elements within arrays in **value** fields are treated as **ORs** and always use logical **AND** between the query parameters ,i.e all conditions in the query have to be met.


<table>
    <thead>
        <tr>
            <th>Metadata Schema Concept</th>
            <th>Beacon Filter Type</th>
            <th>ID</th>
            <th>Operator</th>
            <th>Query Values</th>
        </tr>
    </thead>
    <tbody>
        <td rowspan="4"><b>Sex</b>
        </td>
        <td rowspan="4">Alphanumerical</td>
        <td rowspan="4">NCIT:C28421 </td>
        <td rowspan="4">=</td>
        <td>
        Unknown
        </td>
    </tr>
    <tr><td>Female</td></tr>
    <tr><td>Male</td></tr>
    <tr><td>Other</td></tr>
    <tr>
    <tr>
     <tr>
      <td><b>Number of Subjects</b></td>
        <td>Custom</td>
        <td>nos:number_of_subjects</td>
        <td>>,<,=,>=,<=</td>
        <td>any integer</td>
    </tr>
    <tr>
        <td rowspan="15"><b>Dataset Types</b>
        </td>
        <td rowspan="15">Alphanumerical</td>
        <td rowspan="15">NCIT:C47824</td>
        <td rowspan="15">=</td>
        <td>
        NCIT:C142447(Clinical Information)
        </td>
    </tr>
    <tr><td>NCIT:C19591(Diagnostic marker)</td></tr>
    <tr><td>NCIT:C17369(Imaging)</td></tr>
    <tr><td>NCIT:C16540(Electrophysiology)</td></tr>
    <tr><td>NCIT:C16495(Demographics)</td></tr>
    <tr><td>NCIT:C20641(Functional Ratings)</td></tr>
    <tr><td>NCIT:C28367(Motor)</td></tr>
    <tr><td>NCIT:C100772(Neuropsychiatric)</td></tr>
    <tr><td>NCIT:C165543(Neuropsychological Assessment)</td></tr>
    <tr><td>NCIT:C17047(Quality of Life)</td></tr>
    <tr><td>NCIT:C121705(Sleepscales)</td></tr>
    <tr><td>NCIT:C114457(Digital Data)</td></tr>
    <tr><td>NCIT:C18479(Neuropathology)</td></tr>
    <tr><td>NCIT:C16564(Ethnicity Coverage)</td></tr>
    <tr><td>Other</td></tr>
  

</tbody>
</table>


[ ^ Back to the top](#top)
<hr>



<h3 id="-study-filters-description-"> Dataset endpoint Filters Description </h3>

**Sex**: A single value or an array of biological sex of an individual, eg : Male or [Male,Female]

**Number of Subjects**: The number of subjects in the dataset . eg : >10 ,<20 or =100

**Dataset Types**: A single value or an array of dataset types , eg :NCIT:C19591 or [NCIT:C19591,NCIT:C17369].



[ ^ Back to the top](#top)

<hr>

<h3 id="-example-request-and-response-for-study-"> Example request and response for datasets </h3>

**EXAMPLE /datasets REQUEST**

```JSON
{ 
 "meta":{
      "apiVersion": "v2.0.1"
 },
 "query": {
      
"filters": [
             {
               "id": "NCIT:C28421",
               "operator": "=",
               "value": ["Female","Male"]
             },
             {
               "id": "nos:number_of_subjects",
               "operator": "<",
               "value": 1000
             },
             {
               "id": "NCIT:C47824",
               "operator": "=",
               "value": "NCIT:C19591"
             }
       ],
      "requestedGranularity": "record"
    }
}
```


**requestedGranularity**: It is not mandatory to respond with the exact requested granularity, but the response granularity must specify the actual granularity level of the response in the response metadata. Allowed granularity values include "boolean", "count", "aggregated" and "record", with "record" being the default value.

> **Note**:If the Beacon service supports record-level granularity but the client requests count-level granularity, the service should aggregate the matching records and provide the count of those records as the response.

The following is an example response 

**EXAMPLE /datasets RESPONSE**
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
             {
               "id": "NCIT:C28421",
               "operator": "=",
               "value": ["Female","Male"]
             },
             {
               "id": "nos:number_of_subjects",
               "operator": "<",
               "value": 1000
             },
             {
               "id": "NCIT:C47824",
               "operator": "=",
               "value": "NCIT:C19591"
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
               "entityType": "datasets",
               "schema": "ga4gh-beacon-dataset-v2.0.0",
            }
        ]
    },
    "responseSummary": {
        "exists": true,
        "numTotalResults": 2
    },
   "response": {
        "resultSets": [
        {
            "exists": true,
            "resultsCount": 2,
            "results": [
                {
                    "id": "BEex3",
                    "name": "Basic Element example three",
                    "info":{
                    "type": "dataset",
                    "resourceURLs": ["https://www.example.com"],
                    "description": "string",
                    "releaseLicense": "license URL",
                    "language": "language code",
                    "publisher": {
                      "publisherType": "",
                      "name": "epnd.org",
                      "contactEmail": "epnd@gmail.com",
                      "contactName": "epnd",
                      "url": "string",
                      "location": "Italy"
                        },
                      "datasetVersions": [
                          {
                            "versionName": "v1.0.0",
                            "numberOfSubjects": 100
                          }]
                    }
                },
                {
                    "id": "BEex4",
                    "name": "Basic Element example four",
                    "info":{
                    "type": "dataset",
                    "resourceURLs": ["https://www.example.com"],
                    "description": "string",
                    "releaseLicense": "license URL",
                    "language": "language code",
                    "publisher": {
                      "publisherType": "",
                      "name": "epnd.org",
                      "contactEmail": "epnd@gmail.com",
                      "contactName": "epnd",
                      "url": "string",
                      "location": "Italy"
                        },
                      "datasetVersions": [
                          {
                            "versionName": "v1.0.0",
                            "numberOfSubjects": 100
                          }]
                    }
                }
            ],
            
            
        }
    ]
    }
}
```

[ ^ Back to the top](#top)

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
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="13"><b>Available Diseases</b></td>
            <td rowspan="13">Alphanumerical</td>
            <td rowspan="13">NCIT:C2991</td>
            <td rowspan="13">=</td>
            <td>CG</td>    
        </tr>
        <tr><td>AD</td></tr>
        <tr><td>PD</td></tr>
        <tr><td>DLB</td></tr>
        <tr><td>iRBD</td></tr>
        <tr><td>CAA</td></tr>
        <tr><td>FTD</td></tr>
        <tr><td>ALS</td></tr>
        <tr><td>PSP</td></tr>
        <tr><td>CBD</td></tr>
        <tr><td>MSA</td></tr>
        <tr><td>HD</td></tr>
        <tr><td>AT</td></tr>
    <tr>
        <td rowspan="7"><b>Available Samples</b>
        </td>
        <td rowspan="7">Alphanumerical</td>
        <td rowspan="7">NCIT:C43412</td>
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
        <td rowspan="4">NCIT:C17369</td>
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
            CD:cognitive_data
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
        <td>nos:number_of_subjects</td>
        <td>>,<,=,>=,<=</td>
        <td>any integer</td>
    </tr>
    <tr>
      <td><b>Country</b></td>
        <td>Alphanumerical</td>
        <td>ISO:3166-1</td>
        <td>=</td>
        <td>https://www.iso.org/obp/ui/#search</td>
    </tr>
  

</tbody>
</table>


[ ^ Back to the top](#top)
<hr>



<h3 id="-study-filters-description-"> Study endpoint Filters Description </h3>

**Available Diseases**: A single value or an array of diseases, eg : AD or [CG,AD]

**Available Samples**: A single value or an array of samples, eg : Serum or [Serum,Plasma]

**Available Imaging**: A single value or an array of imaging technique, eg : MRI or [MRI,PET-Amyloid]

**Cohort Design**: A single value or an array of cohort design, eg : Cross-sectional or [Cross-sectional,Longitudinal]

**Number of Subjects**: The number of subjects in the cohort . eg : >10 ,<20 or =100

**Country**: A single value or an array of two-letter country codes eg : AF OR [AF,AT]

[ ^ Back to the top](#top)

<hr>


<table>
    <thead>
        <tr>
            <th>Abbreviation</th>
            <th>Definiton</th>
        </tr>
    </thead>
    <tbody>
        <tr>
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
    
</tbody>
</table>

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
               "id": "NCIT:C2991",
               "operator": "=",
               "value": ["CG","AD"]
             },
             {
               "id": "NCIT:C43412",
               "operator": "=",
               "value": ["Serum","Plasma"]
             },
             {
               "id": "NCIT:C164234",
               "operator": "=",
               "value": "MRI"
             }, 
             {
               "id": "CD:cognitive_data",
               "operator": "=",
               "value": "Cross-sectional"
             },
             {
               "id": "nos:number_of_subjects",
               "operator": ">",
               "value": 100
             },
             {
               "id": "nos:number_of_subjects",
               "operator": "<",
               "value": 1000
             },
             {
               "id": "ISO:3166-1",
               "operator": "=",
               "value": ["AT","BH"]
             }
       ],
      "requestedGranularity": "record"
    }
}
```


**requestedGranularity**: It is not mandatory to respond with the exact requested granularity, but the response granularity must specify the actual granularity level of the response in the response metadata. Allowed granularity values include "boolean", "count", "aggregated" and "record", with "record" being the default value.

> **Note**:If the Beacon service supports record-level granularity but the client requests count-level granularity, the service should aggregate the matching records and provide the count of those records as the response.

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
             {
               "id": "NCIT:C2991",
               "operator": "=",
               "value": ["CG","AD"]
             },
             {
               "id": "NCIT:C43412",
               "operator": "=",
               "value": ["Serum","Plasma"]
             },
             {
               "id": "NCIT:C164234",
               "operator": "=",
               "value": "MRI"
             }, 
             {
               "id": "CD:cognitive_data",
               "operator": "=",
               "value": "Cross-sectional"
             },
             {
               "id": "nos:number_of_subjects",
               "operator": ">",
               "value": 100
             },
             {
               "id": "nos:number_of_subjects",
               "operator": "<",
               "value": 1000
             },
             {
               "id": "ISO:3166-1",
               "operator": "=",
               "value": ["AT","BH"]
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
               "entityType": "study",
               "schema": "beacon-cohort-v2.0.0",
            }
        ]
    },
    "responseSummary": {
        "exists": true,
        "numTotalResults": 2
    },
    "response": {
        "resultSets": [
        {
            "exists": true,
            "resultsCount": 2,
            "results": [
                {
                    "id": "BEex3",
                    "name": "Basic Element example three",
                    "info":{
                    "type": "dataset",
                    "url": "string",
                    "description": "string",
                    "releaseLicense": "license URL",
                    "language": "language code",
                    "publisher": {
                      "publisherType": "",
                      "name": "epnd.org",
                      "contactEmail": "epnd@gmail.com",
                      "contactName": "epnd",
                      "url": "string",
                      "location": "Italy"
                        }
                    }
                },
                {
                    "id": "BEex4",
                    "name": "Basic Element example four",
                    "info":{
                    "type": "dataset",
                    "url": "string",
                    "description": "string",
                    "releaseLicense": "license URL",
                    "language": "language code",
                    "publisher": {
                      "publisherType": "",
                      "name": "epnd.org",
                      "contactEmail": "epnd@gmail.com",
                      "contactName": "epnd",
                      "url": "string",
                      "location": "Italy"
                        }
                    }
                }
            ],
            
            
        }
    ]
    }
}
```

[ ^ Back to the top](#top)

<hr>


The filter **SHOULD** be one of the terms from the [filters and permitted values table](#-list-of-filters-and-permitted-values-for-the-study-endpoint-). Please note that not all resources will support all of the filters. In such cases the response **MUST** include a [warning message in the 'info' part](#warning-response-example) indicating which requested filters are unsupported and these were not included in the query.

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
    "info": {
        "warnings": {
            "unsupportedFilters": [
                
            ]
        }
    },
    "response": {
        "resultSets": [
        {
            "exists": true,
            "resultsCount": 1,
            "results": [
                {
                    "id": "BEex3",
                    "name": "Basic Element example three",
                    "info":{
                    "type": "dataset",
                    "url": "string",
                    "description": "string",
                    "releaseLicense": "license URL",
                    "language": "language code",
                    "publisher": {
                      "publisherType": "",
                      "name": "epnd.org",
                      "contactEmail": "epnd@gmail.com",
                      "contactName": "epnd",
                      "url": "string",
                      "location": "Italy"
                        }
                    }
                }
            ],
            
            
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

<hr>

<h2 id="-understanding-the-query-"> Understanding the query </h2>

<h3 id="-syntax-and-usage-"> Syntax and Usage: </h3>

> Method: POST

There are 2 types of Beacon queries that this specification currently supports: 
1. [Alphanumerical Query](http://docs.genomebeacons.org/filters/)
2. [Custom Query](http://docs.genomebeacons.org/filters/)


The following JSON body depicts the typical usage of filters to query resources. 
   * Alphanumeric filters **require** the use of id, operator(=) and value properties. 
   * Custom filters are intended for non-bio-ontology terms and **require** the use of id, operator and value properties and it is also possible to use comparison operators (>=,>,<=,<) along with '='. 

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
        "id": "CustomFilter_id",
        "operator": "=",
        "value":"CustomFilter_value"
      }
    ],
     "requestedGranularity": "record"
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
               "id": "NCIT:C164234",
               "operator": "=",
               "value": "MRI"
             }, 
             {
               "id": "cognitive_data",
               "operator": "=",
               "value": "Cross-sectional"
             },
      ]
    }
}
```

This filter is asking for cohorts that have Imaging as MRI  **and** cognitive_data as Cross-sectional .


[ ^ Back to the top](#top)

<hr>

<h4 id="beacon-queries-using-multiple-values-as-in-phenotype-or-disease-filters-and-logical-operator-between-filter-values">Beacon queries using multiples of the same type of filter (AND logical operator between filters):</h4>

To query for records with more than one instance of any of the filters you can send multiple of the same filter, such as in the below example:

> Alphanumeric Filter example:

```JSON
{
"query": 
    {
      "filters": [
        {
               "id": "NCIT:C43412",
               "operator": "=",
               "value": "Serum"
             }, 
             {
               "id": "NCIT:C43412",
               "operator": "=",
               "value": "Saliva"
             },
      ]
   }
}
```
This query is looking for cohorts with biosample Serum AND Saliva biosample.


[ ^ Back to the top](#top)

<hr>

<h4 id="beacon-queries-using-multiples-of-the-same-type-of-filter-or-logical-operator-between-filters">Beacon queries using multiples of the same type of filter (OR logical operator between filters):</h4>

> Alaphanumeric Filter using Array Example:

```JSON
{
"query": 
    {
      "filters": [
         {
               "id": "NCIT:C2991",
               "operator": "=",
               "value": ["CG","AD"]
             },
      ]
   }
}
```
This query is looking for cohorts either with Cognitively normal disease OR Alzheimer's disease.

> **Note**: There are no OR operators available **between** filters with beacon queries.

All of the defined filters are optional, the user can provide as many or as few as wanted and the resource does not have to implement all filters.And if the user does not provide any filters all the list of entities will be returned.

If a user sends a query with a filter not supported by a resource, then the resource should complete the query but ignore the unsupported filter(s) and respond as usual, but with a warning noting that certain filters were ignored as they are unsupported.

The warning messages will be provided within the **info**  section of the Beacon.

[ ^ Back to the top](#top)

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
