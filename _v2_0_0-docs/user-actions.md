---
title: User Actions
layout: default
prev_section: stop-recommending-content
category: HowTo's
permalink: v2_0_0-docs/user-actions/
---

<div class="info-badge">All example request URLs in this section are relative to the application root. For instance, if the web application is deployed at http://10.0.0.10:8080/api and the example says to request <code>/users</code>, the full URL would be http://10.0.0.10:8080/api/users.</div>

## Storing user actions

User actions are usually tracked by an external publication system. Currently we support one kind of action, which may be considered equivalent to "User has read" or "User is interested in". Depending on the content specifics and the audience, different systems may opt for different schema of registering user actions. For example, a user may be considered interested in an article if they commented on it or scrolled to the second screen, or a combination of both.

The relation is established by the following call:

```
POST /users/<userid>/history?contentid=existing_article_id
```

When the relation is established, a similar result is returned:

<pre><code>
{
  "version": "Ontotext Recommendations API",
  "type": "SUCCESS",
  "status": "OK",
  "message": "User updated: id = userid-1"
}
</code></pre>

Query parameters:

* `contentid` (required) - the article that the user is reading;
* `weight` (optional) - a value representing the amount of impact that this read has on the user profile. 

    <div class="info-badge">
    This value is currently being ignored but it can be used with a custom recommendation algorithm.
    </div>

* `timestamp` (optional) - the time when the action occurred. If not specified, the action occurs "now".

## Deleting the history of a user

You can delete the whole history of a user with the following call:

```
DELETE /users/<userid>/history
```

## Obtaining a list of user's read articles

The following method can be used to retrieve the number of articles a user has read and the list of the read articles:

```
GET /users/<userid>/history
```

An example result is shown below  (`GET /users/user_1/history`):

<pre><code>
{
  "version": "Ontotext Recommendations API",
  "type": "USER_INFO",
  "status": "OK",
  "response":
  {
      "userHistorySize": 4,
      "limit": 10,
      "offset": 0,
      "articles":
      [
          {
              "id": "doc-en-1",
              "title": "Hello London!",
              "summary": "Document 1 summary",
              "published": "2014-10-14T17:00:37Z",
              "url": "http://example.org/hello-london-article-1",
              "popularity": 5
          },
          {
              "id": "doc-en-2",
              "title": "Hello London Again!",
              "summary": "Document 2 summary",
              "published": "2014-10-15T17:00:37Z",
              "url": "http://example.org/hello-london-article-2",
              "popularity": 2
          }
      ]
  }
}
</code></pre>

Four of the seven reads of the returned articles are from a user with an ID `user_1`.

Query parameters:

`/users/<userid>/history` has the following query parameters:

- `limit` (optional, default = 10) - number of articles to retrieve;
- `offset` (optional, default = 0) - the offset of the first result in the history that you want to fetch;
- `recency` (optional, default = `<empty>`) - limits the list of the returned articles depending on the time they were read by the user. The supported values for recency are integers representing the maximum number of days to go back and ISO-formatted date-time representing the point after which the events are processed. For example, `recency=5` will only return articles newer than five days ago. If omitted, no age filter is applied.


## Obtaining the count of user reads

This is done by the following call:

```
GET /users/<userid>/history/size
```

The received result looks as follows:

<pre><code>
{
  "version": "Ontotext Recommendations API",
  "type": "USER_INFO",
  "status": "OK",
  "response":
  {
      "userHistorySize": 4
  }
}
</code></pre>

