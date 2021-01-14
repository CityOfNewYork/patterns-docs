## Installation

Tailwindcss is not imported the same way as other patterns. All utilities are compiled to a Sass file...

```
{{ this.package.cdn.tailwindsass }}
```

(which can be imported in a Sass project)...

```scss
@import 'node_modules/{{ this.package.name }}{{ this.package.cdn.tailwindsass }}';
```

... and a CSS file in the **/dist** folder:

```
{{ this.package.cdn.tailwindcss }}
```

The CSS file can be included through a CDN with the latest release ({{ this.package.version }}).

```html
<link href="{{ this.package.cdn.url }}@v{{ this.package.version }}{{ this.package.cdn.tailwindcss }}" rel="stylesheet" type="text/css">
```

### As a dependency

It is also possible to install Tailwindcss as a dependency in your project and import the {{ this.package.nice }} tailwindcss.js configuration into your project. See the [Tailwindcss integration guides for various projects](https://tailwindcss.com/docs/installation). Below is the full path to the configuration file.

```javascript
node_modules/{{ this.package.name }}/config/tailwindcss.js
```

### Production distributions and PurgeCSS

It is highly recommended to use [PurgeCSS](https://purgecss.com/) to analyze the classnames used in your static markup files to remove unused CSS classes in your stylesheet. The output of Tailwindcss produces a very large stylesheet with many utilities that won't be used in your project. See the [Tailwindcss optimizing for production guide](https://tailwindcss.com/docs/optimizing-for-production).
