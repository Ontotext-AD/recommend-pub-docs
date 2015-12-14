---
title: Popular Content Recommendations
layout: default
prev_section: advanced-recommendation-parameters
next_section: stop-recommending-content
category: HowTo's
permalink: v2_0_0-docs/popular-content-recommendations/
---
<div class="info-badge">All example request URLs on this page are relative to the application root. For instance, if the web application is deployed at http://10.0.0.10:8080/api and the example says to request <code>/recommend/popular</code>, the full URL would be http://10.0.0.10:8080/api/recommend/popular.</div>

## Retrieving popular articles

To receive recommendations with popular content do the request:

`
GET /recommend/popular
`

For example the request:

`
GET /recommend/popular?count=2
`

will return the two articles which have the highest number of reads. Below is an example response:

<pre><code>
{
  "version": "Ontotext Recommendations API",
  "type": "RECOMMENDATION",
  "status": "OK",
  "articles": [ {
      "id": "test-document-1",
      "title": "Example article 1 title",
      "score": 0.1532602161169052,
      "published": "2014-03-13T15:43:06Z",
      "url": "http://example.org/test-document-1",
      "popularity": 1204,
    }, {
      "id": "test-document-2",
      "title": "Example article 2 title",
      "score": 0.1324324723490948,
      "published": "2014-03-13T15:43:06Z",
      "url": "http://example.org/test-document-2",
      "popularity": 897,
    }
  ]
}
</code></pre>

The number of reads for each article is kept in the `popularity` field.

### Parameters

`/recommend/popular` has the following query parameters:

- `count` (optional, default = 10) - maximum number of articles to return;
- `sort` (optional, default = rel) - sorting method, valid arguments are "pop" (popularity), "rel" (relevancy) and "date";
- `recency` (optional, default = `<empty>`) - limits the age of the articles returned by the recommendation. The supported values for recency are integers representing the maximum number of days to go back and ISO-formatted date-time representing the point after which the events are processed. For example, `recency=5` will only return articles newer than five days ago. If omitted, no age filter is applied.