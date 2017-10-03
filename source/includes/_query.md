# Querying

## API Usage

```http
GET /mycorpus/query HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "query": {
    "tokens":[
      {"pos": "ADJ"}
    ]
  },
  "result": {"type": "kwic"}
}
```

```shell
curl localhost:1189/mycorpus/query
  -X GET
  -H "Content-Type: application/json"
  -d "$JSON"

{
  "query": {
    "tokens":[
      {"pos": "ADJ"}
    ]
  },
  "result": {"type": "kwic"}
}
```

A `GET` request can be made to the endpoint [http://localhost:1189/sample/query](http://localhost:1189/api/query). The body of the request should be in the form of a JSON query.

This will send a `query` request to the `sample` corpus.

### Command Line

Requests can be made from the command line using `curl`.
 
_Review [cURL documentation](https://curl.haxx.se/docs/) for more info._

### Result Types

There are 4 primary result types supported;

| ID    | Description                                                                                   | Params  |
|-------|-----------------------------------------------------------------------------------------------|---------|
| kwic  | Keyword-in-Context - The results of the query will be returned as a set of concordance lines. | context |
| col   | Collocation - Returns a statistics frame of the terms surrounding the query.                  | context |
| ngram | NGram - Builds a frequency list of the Ngrams that contain the query term.                    | n       |
| list | Frequency List - Generates a frequency list of terms that match the query. | |

## Simple Queries

### Basics

```http
GET /mycorpus/query HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "query": {
    "tokens":[
      {"pos": "ADJ"}
    ]
  },
  "result": {"type": "kwic"}
}
```

```shell
curl localhost:1189/mycorpus/query
  -X GET
  -H "Content-Type: application/json"
  -d "$JSON"

{
  "query": {
    "tokens":[
      {"pos": "ADJ"}
    ]
  },
  "result": {"type": "kwic"}
}
```

To query use the QBE (Query By Example) JSON syntax. This will query the `"tokens"` table and the `"pos"` (part-of-speech) column for the value `"ADJ"` and return the results in the form of a `"kwic"` (keyword in context).

### Phrases

```http
GET /mycorpus/query HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "query": {
    "tokens":[
      {"token": "out"},
      {"token": "of"},
      {"token": "time"},
    ]
  },
  "result": {"type": "kwic"}
}
```

```shell
curl localhost:1189/mycorpus/query
  -X GET
  -H "Content-Type: application/json"
  -d "$JSON"

{
  "query": {
    "tokens":[
      {"token": "out"},
      {"token": "of"},
      {"token": "time"},
    ]
  },
  "result": {"type": "kwic"}
}
```

A sequence of tokens or phrase can be searched for by specifying additional objects in the query array example for a given table e.g. to query for the phrase "out of time" as it appears in its original form.

### Regular Expressions

```http
GET /mycorpus/query HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "query": {
    "tokens":[
      {"token": "star.*"}
    ]
  },
  "result": {"type": "kwic"}
}
```

```shell
curl localhost:1189/mycorpus/query
  -X GET
  -H "Content-Type: application/json"
  -d "$JSON"

{
  "query": {
    "tokens":[
      {"token": "star.*"}
    ]
  },
  "result": {"type": "kwic"}
}
```

LexiDB incorporates [dk.brics.automaton](http://www.brics.dk/automaton/) for resolving regular expressions.

Values within queries can be expressed as regular expressions (using the [automaton syntax](http://www.brics.dk/automaton/doc/index.html?dk/brics/automaton/RegExp.html));

This example query would match against any of `star`, `staring`, `starred`, `start`, `starting` etc.

## Query Defaults

### Relative Position

```http
GET /mycorpus/query HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "query": {
    "tokens":[
      {"token": "out", "rp": "0"},
      {"token": "of", "rp": "1"},
      {"token": "time", "rp": "2"},
    ]
  },
  "result": {"type": "kwic"}
}
```

```shell
curl localhost:1189/mycorpus/query
  -X GET
  -H "Content-Type: application/json"
  -d "$JSON"

{
  "query": {
    "tokens":[
      {"token": "out", "rp": "0"},
      {"token": "of", "rp": "1"},
      {"token": "time", "rp": "2"},
    ]
  },
  "result": {"type": "kwic"}
}
```

Many objects in the query syntax have properties that default to particular values if omitted from the query. For example, the QBE array specified for a table contains the hidden property; relative position `"rp"` which defaults to a sequential increment within the array i.e. the previous query would have the relative position properties default to;

```http
GET /mycorpus/query HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "query": {
    "tokens":[
      {"token": "white", "rp": "0"},
      {"token": "rabbit", "rp": "-2 to 2"}
    ]
  },
  "result": {"type": "kwic"}
}
```

```shell
curl localhost:1189/mycorpus/query
  -X GET
  -H "Content-Type: application/json"
  -d "$JSON"

{
  "query": {
    "tokens":[
      {"token": "white", "rp": "0"},
      {"token": "rabbit", "rp": "-2 to 2"}
    ]
  },
  "result": {"type": "kwic"}
}
```

Some properties such as relative position can be specified as individual values, a list of values or in some cases a range;

This query would search for the string "white" in its original form in the tokens table and the string "rabbit" in its original form up to 2 positions before or after. 

The same query could be expressed less elegantly using a list of values; `{"token": "rabbit", "rp": "-2, -1, 0, 1, 2"}`. Obviously here the position 0 is superfluous as it would be in the same position as the previous token but this kind of thing can still be done if you were searching on another column.

### Distribute

```http
GET /mycorpus/query HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "query": {
    "tokens":[
      {"token": "apple.*"}
    ]
  },
  "result": {"type": "kwic"},
  "dist": false
}
```

```shell
curl localhost:1189/mycorpus/query
  -X GET
  -H "Content-Type: application/json"
  -d "$JSON"

{
  "query": {
    "tokens":[
      {"token": "apple.*"}
    ]
  },
  "result": {"type": "kwic"},
  "dist": false
}
```

When lexiDB is deployed in a cluster queries will automatically be distributed to all nodes in the cluster and the results aggregated and returned by the original endpoint the query was sent to. This behavior is caused by the default hidden query param; distribute `dist`. This param will default to true. If you would like to see the results returned by just a single node simply send a query to that nodes endpoint setting the `dist` param to `false`;
