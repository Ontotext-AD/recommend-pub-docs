---
title: Installation
layout: default
prev_section: quick-start
next_section: content-based-recommendations
category: Getting Started
permalink: v2_0_0-docs/installation/
---

## General prerequisites

* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html);
* [Elasticsearch 1.5.2](https://www.elastic.co/downloads/elasticsearch);
* Credentials to our Nexus publishing repos.

## Standard (and easy) setup

This guide is the bare minimum that needs to be done in order to setup a working system. Scalability and high availability are addressed separately.

1. [Install Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/1.5/_installation.html).
2. Create a `recommendation-reads` index using the following command containing the mappings and settings:

<pre><code>
curl -f -XPUT http://elastic-host:elastic-port/recommendations-reads -d '{
  "aliases": {
    "recommendations-reads": {}
  },
  "mappings": {
    "transitions": {
      "_all": {
        "enabled": false
      },
      "_ttl": {
        "enabled": true,
        "default": 15552000000
      },
      "properties": {
        "source_id": {
          "type": "string",
          "index": "not_analyzed"
        },
        "target_id": {
          "type": "string",
          "index": "not_analyzed"
        },
        "target_time": {
          "type": "date",
          "format": "date_time"
        }
      }
    },
    "reads": {
      "_all": {
        "enabled": false
      },
      "_ttl": {
        "enabled": true,
        "default": 15552000000
      },
      "properties": {
        "article_id": {
          "type": "string",
          "index": "not_analyzed"
        },
        "time": {
          "type": "date",
          "format": "date_time"
        },
        "user_id": {
          "type": "string",
          "index": "not_analyzed"
        },
        "weight": {
          "type": "float",
          "null_value": 1
        }
      }
    },
    "profiles": {
      "_all": {
        "enabled": false
      },
      "_id": {
        "path": "user_id"
      },
      "properties": {
        "due_update": {
          "type": "date",
          "format": "date_time"
        },
        "profile": {
          "type": "binary"
        },
        "user_id": {
          "type": "string",
          "index": "not_analyzed"
        }
      }
    }
  },
  "settings": {
    "index": {
      "refresh_interval": "30s"
    }
  },
  "warmers": {}
}'
</code></pre>

and `recommendation-docs` index with the following:

<pre><code>
curl -f -XPUT http://elastic-host:elastic-port/recommendations-docs -d '{
  "aliases": {
    "recommendations-docs": {}
  },
  "mappings": {
    "content": {
      "_all": {
        "enabled": false
      },
      "_id": {
        "index": "not_analyzed"
      },
      "properties": {
        "content": {
          "type": "string",
          "term_vector": "with_positions_offsets",
          "analyzer": "english_html"
        },
        "id": {
          "type": "string",
          "index": "not_analyzed"
        },
        "keyphrases": {
          "type": "string",
          "term_vector": "with_positions_offsets",
          "analyzer": "whitespace"
        },
        "popularity": {
          "type": "long",
          "store": true,
          "null_value": 0
        },
        "published": {
          "type": "date",
          "format": "dateOptionalTime"
        },
        "summary": {
          "type": "string",
          "term_vector": "with_positions_offsets",
          "analyzer": "english_html"
        },
        "tags": {
          "type": "string",
          "term_vector": "with_positions_offsets",
          "analyzer": "whitespace"
        },
        "title": {
          "type": "string",
          "term_vector": "with_positions_offsets",
          "analyzer": "english_html"
        },
        "url": {
          "type": "string",
          "index": "not_analyzed"
        }
      }
    }
  },
  "settings": {
    "index": {
      "analysis": {
        "filter": {
          "english_stop": {
            "type": "stop",
            "stopwords_path": "/path/to/stopwords/file"
          },
          "english_possessive_stemmer": {
            "type": "stemmer",
            "language": "possessive_english"
          },
          "delimited_words": {
            "split_on_numerics": "false",
            "catenate_all": "false",
            "generate_number_parts": "false",
            "catenate_numbers": "false",
            "type": "word_delimiter",
            "catenate_words": "true",
            "split_on_case_change": "true",
            "generate_word_parts": "true"
          },
          "english_stemmer": {
            "type": "stemmer",
            "language": "english"
          },
          "remove_numbers": {
            "pattern": "[0-9]+",
            "type": "pattern_replace",
            "replacement": ""
          }
        },
        "analyzer": {
          "english_html": {
            "type": "custom",
            "char_filter": [
              "html_strip"
            ],
            "filter": [
              "delimited_words",
              "remove_numbers",
              "english_possessive_stemmer",
              "lowercase",
              "english_stop",
              "porter_stem"
            ],
            "tokenizer": "standard"
          }
        }
      },
      "refresh_interval": "5s"
    }
  }
}'
</code></pre>

where `elastic-host` and `elastic-port` are the host and port where Elastic node is reachable from (by default it should be http://localhost:9200). Also you should replace `/path/to/stopwords/file` with the correct path to the file containing the stopwords.

3. Deploy [recommendations-web.war](http://maven.ontotext.com/content/repositories/publishing-releases/com/ontotext/recommend/recommendations-web/2.0.0/recommendations-web-2.0.0.war) to an application container of your choice. We usually use [Apache Tomcat](http://tomcat.apache.org/tomcat-7.0-doc/appdev/installation.html). Some Java system properties need to be set on the container (in the case of Tomcat, they should go in `bin/setenv.sh` or `bin/setenv.bat`, depending on your platform):

	3.1. `-Dcom.ontotext.recommend.backend.url=transport://<host1>:<port1>,<host2>:<port2>/<index-name>` - a URL to the backend. The host and port point to an Elastic cluster

	3.2. `-Dcom.ontotext.recommend.store.url=transport://\<host1\>:\<port1\>,\<host2\>:\<port2\>/\<index-name\>` - a URL to the content store. The host and port should point to an Elastic cluster

	3.3 `-Dcom.ontotext.recommend.requestDocAddress` - the base URL to use for requesting articles that do not exist. It should point to the _/enqueue_ endpoint in a _fetch-annotate-web_ deployment
