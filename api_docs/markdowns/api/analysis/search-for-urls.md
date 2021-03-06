# Search for URLs

Botify API allows you to **filter the dataset of analyzed URLs** and return any available information. Full list of requestable fields can be found in [[URLs Datamodel;analysis-urls-datamodel]].


## Endpoint

- Operation: [[getUrls;reference#/Analysis/getUrls]]
- Path: `analyses/{username}/{project_slug}/{analysis_slug}/urls`
- HTTP Verb: POST
- Body : `Array<BQLQuery>`
- Response: `Pagination<BQLResult>`

Please refer to [[BQLQuery;bql-query]] documentation for information about input.

```SH
curl 'https://api.botify.com/v1/analyses/${username}/${project_slug}/${analysis_slug}/urls' \
     -X POST \
     -H 'Authorization: Token ${API_KEY}' \
     -H 'Content-type: application/json' \
     --data-binary '${BQLQuery}'
```

## Example

The following example of [[BQLQuery;bql-query]] fetches `url` and `metadata.title.nb` fields and filters the dataset on new URLs that respond with a 2xx HTTP code. The result is sorted on descending number of titles.

```JSON
{
  "fields": [
    "url",
    "metadata.title.nb"
  ],
  "filters": {
    "and": [
      {
        "field": "http_code",
        "predicate": "between",
        "value": [200, 300]
      },
      {
        "not": {
          "field": "previous",
          "predicate": "exists"
        }
      }
    ]
  },
  "sort": [
    {
      "metadata.title.nb": {
        "order": "desc"
      }
    }
  ]
}
```

A sample result would be:
```JSON
{
  "count": 11,
  "page": 1,
  "size": 10,
  "results": [
    {
      "url": "www.abc.com/1",
      "metadata": {
        "title": {
          "nb": 15
        }
      }
    },
    {
      "url": "www.abc.com/2",
      "metadata": {
        "title": {
          "nb": 9
        }
      }
    }
  ]
}
```
