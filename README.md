# Patterns Documentation

Reusable documentation for pattern libraries created with the [`@nycopportunity/pttrn`](https://github.com/cityofnewyork/patterns-cli) CLI. Save time on writing common documentation.

## Table of Contents

* [Available Guides](#available-guides)
* [Template Variables](#template-variables)

### Available Guides

Name                                        | Description
--------------------------------------------|-
[Installation](./installation.md)           | How to install pattern libraries in projects that will use them.
[Contributing](./contributing.md)           | A general developers contribution and style guide.
[Patterns](./patterns.md)                   | An overview of the design patterns methodology for elements, components, objects, and utilities.
[Prototyping](./prototyping.md)             | A guide on prototyping with pattern libraries and Figma using the HTML to Figma plugin.
[Tailwindcss](./tailwindcss/tailwindcss.md) | An introduction to [Tailwindcss](https://tailwindcss.com/) and Slm markup that will use a [tailwindcss.js configuration](https://tailwindcss.com/docs/configuration) to create an interactive table demonstrating the configuration.

To include them in other pattern library static documentation sites use the `this.include()` method and double equals sign `==` render markdown files as HTML. Pass the full path to the desired file in the node modules directory.

```pug
== this.include('../node_modules/@nycopportunity/pttrn-docs/installation.md')
```

### Package variables

The following template variables are used by the documentation to refer to paths specific to your project. Template variables set to `this.package` refer to values stored in your project's **package.json** file. Some are required for [NPM package.json files](https://docs.npmjs.com/cli/v6/configuring-npm/package-json) and others are custom attributes.

Template Variable                           | Description
--------------------------------------------|-
`this.package.name`                         | The package name. See [NPM package.json documentation](https://docNPMjs.com/cli/v6/configuring-npm/package-json#name) for details. This may or may not include an [NPM Organization](https://docs.npmjs.com/organizations) prefix.
`this.package.nice`                         | A human readable package or pattern library name.
`this.package.version`                      | The package semantic version. See [NPM package.json documentation](https://docs.npmjs.com/cli/v6/configuring-npm/package-json#version) for details.
`this.package.cdn.url`                      | The preferred CDN url. Usually matches the pattern `https://cdn.jsdelivr.net/gh/{{ GitHub Organization }}/{{ repo }}`. This example uses [JS Delivr](https://www.jsdelivr.com/).
`this.package.cdn.source`                   | The GitHub repository url. Usually matches the pattern `https://github.com/{{ GitHub Organization }}/{{ repo }}`
`this.package.cdn.archive`                  | The GitHub archive url. Usually matches the pattern `https://github.com/{{ GitHub Organization }}/{{ repo }}/archive`.
`this.package.cdn.styles`                   | The local path to the fully distributed stylesheet module.
`this.package.cdn.scripts`                  | The local path to the fully distributed JavaScript module.
`this.package.cdn.svg`                      | The local path to the fully distributed SVG sprite.
`this.package.cdn.tailwindcss`              | The local path to the fully distributed Tailwindcss utility CSS stylesheet.
`this.package.cdn.tailwindsass`             | The local path to the fully distributed Tailwindcss Utility Sass stylesheet.
`this.package.instantiations.scripts`       | The main script module name used to instantiate the global pattern script. For example `new Default()` would instantiate a module named `Default`.

Below is how template variables are set in a sample **package.json** file.

```json
{
  "name": "@npm-organization/pattern-library",
  "nice": "My Pattern Library",
  "version": "1.0.0",
  "cdn": {
    "url": "https://cdn.jsdelivr.net/gh/github-organization/pattern-library",
    "source": "https://github.com/github-organization/pattern-library",
    "archive": "https://github.com/github-organization/pattern-library/archive",
    "styles": "/dist/scripts/default.css",
    "scripts": "/dist/scripts/default.js",
    "svg": "/dist/svg/svgs.svg",
    "tailwindcss": "/dist/styles/tailwindcss.css",
    "tailwindsass": "/dist/styles/_tailwindcss.scss"
  },
  "instantiations": {
    "scripts": "Default"
  },
  // ...
}
```

## Slm Variables

Additional template variables can be set in the project **config/slm.js**.

Template Variable          | Description
---------------------------|-
`this.marked.headerPrefix` | Prefix for markdown file heading IDs. This should be set to a blank string if no prefix is desired `''`. This assists with inline anchor links.