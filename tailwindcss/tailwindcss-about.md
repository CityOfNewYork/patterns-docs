## About Utilities

Utilities allow the flexibility to change specific properties of every Pattern in certain views. For example, if a Pattern is set to `display: block` in one view but in another it needs to be set to `display: inline`, one solution would be to create another type of the Pattern. However, a UI developer may need to repeat this process for other Patterns. Writing alternate versions of Patterns is less efficient for UI development.

A utility class, such as `.inline { display: inline }`, allows the developer to reuse this attribute without creating a different pattern type or writing more CSS. This use case can be extended to every possible CSS attribute, such as color, position, font-families, margin, padding, etc. In addition, they can be bundled within media queries so certain utilities can work for specific screen sizes.

A simple example for using a utility to add padding to an element would be to use the utility `.p-1`. This will add `8px` of padding on all sides of an element.

**CSS**

```css
.py-1 { padding: 8px }
.px-2 { padding: 16px }
.border { border: 1px }
```

**HTML**

```html
<div class="border py-1 px-2">
  An element styled with utilities
</div>
```

**Renders**

<div class="border py-1 px-2">
  An element styled with utilities
</div>

### Utilities and Design Tokens

Utilities can also be used to create new components that are not readily available in a pattern library. This enables scale of the design system but maintains the relationship with the design through it's tokens. While some utilities are very CSS centric for front-end development purposes (display, accessibility, position, etc.) others are programmatic implementations of the design tokens that make the system unique (colors, typography, grid, etc.).

**CSS**

```css
.p-2 { padding: 16px }
.bg-blue { background-color: #284CCA }
.shadow-up { box-shadow: 8px 8px 0px 0px }
.text-base-white { color: #FFF }
```

**HTML**

```html
<div class="bg-blue shadow-up rounded p-2">
  <div class="text-base-white">An element styled with utilities.</div>
</div>
```

**Renders**

<div class="bg-blue shadow-up rounded p-2">
  <div class="text-base-white">An element styled with utilities.</div>
</div>