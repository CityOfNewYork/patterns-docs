Starting the development server (assuming you've added the recommended [npm scripts](../readme.md#npm-scripts) to your package.json) will enable compilation and previewing changes to patterns;

```shell
$ npm start
```

The most important changes developers may need to make are to files within two directories: The **src/** directory, which includes all of the pattern source including scripts, styles, and template source, and the **config/** directory, which includes all of the configuration for the different node libraries and global variables for the Patterns.

Every Pattern is developed with Style, JavaScript, and Markup dependencies bundled together so they can all be exported and imported independently of the rest of the Patterns.

    src/{{ pattern type }}/{{ name }}/{{ name }}.{{ extension }}

For example, all of the relevant **Accordion Component** dependencies live in:

    src/component/accordion/accordion.slm   // Markup
    src/component/accordion/accordion.js    // JavaScript
    src/component/accordion/_accordion.scss // Styling
    src/component/accordion/accordion.md    // Documentation
    src/component/accordion/readme.md       // Developer Usage

The `make` command takes care of managing this organization for you when creating new patterns using the command.

### Style guide

#### JavaScript

JavaScript for patterns are written in ES module syntax and bundled by [Rollup.js](https://rollupjs.org/guide/en/#es-module-syntax). The module format provides benefits for developing component based design systems by scoping and encapsulating pattern functionality as an object in a single script file to be exported and imported by other concerned scripts.

The patterns generally use the simplest implementation of module export and import statements.

##### Export / Import

```javascript
export default Accordion;
```

Read more about exports in the [MDN Web Docs](https://wiki.developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export).

```javascript
import Accordion from 'components/accordion/accordion';
```

Read more about imports in the [MDN Web Docs](https://wiki.developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import).

##### Classes

The use of classes that correspond with the pattern name assist in clarifying instances of the pattern within various scripts. Each pattern should have a class declaration that will enable it to be instantiated as an object with relevant data and methods stored as instance properties.

```javascript
class Accordion {
  constructor(element = false, settings = {}) {
    this.element = element;

    this.settings = settings;

    // constructor does something here

    return this;
  }

  method() {
    // method does something here

    return this;
  }
}
```

The `constructor` is a special method that runs when the object is initialized. Initializing the method above would be done by `new Accordion()`. The option is available to pass `element` and/or `settings` that will be set to intial instance properties within the constructor. These properties can be used for reference or transformed by the object later based on user interaction.

More about classes can be read about in the [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes).

##### Selector Strategy

If a module requires targeting a DOM element by a selector, it is better to use data attributes with `js`; `data-js=”accordion”` or `data-js=”toggle”`. While using classes or ids as targets is less preferable, if it is required, it should have a `js` prefix in the name to avoid confusion: `.js-` or `#js-`.

The selector can be stored as a static property of the class object before it is exported.

```javascript
Accordion.selector = '[data-js*="accordion"]';
```

Then the component would be attributed in the DOM like so:

```html
<div data-js="accordion"> ... </div>
```

Note: the asterisk in this selector permits the use of multiple `data-js` attribute values in the event it is required.

```javascript
<div data-js="accordion track toggle"> ... </div>
```

In concerned scripts where the module is imported the component can be selected and passed to an instance of itself. Below is an example of a single DOM instance.

```javascript
(element => {
    if (element) new Accordion(element);
})(document.querySelector(Accordion.selector));
```

If there are multiple instances of the element in the DOM:

```javascript
(elements => {
  elements.forEach(element => {
    new Accordion(element);
  });
})(document.querySelectorAll(Accordion.selector));
```

Other selectors may be needed by the module for child elements. These can be stored in an additional static property `selectors`:

```javascript
Accordion.selectors = {
  CHILD: '[data-accordion*="child"]'
};
```

##### Linting

By default JavaScript is linted by ESLint using the [Google Standard Configuration](https://github.com/google/eslint-config-google).

#### Aria Attributes

An example includes `aria-controls` which is typically set to a button element that toggles another element. It is easier to read `id="aria-c-{{ element name }}"` on the target element name and understand that it is influenced by another accessible control element. In this case the toggling control would have the `aria-controls` attribute set as "aria-c-{{ element name }}".

#### Styles

Styles for patterns are written in Sass using the primary implementation [Dart Sass](https://github.com/sass/dart-sass). Dart Sass recieves the latest features so there will be some differences and benefits. For example; the `@import` at-rule is deprecated and replaced with `@use` and `@forward`. Both of which are more module based versions of `@import`.

```scss
@use `config/accordion`;
```

`@use` rules must appear at the top of module stylesheet and variables, functions, and mixins from a module stylesheet are prefixed. From the example above, a variable `$bg` from the accordion configuration file would be retrieved using the `accordion` prefix:

```scss
.c-accordion {
  background-color: accordion.$bg;
}
```

Read more about the @use at-rule in the [Sass Documentation](https://sass-lang.com/documentation/at-rules/use).

##### BEM

Styles are written accordion to in a modified block, element, modifier (BEM) standard that includes prefixes for components:

```css
.{{ variant: }}{{ type }}-{{ block }}{{ __element and/or --modifyer }} {
  ...
}
```

* `.c-accordion { }` Block, the root element.
* `.c-accordion__child { }` Element, a child of the block. A double underscore `__` is preferred over a single '_' underscore but it may be used as long as the project is consistent.
* `.c-accordion--type { }` Modifier, a variant or type of the root element. A double `--` dash is preferred over a single dash `-` but it may be used as long as the project is consistent. Modifiers may be less common and should be avoided if possible given the availability of CSS Utilities that enable the transformation of styled component properties in different contexts.

##### Prefixes

* `.c-` for components
* `.o-` objects

There are no prefixes for elements and utilities.

##### Variants

Responsive and print variants precede the block selector and are separated with a semicolon:

```css
.desktop:c-accordion { }
```

##### Utilities

Most utilities are sources from the [Tailwindcss](https://tailwindcss.com/) utility framework. However, there may be a need to extend or create custom CSS utilities in a project. The recommended naming convention would be as follows:

```css
.{{ variant: }}{{ project prefix }}-{{ attribute acronym or abbreviation }}-{{ value }} {
  {{ css attribute }}: {{ css value }}
}
```

* `{{ variant }}` - The responsive breakpoint name.
* `{{ project prefix }}` - A project specific prefix to distinguish the utility from Tailwindcss default utilities.
* `{{ attribute acronym or abbreviation }}` - The first letter of each word in the attribute. Abbreviations are less preferred but may be necessary to avoid conflicts with matching acronyms.
* `{{ value }}` - The value. Units are recommended for specific pixel definitions but this may be unitless because percentages and `vh` values could be more easily interpreted as `100`, `1/2`, `half`, or `full`. It may also be a value exponent such as `1`, `2`, `3` meaning 1 times, 2 times, or 3 times a base value.
* `{{ css attribute }}` - CSS attribute
* `{{ css value }}` - CSS attribute value

For example:

```css
@media screen and (min-width: 960px) {
  .tablet\:wc-mh-280px {
    min-height: 280px
  }
}
```

```html
<div class="tablet:wc-mh-280px"> ... </div>
```

Another common scenario for utilities is to share and reuse type styling for heading and other typography specific tags. In which case having the selector match the tag name is preferred.

`.h1`, `.h2`, `.h3`, `.h4`, `.h5`, `.h6`, `.small`, `.blockquote`, etc.

##### Linting

By default Sass is linted using the [stylelint standard config](https://github.com/stylelint/stylelint-config-standard).

<!-- #### Markup -->

<!-- ##### Pa11y -->

<!-- #### Documentation -->