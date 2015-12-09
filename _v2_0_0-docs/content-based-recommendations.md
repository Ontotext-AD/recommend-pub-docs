---
layout: default
title: Content-based Recommendations
next_section: behavioural-recommendations
category: HowTo's
permalink: v2_0_0-docs/content-based-recommendations/
---

<div class="info-badge">All example request URLs on this page are relative to the application root. For instance, if the web application is deployed at http://10.0.0.10:8080/api and the example says to request <code>/recommend/contextual</code>, the full URL would be http://10.0.0.10:8080/api/recommend/contextual.</div>

## Retrieving articles based on content similatity

To receive content-based recommendations for existing article, do the request:

`
GET /recommend/contextual?contentid=existing_id
`

and it will return the following (example) response:

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
      "popularity": 12,
    }, {
      "id": "test-document-2",
      "title": "Example article 2 title",
      "score": 0.1324324723490948,
      "published": "2014-03-13T15:43:06Z",
      "url": "http://example.org/test-document-2",
      "popularity": 8,
    }
    // possibly more documents
  ]
}
</code></pre>

### Parameters

`/recommend/contextual` has the following query parameters:

- `contentid` (required) - existing article identifier;
- `count` (optional, default = 10) - maximum number of articles to return;
- `sort` (optional, default = rel) - sorting method, valid arguments are "pop"(popularity), "rel"(relevancy) and "date";
- `explain` (default = false) - If set to true, the response contains additional debug information;
- `onlyunread` (default = true) - If set to true, the recommendation contains only articles the user has never read. Otherwise read articles are also included in the response. 
- `recency` (optional, default = <empty>) - limits the age of the articles returned by the recommendation. The supported values for recency are integers representing the maximum number of days to do back and ISO-formatted date-time representing the point after which the events are processed. For example, recency=5 will only return articles newer than five days ago. If omitted, no age filter is applied;
- `filter` (optional, default = <empty>) - a string containing space delimeted key-value pairs which are interpreted as an "AND" constraint. For example:
1. title:"Some title" - retrieves articles having a title that matches the requirement
2. title:"Some title" summary:"Some summary" - retrieves all articles with matching title and summary

## Retrieving articles based on content similarity with advanced parameters

There are some advanced parameters that could be passed for fine-tuning the results. For this aim you can use the POST method with the parameters that you want to set in the body section:

<pre><code>
POST /recommend/contextual?contentid=existing_id
Content-type: application/json
{
  "beh.weight.popularity": 0.2,
  "beh.weight.freshness" : 0.8
}
</code></pre>

This is described in more detail in the [Advanced recommendation parameter](/recommend-pub-docs/v2_0_0-docs/advanced-recommendation-parameters/) section

## Retrieving articles based on content similarity to a non-indexed article

This can be achieved using the following request with a JSON object:

<pre><code>
POST /recommend/contextual/doc
Content-type: application/json
{
  "doc":
  {
    "title": "This is the title",
    "content": "Goldman Sachs",
    "tags": "http://dbpedia.org/resource/Goldman_Sachs"
  }
}
</code></pre>

Optionally you can include values for the advanced parameters:

<pre><code>
POST /recommend/contextual/doc
Content-type: application/json
{
  "doc":
  {
    "title": "This is the title",
    "content": "Goldman Sachs",
    "tags": "http://dbpedia.org/resource/Goldman_Sachs"
  },
  "beh.weight.freshness": 0.8
}
</code></pre>

