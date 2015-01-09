---
Title: Stop recommending a piece of content
layout: docs
prev_section: advanced-recommendation-parameters
next_section: uploading-articles
category: HowTo's
permalink: recommendations-api/stop-recommending-content
---
In many systems documents are not valid forever. An article may be deleted or an advertisement may expire.

In these cases it would be appropriate to stop recommending this piece of content.

```
DELETE /recommend/content/<existing_doc_id>/?key=<api_key>
```
Please, note that deleting an article does not impact read statistics and users' profiles.

That is, if an user has read just one article - about heavy machinery - and this article gets deleted, then user would still have one read operation and their profile would relate to other content about heavy machinery.
