<nav>
  <span>Contents:</span>
  <a href="#{{ this.marked.headerPrefix }}npm-install">NPM Install</a>
  <a href="#{{ this.marked.headerPrefix }}cdn">CDN</a>
  <a href="#{{ this.marked.headerPrefix }}download">Download</a>
  <a href="#{{ this.marked.headerPrefix }}usage">Usage</a>
  <a href="#{{ this.marked.headerPrefix }}examples">Examples</a>
</nav>

#### Getting Started

There are three main methods of installation. The preferred method is to use <a href="https://www.npmjs.com/" target="_blank" rel="noopener nofollow">NPM (Node Package Manager)</a> to install the source in the _node_modules/_ directory of your project. This method enables you to compile your application's CSS, JavaScript, and SVGs. It also maintains a dependency link with the {{ this.package.nice }} source.

The other two options involve using distributed stylesheets and scripts via a public CDN. These are linked to the page using `<link>` and `<script>` tags.

#### NPM Install

```shell
npm install {{ this.package.name }}
```

#### CDN

Compiled styles and scripts in the **/dist** folder of the GitHub repository can be imported on the page using a CDN such as <a href="https://www.jsdelivr.com" target="_blank" rel="noopener nofollow">JsDelivr</a>. The following global stylesheet link can be copied and pasted into the `<head>` of your HTML document.

```html
<link href="{{ this.package.cdn.url }}@v{{ this.package.version }}/{{ this.global.dist }}/{{ this.global.entry.styles }}" rel="stylesheet">
```

The following global script source can be copied and pasted before your HTML document's closing `</body>` tag.

```html
<script src="{{ this.package.cdn.url }}@v{{ this.package.version }}/{{ this.global.dist }}/{{ this.global.entry.scripts }}"></script>
```

You can add SVG icons with the following snippet when the global script source is linked.

```html
<script>
  var patterns = new {{ this.global.entry.name }}();

  patterns.icons('{{ this.package.cdn.url }}@v{{ this.package.version }}{{ this.global.entry.svgs }}');
</script>
```

The following url is the base url for all distributed files available via a CDN.

```
{{ this.package.cdn.url }}@v{{ this.package.version }}/dist/
```

<a href="{{ this.package.cdn.source }}/tree/v{{ this.package.version }}/dist/" target="_blank" rel="noopener nofollow">Visit the GitHub repository to browse all distributed files available to the CDN</a>.

There are regular releases to the patterns which follow semantic versioning. You can keep up-to-date with <a href="https://help.github.com/en/github/receiving-notifications-about-activity-on-github/watching-and-unwatching-releases-for-a-repository" target="_blank" rel="noopener nofollow">new releases on each repository's releases page</a>.

#### Download

You may also download an archive of the repository to include in your project; <a href="{{ this.package.cdn.archive }}/v{{ this.package.version }}.zip" target="_blank" rel="noopener nofollow">Download v{{ this.package.version }}.zip</a>

#### Usage

##### Sass

You can import all of the Sass modules into a project from the source directory.

```scss
@forward '{{ this.package.name }}/src/scss/imports'
```

Or you can import individual Sass modules for any pattern from their respective pattern directory.

```scss
@forward '{{ this.package.name }}/src/components/accordion/accordion';
```

**Specificity**

Most patterns share the same filename for Sass and JavaScript (if used). Specifying that you need to import the Sass file for <a href="https://reactjs.org/" target="_blank" rel="noopener nofollow">React</a> (or other) applications may be necessary.

```scss
@forward '{{ this.package.name }}/src/components/accordion/_accordion.scss';
```

##### Tailwindcss

Importing Tailwindcss is compiled to a Sass file in the _src_ directory and a CSS file in the distribution folder and a CSS file in the _dist_ folder.

```scss
@forward 'node_modules/{{ this.package.name }}/src{{ this.global.entry.tailwindsass }}';
```

```html
<link href="{{ this.package.cdn.url }}@v{{ this.package.version }}{{ this.package.version }}{{ this.global.entry.tailwindcss }}" rel="stylesheet">
```

Refer to the [Tailwindcss page](tailwindcss) for more details.

##### Asset Paths and CDN

Stylesheets use the `url()` CSS function for loading external assets such as web fonts, images, and SVGs. By default, it looks for asset directories from one directory up from the distributed stylesheet. This means the directory structure of your application is expected to look like so.

```
styles/site-default.css
svg/..
```

However, you can set the path differently using the `$cdn` variable.

```scss
// $cdn: '../'; (default)
$cdn: 'path/to/assets/';
```

To modify, you should place this variable above your `@forward` rules for Sass files. You can set the CDN to another local path (such as an absolute path), or you can set it to the remote url within the `$tokens` variable map. This default uses <a href="https://www.jsdelivr.com" target="_blank" rel="noopener nofollow">JsDelivr</a> CDN to link the assets from the patterns GitHub repository and the tag of the installed version.

```scss
@use 'config/tokens' as *;

$cdn: map-get($tokens, 'cdn');
```

These are the default paths to the different asset types within the asset folder. Uncomment and set it to override their defaults.

```scss
$path-to-fonts: 'fonts/';
$path-to-images: 'images/';
$path-to-svg: 'svg/';
```

