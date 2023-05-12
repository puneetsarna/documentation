# NGINX Docs

This repo contains all of the user documentation for NGINX's enterprise products, as well as the requirements for linting, building, and publishing the documentation.

Docs are written in Markdown. We build the docs using [Hugo](https://gohugo.io) and host them on [Netlify](https://www.netlify.com/).

## ✨ Getting Started

1. To install Hugo locally, refer to the [Hugo installation instructions](https://gohugo.io/getting-started/installing/).

    > **NOTE**: We don't support Hugo versions newer than `v0.91`. 
    > This means you'll need to install Hugo by following the "Prebuilt binaries" instructions for your operating system.

2. We use `markdownlint` to check that Markdown files are correct. Use `npm` to install `markdownlint-cli`:

    ```bash
    npm install -g markdownlint-cli   
    ```

---

## 📨 Feature Requests, Support and Issue Reporting

For now, open an [issue](https://github.com/nginxinc/docs/issues).

## Local Docs Development

To build the docs locally, run the desired `make` command from the docs directory:

```text
make clean          -   removes the local `public` directory, which is the 
                        default output path used by Hugo
make docs           -   runs a local hugo server so you can view docs in your 
                        browser while you work
make hugo-mod       -   cleans the Hugo module cache and fetches the latest 
                        version of the theme module
make docs-local     -   runs the `hugo` command to generate static HTML files 
                        into the `public` directory
make docs-drafts    -   runs the local hugo server and includes all docs marked 
                        with `draft: true`
```

## Linting

- To run the markdownlint check, run the following command from the docs directory:

    ```bash
    markdownlint -c docs/mdlint_conf.json content
    ```

    Note: You can run this tool on an entire directory or on an individual file.

## Add new docs

### Generate a new doc file using Hugo

To create a new doc file that contains all of the pre-configured Hugo front-matter and the docs task template, **run the following command in the docs directory**:

`hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>`

e.g.,

`hugo new getting-started/install.md`

The default template -- task -- should be used for most docs. To docs from the other content templates, you can use the `--kind` flag:

`hugo new tutorials/deploy.md --kind tutorial`

The available content types (`kind`) are:

- concept: Helps a customer learn about a specific feature or feature set.
- tutorial: Walks a customer through an example use case scenario; results in a functional PoC environment.
- reference: Describes an API, command line tool, config options, etc.; should be generated automatically from source code. 
- troubleshooting: Helps a customer solve a specific problem.
- openapi: Contains front-matter and shortcode for rendering an openapi.yaml spec

## How to format docs

### How to format internal links

Format links as [Hugo refs](https://gohugo.io/content-management/cross-references/). 

- File extensions are optional.
- You can use relative paths or just the filename. (**Paths without a leading / are first resolved relative to the current page, then to the remainder of the site.**)
- Anchors are supported.

For example:

```md
To install <product>, refer to the [installation instructions]({{< ref "install" >}}).
```

### How to use Hugo shortcodes

You can use [Hugo shortcodes](/docs/themes/f5-hugo/layouts/shortcodes/) to do things like format callouts, add images, and reuse content across different docs. 

For example, to use the note callout:

```md
{{< note >}}Provide the text of the note here. {{< /note >}}
```

The callout shortcodes also support multi-line blocks:

```md
{{< caution >}}
You should probably never do this specific thing in a production environment. 

If you do, and things break, don't say we didn't warn you.
{{< /caution >}}
```

Supported callouts:

- `caution`
- `important`
- `note`
- `see-also`
- `tip`
- `warning`

A few more fun shortcodes:

- `fa`: inserts a Font Awesome icon
- `img`: include an image and define things like alt text and dimensions
- `include`: include the content of a file in another file (requires the included file to be in the content/includes directory; will be deprecated in favor of readfile)
- `link`: makes it possible to link to a file and prepend the path with the Hugo baseUrl
- `openapi`: loads an OpenAPI spec and renders as HTML using ReDoc
- `raw-html`: makes it possible to include a block of raw HTML
- `readfile`: includes the content of another file in the current file (intended to replace `include`)
- `bootstrap-table`: formats a table using Bootstrap classes; accepts any bootstrap table classes as additional arguments, e.g. `{{< bootstrap-table "table-bordered table-hover" }}`
