Extra features
==============

Sass support
------------

Install sass-loader:

```
yarn add --exact sass-loader
```

Add `sass-loader` in `module.rules[0].use`'s ExtractTextPlugin `use` block:

```diff
  use: ExtractTextPlugin.extract({
    fallback: 'style-loader',
    use: [
      {
        loader: 'css-loader',
        /* snip */
      },
+     /* Right below the postcss-loader block */
+     {
+       loader: 'sass-loader',
+       options: {
+         includePaths: join(__dirname, 'node_modules'),
+         outputStyle: DEBUG ? 'nested' : 'compressed'
+       }
+     }
```

In `postcss.config.js`, disable Stylelint because there's no way for it to check for lint violations /before/ Sass processes it.

React Hot module reloading
--------------------------

See: https://webpack.js.org/guides/hmr-react/

Phoenix
-------

To use this with [Phoenix](http://www.phoenixframework.org), take note of a few things.

You need to change the *source* and *destination* paths. You can just enter the following paths in the `install.sh` prompts.

```js
const SRC = './web/static'             // Phoenix 1.2 and below
const SRC = './lib/MYAPP/web/static'   // Phoenix 1.3+
const DEST = './priv/static'
```

`config/dev.exs` - You need to change watchers to use Webpack.

```diff
 config :myapp, Myapp.Endpoint,
   #...snip...
+  watchers: [
+    node: [
+      "node_modules/webpack/bin/webpack", "--watch", "--progress", "--colors",
+      cd: Path.expand("../", __DIR__)
+    ]
+  ]
```

In your deployment script, you need to use `npm run build`.

```ex
# It depends on how you deploy your app.
```