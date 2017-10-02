---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Creating a Table

A corpus is created automatically when you create a new table. To create a table simply send a `POST` request to the path you would like your new corpus to have along with a JSON table description to create your first table. 

For example to create a corpus named `mycorpus` just `POST` a JSON table description to the URL [http://localhost:1189/mycorpus/create](http://localhost:1189/mycorpus/create) .

The JSON table description defines both the columns & column sets in the table;

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

```shell
curl -H "Content-Type: application/json" -X POST localhost:1189/mycorpus/create -d "$JSON"
```

This will create a table named `tokens` containing a single column set and a single column named `token`. The `type` property specifies that the data in the column set is zipfian in nature and the `"indexed": true` ensures an index is built on the column set.

## Command Line
With the above config saved to the file `config.json` you can use `curl` to `POST` the request.

```

```
`response.json` will contain details as to if the corpus and table were created successfully.

# Inserting Data

Once you have created a table inserting data can be done by `POST`ing a `*.tsv` or `*.csv` file to the path of your destination table. Using our earlier example to add some data to our corpus we would post a file to; [http://localhost:1189/mycorpus/tokens](http://localhost:1189/mycorpus/tokens) .

```tsv
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

## Command Line
To `POST` the file `text.tsv` to the table `mycorpus/tokens` you can use `curl`;
```
curl -X POST --data-binary @text.tsv localhost:1189/mycorpus/insert > response.json
```
The response will be saved to `response.json` which will tell you if the operation was successful.

It is important that the contents of the `*.tsv` or `*.csv` contains a header line where the headers match precisely the names of the columns in the table you are trying to insert into (although the ordering of the columns is unimportant).

