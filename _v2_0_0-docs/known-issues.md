---
title: Known Issues
layout: default
prev_section: upgrade-to-a-newer-version
next_section: troubleshooting
category: Miscellaneous
permalink: v2_0_0-docs/known-issues/
---

## `GET /content/{id}` - doesn't appear to work with the article ID as URI

This is related to a spring bug/limitation when a URI is passed as a `@PathVariable`.
For now, we suggest the following [workaround](http://stackoverflow.com/questions/16332092/spring-mvc-pathvariable-with-dot-is-getting-truncated). We will look at it again as soon as possible.
