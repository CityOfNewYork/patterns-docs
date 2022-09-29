##### As a stylesheet

Tailwindcss is not imported the same way as other patterns. All utilities are compiled to a Sass file `{{ this.package.cdn.tailwindsass }}` which can be imported in a Sass project.

```scss
@import '{{ this.package.name }}/{{ this.global.src }}{{ this.global.entry.tailwindsass }}';
```

They are also available as compiled a CSS file in the **/dist** folder:

```
/{{ this.global.dist }}{{ this.global.entry.tailwindcss }}
```

The CSS file can be included through a CDN with the latest release ({{ this.package.version }}).

```html
<link href="{{ this.package.cdn.url }}@v{{ this.package.version }}/{{ this.global.dist }}{{ this.global.entry.tailwindcss }}" rel="stylesheet" type="text/css">
```

##### As a dependency

It is also possible to install Tailwindcss as a dependency in your project and import the {{ this.package.nice }} tailwindcss.js configuration into your project. See the <a href="https://tailwindcss.com/docs/installation" target="_blank" rel="noindex nofollow">Tailwindcss integration guides for various projects</a>.

```javascript
let tailwindcss = require('{{ this.package.name }}/config/tailwindcss.js');

module.exports = tailwindcss;
```

Modifications can be made to the configuration before exporting it. For example, the Tailwindcss Just-in-time (JIT) engine can be enabled for production distributions (described below).

```javascript
let tailwindcss = require('{{ this.package.name }}/config/tailwindcss.js');

tailwindcss.purge = [
  './views/**/*.twig',
  './views/**/*.vue'
];

module.exports = tailwindcss;
```

##### Production distributions

Tailwindcss (v3) will generate styles "on demand" by analyzing static templates or html files. It is recommended to enable this feature to keep your production stylesheets as small as possible. Read more about the <a href="https://tailwindcss.com/docs/upgrade-guide#migrating-to-the-jit-engine" target="_blank" rel="noindex nofollow">Tailwindcss JIT engine</a>.

The {{ this.package.nice }} disables the JIT engine so that all utilities are generated and available for development and integration in other projects.
