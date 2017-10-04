# Result Types

There are 4 primary result types supported;

| ID    | Description                                                                                   | Params  |
|-------|-----------------------------------------------------------------------------------------------|---------|
| kwic  | Keyword-in-Context - The results of the query will be returned as a set of concordance lines. | context |
| col   | Collocation - Returns a statistics frame of the terms surrounding the query.                  | context |
| ngram | NGram - Builds a frequency list of the Ngrams that contain the query term.                    | n       |
| list | Frequency List - Generates a frequency list of terms that match the query. | |

All results are automatically paged with details of the page provided in the resulting JSON object.

## KWIC

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "concordances": [
  	[{"token": "but"}, {"token": "the"}, {"token": "apple"}, {"token": "fell"}, {"token": "to"}, ],
  	[{"token": "big"}, {"token": "red"}, {"token": "apple"}, {"token": "."}, {"token": "I"}, ],
  	[{"token": "and"}, {"token": "an"}, {"token": "apple"}, {"token": "sat"}, {"token": "on"}, ],
  	[{"token": "orange"}, {"token": ","}, {"token": "apple"}, {"token": ","}, {"token": "pear"}, ],
  	[{"token": "the"}, {"token": "poison"}, {"token": "apple"}, {"token": "caused"}, {"token": "her"}, ]
  ],
  "page" : 1,
  "pages" : 3,
  "results": 12,
  "resultsPerPage": 5
}
```

KWIC (Keyword-in-Context) queries return their results as a list of concordance lines.
