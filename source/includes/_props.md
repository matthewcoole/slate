# Hidden Query Properties

## Relative Position

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

## Range Values

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

## Distribute

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
