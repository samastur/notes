# conc.at 2018 workshop - Webpack

* [Slides](https://presentations.survivejs.com/webpack-the-good-parts/#/1)
* [Book](https://survivejs.com/webpack/)
* [Workshop code](https://github.com/survivejs-training/webpack-salzburg-02-03-2018)
* plugins: have access to the whole lifecycle of webpack
* loaders: more specific; are transformation functions on strings (return
  string too)
* entry: where to start bundling
* module.rules: how to process
* plugins: what additional processing to perform
	* webpack is basically a collection of plugins
* resolve: what happens on require/import
	* alias: create an alias for package for easier importing
	* extension: simplify imports by specifing file extensions to match
	* modules: specify which folders to check for modules to import
* going through chapters of SurviveJS book found at the address above
* source-map-visualization can show you mapping between source and build files
  and their source maps
* To get most out of css-loader, you should understand how it performs its
lookups. Even though css-loader handles relative imports by default, it doesn't
touch absolute imports (url("/static/img/demo.png")). If you rely on this kind
of imports, you have to copy the files to your project (for example with
copy-webpack-plugin and/or resolve-url-loader)
* inline definitions (loaders) slide: those are limited to webpack
