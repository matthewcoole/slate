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

## Command Line

Requests can be made from the command line using `curl`.
 
_Review [cURL documentation](https://curl.haxx.se/docs/) for more info._

## Simple Queries

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

## Phrases

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

## Regular Expressions

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
