# kraken_schema
Schema definition for kraken objects


## Observation
### Definition
An observation is a specific data point from a given source
For example, someone might go by several first names: Richard, Dick, Dicky, etc. There also might be some typos: Ricard, Richar, etc. 
Observations allows to compile there various data points and determine which one is the best based on criterias such a s source, latest date, credibility, etc. 

### Use cases

#### Compilation of information from different sources
Similar observations can be gathered from different sources and the best information can be selected based on date, credibility, source system, etc.

#### Compilation of information through time
Observations can be gathered throughout time to see the evolution of the value. For example, a transaciton going through the different parts of its lifecycle (new, open, closed, canceled, etc).

#### Traceability of information
Observations also store the other data points that lead to their generation. This if a source data point turns out to be false, all child data point (the data points that were infered form the source) can be traced back and corrected. 

For example, a data point for email derived another data point for domain. If the dat aopoint for email turns out to be wrong, the domain data point can be traced back and removed. 


### Schema

#### Version 1.0
|Key | Type | Definition |
| :--- | :--- | :--- |
| @type | str           | krkn:observation |
| @id | str             | id of the datapoint (uuid) |
| @version | str        | the version of the schema |
| key | str             | the key the datapoint represents (schema:name, schema:url, etc) |
| value | ANY           | the actual value of the data point (string, integer, float, dict, list, object, etc) |
| date | datetime       | the date the data point was created, either in the original source or default to now |
| start | datetime      | date the value starts to be effective |
| finish | datetime     | date the value is no longer effective |
| credibility | float   | the credibility of the data point from 0 to 1, 1 being highest |
| object | dict         | he object at the source of the data point. Can be system, person, etc. |
| instrument | dict     | the tools that generated the data point from the object |
| sources | list of str | The ids of the observations that generated the data point. |


### Examples

#### Name data point entered by a user
```
{
  "@type": "krkn:observation",
  "@id": "ddffkoo33",
  "@version": "1.0",
  "krkn:key":   "schema:givenName",
  "krkn:value":   "Richard",
  "krkn:date": "2021-07-27#16:01:12.090202",
  "krkn:credibility": 0.4,
  "krkn:object": {
              "@type": "schema:person",
              "@id": "223344",
              "schema:givenName": "Richard",
              "schema:familyName": "Blanc"
            },
  "krkn:instrument": "",
  "krkn:sources": []
}

```
  
