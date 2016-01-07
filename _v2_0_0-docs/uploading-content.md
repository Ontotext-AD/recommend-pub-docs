---
title: Uploading Articles
layout: default
prev_section: stop-recommending-content
next_section: user-actions
category: HowTo's
permalink: v2_0_0-docs/uploading-articles/
---

Once annotated, the articles for the recommendation service can be uploaded to the `/content` endpoint. Annotating articles is outside the scope of this document.

## Article schema

The recommendation article schema consists of fields and each field has a name and value. The  fields used are as follows:

* `id` (required) - the article ID, could be an arbitrary string;
* `title` (optional) - the article title;
* `summary` (optional) - a short summary of the article;
* `content` (optional) - the article content;
* `published` (optional) - the article publication date and time. The allowed formats are described [here](http://lucene.apache.org/solr/4_10_1/solr-core/org/apache/solr/schema/DateField.html).
* `url` (optional) - the source URL of the article, if applicable;
* `tags` (optional) - a space-separated list of tags for the article. Anything can be a tag but usually these are the instance IDs of the mentioned entities. For example, an article mentioning twice New York and once London  will have `http://dbpedia.org/resource/New_York_City`,
`http://dbpedia.org/resource/London` and `http://dbpedia.org/resource/New_York_City`
 in its `tags` field. This field is used only for named entities.
* `keyphrases` (optional) - a space-separated list of keyphrases. The format is the same as `tags` - anything can be a keyphrase but generally these are generated URIs such as `http://data.ontotext.com/publishing/topic/Metal` or `http://data.ontotext.com/publishing/topic/Oil_refiner`. This field is used only for general words and phrases that are key for the article.


<div class="note-badge">The recommendations work mainly with <code>tags</code> and <code>keyphrases</code> so it's important that they are present.</div>

The article schema is naturally represented by a simple JSON object, for example:

<pre><code>
{
  "id":"test-document-1",
  "content":"Example Hello London!",
  "title":"Hello London!",
  "published":"2014-10-14T17:00:37Z",
  "url":"http://example.org/hello-london-article",
  "tags":"http://dbpedia.org/resource/London http://dbpedia.org/resource/London",
  "keyphrases": "http://example.org/keyphrases/Example"
  }
</code></pre>


## Uploading articles

If the recommendation engine is deployed at http://recommendation/app, then a couple of actions are available through http://recommendation/app/content:

* `POST /content/<id>` uploads the article with the specified ID in the path. In this case, you can skip the `id` field from the JSON. If both are present, the one from the URL will take precedence. For example:

<pre><code>
POST /content/test-document-1
Content-type: application/json

{
  "content":"Example Hello London!",
  "title":"Hello London!",
  "published":"2014-10-14T17:00:37Z",
  "url":"http://example.org/hello-london-article",
  "tags":"http://dbpedia.org/resource/London http://dbpedia.org/resource/London",
  "keyphrases": "http://example.org/keyphrases/Example"
  }
</code></pre>

* `POST /content` uploads a list of articles. For example:

<pre><code>
POST /content
Content-type: application/json

[
  {
    "id": "test-document-1",
    "content":"Example Hello London!",
    "title":"Hello London!",
    "published":"2014-10-14T17:00:37Z",
    "url":"http://example.org/hello-london-article",
    "tags":"http://dbpedia.org/resource/London http://dbpedia.org/resource/London",
    "keyphrases": "http://example.org/keyphrases/Example"
  },
  {
    "id": "test-document-2",
    "content":"Example Hello New York!",
    "title":"Hello New York!",
    "published":"2014-10-14T17:00:37Z",
    "url":"http://example.org/hello-london-article",
    "tags":"http://dbpedia.org/resource/New_York_City http://dbpedia.org/resource/New_York_City",
    "keyphrases": "http://example.org/keyphrases/Example"
  }
]
</code></pre>

## Retrieving articles

* `GET /content/<id>` retrieves the article with the specified ID in the path. For example, `GET /content/test-document-1` yields:

<pre><code>
{
  "version": "Ontotext Recommendations API",
  "type": "RECOMMENDATION",
  "status": "OK",
  "articles": [
    {
      "id":"test-document-1",
      "content":"Example Hello London!",
      "title":"Hello London!",
      "published":"2014-10-14T17:00:37Z",
      "url":"http://example.org/hello-london-article",
      "tags":"http://dbpedia.org/resource/London http://dbpedia.org/resource/London",
      "keyphrases": "http://example.org/keyphrases/Example"
    }
  ]
}
</code></pre>

* `GET /content?contentid=content_id` - an alternative to the method above, in case the articles have IDs in the form of URIs.

<div class="note-badge">
  <ul>
    <li> The <code>Content-type</code> header for <code>POST</code> requests should be <code>application/json</code>.</li>
    <li> It is recommended (though not required) to set the <code>Accept: application/json</code> header for all requests.</li>
  </ul>
</div>
