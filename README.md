# rollup-plugin-html-entry

Use HTML files as entry points in your [rollup](https://github.com/rollup/rollup) bundle.
HTML files and [HTML imports](https://www.w3.org/TR/html-imports/) will be traversed, then all scripts found will be combined.
Optionally, write HTML files cleaned of `<script>`s.
This is particularly useful for [web components](https://www.webcomponents.org/introduction) and web applications in general.

```html
<!-- 0.html -->
<script>export const zero = 0;</script>
```

```html
<!-- 1.html -->
<script>export const one = 1;</script>
```

```html
<!-- 2.html -->
<script src="2.js"></script>
```

```js
// 2.js
export const two = 2;
```

```html
<!-- all-imports.html -->
<link rel="import" href="0.html">
<link rel="import" href="1.html">
<link rel="import" href="2.html">

```

Using `all-imports.html` as entry point will yield a bundle with exports for `zero`, `one`, and `two`.

So, this plugin works like [rollup-plugin-multi-entry](https://github.com/rollup/rollup-plugin-multi-entry) does, but using `<script>`s contained in HTML files as entry points.

## Install

```
$ npm install [--save-dev] rollup-plugin-html-entry
```

## Usage

In `rollup.config.js`:

```js
import htmlEntry from 'rollup-plugin-html-entry';

export default {
  entry: 'test/**/*.html',
  plugins: [htmlEntry()]
};
```

The `entry` above is the simplest form which simply takes a glob string.
You may pass an array of glob strings or an object with one or more of the following options:

```js
export default {
  entry: {
    // Arrays of globs to include
    include: ['index.html', 'and/globs/**/*.html'],
    // Arrays of globs to exclude
    exclude: ['excluded-file.html', 'and/globs/*.to.be.excluded.html'],
    // Arrays of globs that should remain external to the bundle
    external: ['lazy-imports.html', 'and/globs/*.to.be.omitted.html']
  }
  // ...
};

```

By default HTML files will be not written. If `output` option is present, HTML files stripped of `<script>`s will be written into specified path.

```js
export default {
  entry: 'index.html',
  plugins: [htmlEntry({ output: "build" })]
  // ...
};
```

Finally, you may not need to export anything from the rolled-up bundle for web applications. In
such cases, use the `exports: false` option like so:

```js
export default {
  entry: 'index.html',
  plugins: [htmlEntry({ exports: false })]
};
```

## License

MIT
