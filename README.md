This is the technical documentation of the Recommendation Engine API.
It contains information about how to get the engine up and running, how to ingest content into it, log user actions, tune the recommendation algorithm, scale, and deploy.

## How this documentation is organized

As the documentation uses Jekyll as a site generator, its structure follows Jekyll's conventions for organizing content. Basically, it consists of static and dynamic content. The static pages are the ones that are not updated regularly such as Home, Roadmap, and Help. The dynamic content are pages written in Markdown format, which you can find in the *_docs* and *_posts* folders, and which can be changed/updated at any moment. Such pages are the ones from the HowTo's and Release sections.

Jekyll also happens to be the engine behind GitHub Pages, which means we use Jekyll to host our documentation website from the GitHub’s server.

## Adding content

### Add a new page to the documentation

* Add a page in Markdown format to the *_docs* folder;
* Add a config entry in *_data/docs.yml*.

### Add a release note or news

* Copy the release note written in Markdown format from GitHub;
* Add it to *_posts* folder, while respecting the naming convention.

## Test changes

* You need Jekyll installed;
* Go to your local clone of the documentation repo and execute
`jekyll serve`;

* The documentation is available on http://localhost:4000;
* When changing the *.yml* file(s), you need to restart Jekyll;
* All other changes are applied immediately.

## Deploy changes

Just push them to the gh-pages branch.

## Caveats

### Code blocks

When writing in Markdown, you need to wrap a multi-line code block in `<pre><code>...</code></pre>`, in order to preserve white spaces. Single lines/embedded code snippets can be wrapped in single backticks (`). Everything within the backticks appear as-is, with no other special formatting. For more information, see [Markdown Basics](https://help.github.com/articles/markdown-basics/). 

### Front matter

* Category comes from the *_data/docs.yaml*;
* Permalink is *docs/file-name/*.
