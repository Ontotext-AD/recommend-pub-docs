This is the technical documentation of the Recommendation Engine API.
It contains information about how to get the engine up and running, how to ingest content into it, log user actions, tune the recommendation algorithm, scale, and deploy.

## How this documentation is organized

As the documentation uses Jekyll as a site generator, its structure follows Jekyll's conventions for organizing content. Basically, it consists of static and dynamic content. The static pages are the ones that are not updated regularly such as Home, Roadmap, and Help. The dynamic content are pages written in Markdown format, which you can find in the *_docs* and *_posts* folders, and which can be changed/updated at any moment. Such pages are the ones from the HowTo's and Release sections.

Jekyll also happens to be the engine behind GitHub Pages, which means we use Jekyll to host our documentation website from GitHub’s server.

## Adding content

### Add a new page to the documentation

* Add a page in markdown format to the *_docs* folder
* Add a config entry in *_data/docs.yml*

### Add a release note or news

* Copy the release note written in markdown format from GitHub
* Add it to *_posts* folder, while respecting the naming convention

## Test changes

* You need Jekyll installed
* Go to your local clone of the documentation repo and execute
`jekyll serve`

* The documentation is available on http://localhost:4000
* When changing the *.yml* file(s) a Jekyll restart is required
* For all other changes they are applied immediately

## Deploy changes

Just push to the master branch.

## Caveats

### Code blocks

When writing in markdown, need to wrap a multi-line code block in `<pre/>` in order to preserve white spaces. Single lines/embedded code snippets are fine the way they are.

### Front matter

* Category comes from the *_data/docs.yaml*
* Permalink is *docs/file-name/*
