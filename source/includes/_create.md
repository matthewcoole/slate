# Creating a Corpus

## Creating a Table

```http
POST /mycorpus/create HTTP/1.1
Host: localhost:1189
Content-Type: application/json

{
  "name": "tokens",
  "sets": [
    {
      "name": "text"
      "type": "ZIPFIAN",
      "indexed": true,
      "columns": [{"name": "token"}]
    }
  ]
}
```

A corpus is created automatically when you create a new table. To create a table simply send a `POST` request to the path you would like your new corpus to have along with a JSON table description to create your first table. 

For example to create a corpus named `mycorpus` just `POST` a JSON table description to the URL [http://localhost:1189/mycorpus/create](http://localhost:1189/mycorpus/create) .

The JSON table description defines both the columns & column sets in the table;

This will create a table named `tokens` containing a single column set and a single column named `token`. The `type` property specifies that the data in the column set is zipfian in nature and the `"indexed": true` ensures an index is built on the column set.

## Inserting Data

```http
POST /mycorpus/tokens/insert HTTP/1.1
Host: localhost:1189
Content-Type: text/csv

token
The
quick
brown
fox
jumped
over
the
lazy
dog
.
```

Once you have created a table inserting data can be done by `POST`ing a `*.tsv` or `*.csv` file to the path of your destination table. Using our earlier example to add some data to our corpus we would post a file to; [http://localhost:1189/mycorpus/tokens](http://localhost:1189/mycorpus/tokens) .

It is important that the contents of the `*.tsv` or `*.csv` contains a header line where the headers match precisely the names of the columns in the table you are trying to insert into (although the ordering of the columns is unimportant).

## Multiple inserts

When inserting data to lexiDB the database will automatically commit everything in the transferred CSV/TSV file and index it. To insert multiple CSV/TSV files you must include the query param `commit=false`. This will prevent lexiDB from indexing so you can insert multiple files into the same table. When inserting the final file simply remove the query param or use `commit=true`

```http
POST /mycorpus/tokens/insert?commit=false HTTP/1.1
Host: localhost:1189
Content-Type: text/csv

token
The
quick
brown
fox
```

```http
POST /mycorpus/tokens/insert?commit=true HTTP/1.1
Host: localhost:1189
Content-Type: text/csv

token
jumped
over
the
lazy
dog
.
```
