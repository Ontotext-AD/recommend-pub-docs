---
Title: Uploading Articles
layout: docs
prev_section: stop-recommending-content
next_section: user-actions
category: HowTo's
permalink: recommendations-api/uploading-articles
---

Once annotated, articles for the recommendation service can be uploaded to the `/content` endpoint or added directly to Solr. Annotating articles is outside the scope of this document.

## Article schema

Recommendation articles consist of fields, each field having a name and value. Fields used by recommendation are:
* *id* (required) - the id of the article, could be arbitrary string.
* *title* (optional) - the title of the article.
* *summary* (optional) - a short summary of the article.
* *content* (optional) - the text content of the article.
* *published* (optional) - the date and time of publishing this article. Allowed formats are described [here](http://lucene.apache.org/solr/4_10_1/solr-core/org/apache/solr/schema/DateField.html).
* *url* (optional) - the source URL of the article, if applicable
* *tags* (optional) - a space-separated list of tags for this article. Anything can be a tag, but we usually use instance ids of mentioned entities. For example, an article mentioning New York, London and New York again will have `http://dbpedia.org/resource/New_York_City http://dbpedia.org/resource/London http://dbpedia.org/resource/New_York_City` in its *tags* field. This field should be used for named entities only - general "important" words and phrases from the text should go in the *keyphrases* field.
* *keyphrases* (optional) - a space-separated list of keyphrases. The format is the same as *tags* - anything can be a keyphrase, but we generally use generated URIs like `http://data.ontotext.com/publishing/topic/Metal` or `http://data.ontotext.com/publishing/topic/Oil_refiner`. Again, named entities should go in *tags* and general important words and phrases in this field.

:exclamation: **Note:** Recommendations mostly work with *tags* and *keyphrases* so having those is important.

Recommendation articles are naturally represented by a simple JSON object, for example:

```
{
  "id":"test-document-1",
  "content":"Example Hello London!",
  "title":"Hello London!",
  "published":"2014-10-14T17:00:37Z",
  "url":"http://example.org/hello-london-article",
  "tags":"http://dbpedia.org/resource/London http://dbpedia.org/resource/London",
  "keyphrases": "http://example.org/keyphrases/Example"
  }
```


## Uploading articles through the `/content` endpoint (recommended)

If the recommendation engine is deployed at http://recommendation/app, then a couple of actions are available through http://recommendation/app/content:
* `POST /content/<id>` to upload a single article with the specified id. In that case, you can skip the _id_ field from the JSON. If both are present, the one from the URL will take precedence. Example:

```
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

```

* `POST /content` to upload a list of articles. Example

```
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
    }, {
      "id": "test-document-2",
      "content":"Example Hello New York!",
      "title":"Hello New York!",
      "published":"2014-10-14T17:00:37Z",
      "url":"http://example.org/hello-london-article",
      "tags":"http://dbpedia.org/resource/New_York_City http://dbpedia.org/resource/New_York_City",
      "keyphrases": "http://example.org/keyphrases/Example"
    }
]
```

* `GET /content/<id>` to retrieve the article with the specified id in the format above (example: `GET /content/test-document-1`). For example `GET /content/test-document-1` will yield:

```
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
```

* `DELETE /content/<id>` to delete the document with the specified id.

:exclamation: **Notes:**

* for the `/content` endpoint, the `PUT` method is equivalent to `POST` and the two can be used interchangeably
* `Content-type` header for `PUT` and `POST` requests should be `application/json`
* It's recommended (though not required) to set `Accept: application/json` header for all requests

## Uploading directly to Solr (not recommended)

Uploading data to Solr is best described by the relevant [Solr documentation](https://cwiki.apache.org/confluence/display/solr/Uploading+Data+with+Index+Handlers). The article format is the same as for `/content` endpoints.


## Reference: relevant Solr `schema.xml` sections:

```
Fields:

    <field name="id" type="string" indexed="true" stored="true" required="true" multiValued="false" />

    <field name="title" type="text_en_splitting" indexed="true" stored="true" termVectors="true" termPositions="true" termOffsets="true" omitNorms="true"/>
    <field name="summary" type="text_en_splitting" indexed="true" stored="true" termVectors="true" termPositions="true" termOffsets="true" />
    <field name="content" type="text_en_splitting" indexed="true" stored="true" termVectors="true" termPositions="true" termOffsets="true" />

    <field name="popularity" type="popularity_ext" /> <!-- TODO: we need to setup replication for this field if we ever get there -->
    <field name="recency" type="float" indexed="true" stored="true" docValues="true" /> <!-- we'll not use this probably -->
    <field name="published" type="tdate" indexed="true" stored="true" docValues="true" />
    <field name="url" type="string" indexed="true" stored="true" />

    <field name="tags" type="onto_tags" indexed="true" stored="true" termVectors="true" termPositions="true" termOffsets="true" />
    <field name="keyphrases" type="onto_tags" indexed="true" stored="true" termVectors="true" termPositions="true" termOffsets="true" />

    <!-- catchall field, containing all other searchable text fields (implemented via copyField further on in this schema  -->
    <field name="text" type="text_en_splitting" indexed="true" stored="false" multiValued="true"/>
    <!-- catchall text field that indexes tokens both normally and in reverse for efficient	leading wildcard queries. -->
    <field name="text_rev" type="text_general_rev" indexed="true" stored="false" multiValued="true"/>

```

And the `onto_tags` type used above

```
    <fieldtype name="onto_tags" stored="true" indexed="true" class="solr.TextField" >
    <analyzer>
    <tokenizer class="solr.WhitespaceTokenizerFactory"/>
    <filter class="solr.DelimitedPayloadTokenFilterFactory" encoder="float"/>
    </analyzer>
    </fieldtype>
```
