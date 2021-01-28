## Tailwindcss Logical

The [Tailwindcss Logical](https://github.com/stevecochrane/tailwindcss-logical) plugin extends the utility set with CSS logical properties. {{ this.package.nice }} includes this plugin in its Tailwindcss implementation. This particularly useful for languages that use right-to-left reading directions.

Utility           | Description
------------------|-
`.text-end`       | Set text to align at the *end* of the element.
`.text-start`     | Set text to end at the *start* of the element.
`.mie-{{ size }}` | Set margin (outer spacing) to the *end* of an *inline* element.
`.mis-{{ size }}` | Set margin (outer spacing) to the *start* of an *inline* element.
`.mbe-{{ size }}` | Set margin (outer spacing) to the *end* of an *block* element.
`.mbs-{{ size }}` | Set margin (outer spacing) to the *start* of an *block* element.
`.pie-{{ size }}` | Set padding (inner spacing) to the *end* of an *inline* element.
`.pis-{{ size }}` | Set padding (inner spacing) to the *start* of an *inline* element.
`.pbe-{{ size }}` | Set padding (inner spacing) to the *end* of an *block* element.
`.pbs-{{ size }}` | Set padding (inner spacing) to the *start* of an *block* element.

More supported properties are described in the [plugin's readme file](https://github.com/stevecochrane/tailwindcss-logical#whats-included).