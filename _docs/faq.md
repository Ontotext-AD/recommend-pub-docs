---
title: FAQ
layout: default
prev_section: user-actions
next_section: troubleshooting
category: Miscellaneous
permalink: docs/faq/
---

## Is there a max number of articles one can POST to `/content` at a time?

As this method takes a single JSON in the request body, it is not streaming and the whole request has to fit in the memory. The only limitation is the available memory on the `recommendations-web` node. Practically speaking, chunks of up to 100mb will succeed most of the time. The number of articles for such chunks depends on the article size.