It would help if you used this for <a href="https://webpack.js.org/" target="_blank" rel="noopener nofollow">Webpack</a> projects using the <a href="https://webpack.js.org/loaders/css-loader" target="_blank" rel="noopener nofollow">css-loader</a> (such as React and projects scaffolded using create-react-app). Webpack will try to import the asset into your distributed stylesheet. Assuming you don't want to change the $cdn variable. In that case, you can disable the <a href="https://webpack.js.org/loaders/css-loader/#boolean" target="_blank" rel="noopener nofollow">url / image-set functions handling with a boolean</a>.

##### Resolving Paths to Patterns

You can add the string `node_modules/{{ this.package.name }}/src` to your "resolve" or "include" paths which will allow you to write the shorthand path.

```scss
@forward 'components/accordion/accordion';
```

or

```scss
@forward 'components/accordion/_accordion.scss';
```

Below is an example of the Sass `includePaths` option, an array of path strings that attempt to resolve your `@import` (deprecated), `@forward`, or `@use` rules if Sass can't find files locally.

```javascript
Sass.render({
    file: './src/scss/default.scss',
    outFile: 'site-default.css',
    includePaths: [
      `${process.env.PWD}/node_modules`,
      `${process.env.PWD}/node_modules/{{ this.package.name }}/src`
    ]
  }, (err, result) => {
    Fs.writeFile(`${process.env.PWD}/dist/styles/default.css`, result.css);
  }
});
```

Similar to the the [gulp-sass](https://www.npmjs.com/package/gulp-sass) `includePaths` option.

```javascript
gulp.task('sass', () => {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass.({includePaths: [
      `${process.env.PWD}/node_modules`,
      `${process.env.PWD}/node_modules/{{ this.package.name }}/src`
    ]})).pipe(gulp.dest('./css'));
});
```

<a href="https://github.com/sass/node-sass" target="_blank" rel="noopener nofollow">LibSass</a> and <a href="https://github.com/sass/dart-sass" target="_blank" rel="noopener nofollow">Dart Sass</a> also support using the `SASS_PATH` environment variable. This variable is valid when configuring a React Application using <a href="https://create-react-app.dev/docs/adding-a-sass-stylesheet" target="_blank" rel="noopener nofollow">React Scripts (Create React App)</a>.

```
SASS_PATH=node_modules:node_modules/{{ this.package.name }}/src
```

You can configure Webpack with the <a href="https://webpack.js.org/configuration/resolve/#resolvemodules" target="_blank" rel="noopener nofollow">resolve modules option</a>.

```javascript
module.exports = {
  //...
  resolve: {
    modules: [
      `${process.env.PWD}/node_modules`,
      `${process.env.PWD}/node_modules/{{ this.package.name }}/src`
    ]
  }
};
```

Below is an example for a <a href="https://nuxtjs.org/" target="_blank" rel="noopener nofollow">Nuxt.js</a> application configuration (which uses Webpack under the hood).

```javascript
const config = {
  css: ['@/assets/scss/main.scss'],
  styleResources: {
    scss: [
      '@/assets/scss/_variables.scss',
    ]
  },
  buildModules: [
    '@nuxtjs/style-resources',
  ],
  build: {
    extend(config) {
      config.resolve.modules.push(`${process.env.PWD}/node_modules`);
      config.resolve.modules.push(`${process.env.PWD}/node_modules/{{ this.package.name }}`);
    }
  }
}

export default config;
```

##### Scripts

The JavaScript source uses ES Modules. Import all JavaScript dependencies from the source and execute them individually.

```javascript
import Main from '{{ this.package.name }}/src/js/default'

Main.accordion();
```

Or you can import individual ES Modules for any pattern from their respective pattern directory and initialize them. Individual imports enable you to customize JavaScript behavior.

```javascript
import Accordion from '{{ this.package.name }}/src/components/accordion/accordion';

new Accordion();
```

Using <a href="https://rollupjs.org" target="_blank" rel="noopener nofollow">Rollup.js</a>, an IFFE function with all JavaScript modules is distributed in a single global script. You may import this script and initialize modules individually.

```html
<script src="{{ this.package.cdn.url }}@v{{ this.package.version }}/{{ this.global.dist }}/{{ this.global.entry.scripts }}"></script>

<script type="text/javascript">
  var patterns = new {{ this.global.entry.name }}();

  patterns.accordion();
</script>
```

The main JavaScript import file in the source will show how each module needs to be initialized if it isn't specified in the individual pattern's documentation.

#### SVGs

You can add the SVG icon sprite to the DOM several ways. The first example is from the main script source.

```javascript
import Main from '{{ this.package.name }}/src/js/default'

Main.icons('path/to{{ this.global.entry.svgs }}');
```

Or by using the <a href="https://github.com/CityOfNewYork/patterns-scripts">Patterns Script Utility</a> which is a dependency of the {{ this.package.nice }}.

```javascript
import Icons from '@nycopportunity/patterns-scripts/src/icons/icons'

new Icons('path/to{{ this.global.entry.svgs }}');
```

Or from the distributed global script.

```html
<script>
  var patterns = new {{ this.global.entry.name }}();

  patterns.icons('path/to{{ this.global.entry.svgs }}');
</script>
```

More details can be found on the [SVGs](svgs) page.

#### Examples

The {{ this.package.nice }} was created using the <a href="https://nycopportunity.github.io/patterns-framework/" target="_blank" rel="noopener nofollow">NYC Opportunity UI Patterns Framework</a>. You can find demonstrations of different integrations there for reference.
