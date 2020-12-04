# Patterns Documentation

Reusable documentation for pattern libraries created with the [`@nycopportunity/pttrn-cli`](https://github.com/cityofnewyork/patterns-cli).

## Table of Contents

Name                                        | Description
--------------------------------------------|-
[Installation](./installation.md)           | How to install pattern libraries in projects that will use them.
[Contributing](./contributing.md)           | A general developers contribution and style guide.
[Patterns](./patterns.md)                   | An overview of the design patterns methodology for elements, components, objects, and utilities.
[Prototyping](./prototyping.md)             | A guide on prototyping with pattern libraries and Figma using the HTML to Figma plugin.
[Tailwindcss](./tailwindcss/tailwindcss.md) | An introduction to [Tailwindcss](https://tailwindcss.com/) and Slm markup that will use a [tailwindcss.js configuration](https://tailwindcss.com/docs/configuration) to create an interactive table demonstrating the configuration.

To include them in other pattern library static documentation sites use the `this.include()` method and double equals sign `==` render markdown files as HTML.

```pug
== this.include('../node_modules/@nycopportunity/pttrn-docs/installation.md')
```