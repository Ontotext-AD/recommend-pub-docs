---
title: Stop recommending a piece of content
layout: default
prev_section: popular-content-recommendations
next_section: uploading-articles
category: HowTo's
permalink: v2_0_0-docs/stop-recommending-content/
---
In many systems, documents are not valid forever. An article may be deleted or an advertisement may expire.

In such cases it would be appropriate to stop recommending this piece of content.


* `DELETE /content/existing_doc_id`
* `DELETE /content?contentid=existing_doc_id` - an alternative to the method above in case the article's ID is in the form of an URI.

<div class="note-badge">
Deleting an article does not impact the read statistics and user profiles.
That is, if a user has read just one article about heavy machinery and this article gets deleted, then the user will still have one read operation and their profile would relate to content about heavy machinery.
</div>



