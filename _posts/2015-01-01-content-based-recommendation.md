---
Title: Content-based Recommendation
layout: docs
prev_section: installation
next_section: get-behavioural-recommendations
category: HowTo's
permalink: recommendations-api/content-based-recommendation
---

:exclamation: **Note:** All request URLs in the document are relative to the application root. E.g. if the web application is deployed at http://10.0.0.10:8080/api and this document says to request `/recommend/contextual`, the full URL would be http://10.0.0.10:8080/api/recommend/contextual.

To receive content-based recommendations for existing article do a request:

`
GET /recommend/contextual?contentid=<existing_id>
`

which will return the following (example) response

```
{
  "version": "Ontotext Recommendations API",
  "type": "RECOMMENDATION",
  "status": "OK",
  "articles": [
  {
    "id": "test-document-1",
    "title": "Example article 1 title",
    "score": 0.1532602161169052,
    "published": "2014-03-13T15:43:06Z",
    "url": "http://example.org/test-document-1",
    "popularity": 12
    }, {
      "id": "test-document-1",
      "title": "Example article 2 title",
      "score": 0.1324324723490948,
      "published": "2014-03-13T15:43:06Z",
      "url": "http://example.org/test-document-2",
      "popularity": 8
    }
    // possibly more documents
    ]
  }
```

## Parameters

`/recommend/contextual` has the following query parameters:

- *contentid* (required) - existing article identifier
- *count* (optional, default = 10) - maximum number of articles to return
- *sort* (optional, default = rel) - sorting method, valid arguments are "pop"(popularity), "rel"(relevancy) and "date"
- *recency* (optional, default = <empty>) - limit the age of articles returned by the recommendation. Supported values for recency are "amonthago", "aweekago", and integers representing the maximum number of days to go back. For example recency=5 will only return articles newer than five days ago. If omitted, no age filter is applied.
