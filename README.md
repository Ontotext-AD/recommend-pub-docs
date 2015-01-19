recommend-pub-docs
==================

Documentation for the Recommendation services

This is the technical documentation of the Recommendation Engine API.
It contains information about how to get the engine up and running, how to ingest content into it, log user actions, tune the recommendation algorithm, scale, and deploy.

The documentation uses Jekyll as a site generator. Therefore, it takes a template directory containing raw text files in various formats, runs it through Markdown (or Textile) and Liquid converters, and spits out a complete, ready-to-publish static website suitable for serving with your favorite web server. Jekyll also happens to be the engine behind GitHub Pages, which means you can use Jekyll to host your project’s page, blog, or website from GitHub’s servers for free.

## Dynamic content

1. How to's
2. News/Release notes

## Static pages

1. Roadmap
2. Help
3. Home

# How to

## Add a new page to the documentation

* Add a page in markdown in _docs
*  Add a config entry in _data/docs.yml

## Add release notes

* Copy release notes markdown from GitHub
* Add to _posts, while respecting the naming convention

## Test changes

* You need Jekyll installed
* Then go to your local clone of the documentation repo and execute {code}jekyll serve{code}
* The documentation is then available on http://localhost:4000
* When changing the *.yml file(s) a Jekyll restart is required
* For all other changes they're applied immediately

## Deploy changes

Just push to the master branch.

# Caveats

## Code blocks

When writing in markdown, need to wrap a multi-line code block in <pre/> in order to preserve white spaces. Single lines/embedded code snippets are fine the way they are.

## Front matter

* Category comes from the _data/docs.yaml
* Permalink is docs/file-name/
