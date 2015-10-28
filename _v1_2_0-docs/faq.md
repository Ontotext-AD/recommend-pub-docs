---
title: FAQ
layout: default
prev_section: user-actions
next_section: troubleshooting
category: Miscellaneous
permalink: v1_2_0-docs/faq/
---

## Is there a max number of articles one can POST to `/content` at a time?

As this method takes a single JSON in the request body, it is not streamed and the whole request has to fit in the memory. The only limitation is the available memory on the `recommendations-web` node. Practically speaking, chunks of up to 100mb succeed most of the time. The number of articles for such chunks depends on the article size.

## I see messages suggesting memory leak in Tomcat while shutting down/re-deploying recommendations-web, how to fix it?

Messages such as this one are common when shutting down our web applications:
<pre><code>
Jan 19, 2015 3:01:39 AM org.apache.catalina.loader.WebappClassLoader checkThreadLocalMapForLeaks
SEVERE: The web application [/recommendations-web] created a ThreadLocal with key of type [com.codahale.metrics.ThreadLocalRandom$1] (value [com.codahale.metrics.ThreadLocalRandom$1@6475dc22]) and a value of type [com.codahale.metrics.ThreadLocalRandom] (value [com.codahale.metrics.ThreadLocalRandom@68008a89]) but failed to remove it when the web application was stopped. Threads are going to be renewed over time to try and avoid a probable memory leak.
</code></pre>

Some of these are generated by code that is not directly under our control such as the metrics library and the datastax driver. There is no easy way for us to fix this. We can, however, guarantee that you will have no issues if you stick to restarting Tomcat each time a war file is being re-deployed and that these messages can safely be ignored on Tomcat shutdown.

A memory leak is likely to be created in [this case](http://tomcat.apache.org/tomcat-7.0-doc/deployer-howto.html#Deploying_on_a_running_Tomcat_server).
Basically, it means that re-deploying the same application (`recommendations-web`/`solr`) on a running Tomcat server, is likely to create memory leaks. As long as you restart the Tomcat server for backing up/redeployment, there will be no memory leaks.