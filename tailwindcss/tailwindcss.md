## Tailwindcss

{{ this.package.nice }} integrates [Tailwindcss](https://tailwindcss.com), a CSS utility-first framework that is customized with {{ this.package.nice }} design tokens. There are three parts to the Tailwindcss configuration.

* **Core Plugins**: This array is a white list of utility plugins that defines what sets of utilities will be compiled in the final stylesheet distribution. [Source documentation](https://tailwindcss.com/docs/configuration#core-plugins). Example; the core plugin for padding is `padding`. Adding or removing it to the array will determine wether those utilities are compiled to the global stylesheet.

* **Variants**: This object contains variants that represent different states that the utilities appear in such as media queries, `:hover`, and `:focus` states. [Source documentation](https://tailwindcss.com/docs/configuring-variants). Example; to have padding only appear for desktop screens within {{ this.package.nice }} the variant `desktop:` is added to the `.p-1` utility: `<div class="desktop:p-1"></div>`.

* **Theme**: This object contains {{ this.package.nice }} tokens for particular utilities such as font families, colors, margin, padding, etc. [Source documentation](https://tailwindcss.com/docs/theme). Example; the padding plugin is customized to use `8px` as the basis for all padding increments. `.p-2` would add `8px × 2 = 16px` padding on all sides of an element: `<div class="p-2"></div>`.

The current <a href="{{ this.package.cdn.source }}/blob/main/config/tailwindcss.js">configuration source</a> describes which core plugins are enabled, what variants they use, and if they are themed with the {{ this.package.nice }} design tokens.
