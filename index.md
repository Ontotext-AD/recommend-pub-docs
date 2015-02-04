---
layout: default
title: Home
---

This site aims to be a comprehensive guide to the Ontotext Recommendation engine. We will cover topics such as getting the engine up and running, ingesting content into the system, logging user actions, tuning the recommendation algorithm, scaling, and deployment.

## Who is this site for?

This is a technical documentation, written to be used by technical people. Whether you are an architect, evaluating how this recommendation engine fits as a component to your system, or you are a developer, who has already integrated it and actively employs its features - this is the complete reference. We also try to provide as much as possible insight into what we are currently working on and our future ideas via the roadmap page, and will keep you posted with news about releases and upgrade paths.

## What is the Recommendation engine?

The Recommendation engine is a system that takes content and user actions as input and provides recommended content based on contextual similarity and user profiles in JSON format. It also provides RESTful interface to tune the complex recommendation algorithm at runtime, by setting different weights for the user behaviour components, contextual boosts, and recency relevance decay.

### Advantages:

- The system is designed for high throughput and resilience. It implements a distributed architecture favouring Availability and Partition tolerance at the cost of eventual Consistency.

- Easy to scale horizontally by adding more nodes;

- High throughput - a fairly small setup of 6 AWS boxes can handle up to 10m API requests a day for ~$1500 a month;

- We also plan to develop built-in extension and tuning points. This will increase its capabilities to add more user actions, specific to the business case, and to weigh them appropriately.

- Although the recomemndations components integrate well with other Ontotext products, such as the Concept Extraction Service and GraphDB, it is not bound to RDF in any way. It can be integrated with other existing systems,
 which use whatever database to store their tags. The tags and keyphrases can contain anything which can be represented as string - numbers, hashes, URIs or words - and similarity is calculated on top of that.

## Helpful hints

<div class="info-badge">
Info badges are handy pieces of information.
</div>

<div class="note-badge">
Notes are comments or references that may save you time and unnecessary effort.
</div>

<div class="warning-badge">
Warnings are pieces of advice that turn your attention to things you should be cautious about.
</div>
