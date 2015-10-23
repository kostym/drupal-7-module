# Kostym in Drupal 7

If you haven't already read the [Kostym documentaion](https://github.com/kostym/documentation), please do that first.

* [Getting started](#getting-started)
	* [Why patch core?](#why-patch-core)
	* [Why patch ctools?](#why-patch-ctools)
* [Things explained and examples](#things-explained-and-examples)
	* [kostym.libraries.json](#kostymlibrariesjson) 
	* [Components get auto enabled](#components-get-auto-enabled)
	* [Code examples](#code-examples)

## Getting started

Get Drupal ready by:

1. Patch core with [`core-7.x-allow-modules-in-theme.patch`](https://github.com/kostym/drupal-7-module/tree/7.x-1.x/patches) [Why?](#why-patch-core)
* Patch ctools with [`ctools-1.x-allow-ctools-plugins-in-kostym-components.patch`](https://github.com/kostym/drupal-7-module/tree/7.x-1.x/patches) [Why?](#why-patch-ctools)
* Enable this kostym module

then make your theme ready by:

1. Create a `kostym_components` folder in your theme. [Not clear?](#things-explained-and-examples)
* Add a `kostym.libraries.json` in the `kostym_components` you just created. [What is this?](#kostymlibrariesjson)

**Done!** Now your site is ready to wear the Kostym like a boss!

### Why patch core?
Some Kostym componets are modules. So to make it possible for Drupal to find them even though they are place in a theme, we need to patch core a bit. And remember, keep calm and carry on.

### Why patch ctools?
This is to keep the total number of modules down. The patch make it possible to create ctools plugins in components without creating a new module.

## Things explained and examples

To make it a bit more clear where things should go, look at this
example tree structure of a Drupal profile called `project`

It has two external libraries `modernizr` and `swiper`, a theme called `project_theme`, a kostym component called `ExampleModule` and an other called `ExampleCtools`.

<pre>
.
├── libraries
│   ├── <b>modernizr</b>
│   └── <b>swiper</b>
├── project.info
├── project.install
├── project.make
├── project.profile
├── modules
└── themes
    └── <b>project_theme</b>
	     ├── README.md
	     ├── dist
	     ├── favicon.ico
	     ├── gulpfile.js (<a href="https://github.com/kostym/drupal-7-gulpfile.js">Gulpfile.js boilerplate here</a>)
	     ├── images
	     ├── <b>kostym_components</b> (Your components goes here)
	     │   ├── <b>ExampleModule</b> 
	     │   │   ├── ExampleModule.module (A module in a theme, pretty cool!)
	     │   │   ├── ExampleModule.info
	     │   │   ├── _ExampleModule.scss
	     │   │   └── ExampleComponent.tpl.php
   	     │   ├── <b> ExampleCtools </b> 
	     │   │   ├── ctools-content_types (Ctools plugin without being a module, also pretty cool!)
	     │   │   │   └── ExampleCtools.inc
	     │   │   ├── ExampleCtools.js
	     │   │   └── _ExampleCtools.scss
	     │   └── <b>kostym.libraries.json</b> (This is explaiend below)
	     ├── logo.png
	     ├── project_theme.info
	     ├── package.json
	     ├── scss
	     └── template.php
</pre>

### kostym.libraries.json

Kostym makes it easy to load external libraries into your theme as you can see in the example below.

In this example we want to load `modernizr` on all pages and `swiper` only when needed.
So on `modernizr` we set the `auto-add` setting to `true` and on `swiper` to `false`. 

Now `modernizr` gets loaded on all pages and `swiper` only gets loaded if a component calls it by `drupal_add_library('kostym', 'swiper')`.

 
```javascript
{
  "modernizr": {
    "js": {
      "modernizr/modernizr.custom.min.js": []
    },
    "auto-add": true
  },
  "swiper": {
    "js": {
      "swiper/dist/js/swiper.jquery.js": []
    },
    "css": {
      "swiper/dist/css/swiper.css": []
    },
    "auto-add": false
  }
}
```

### Components get auto enabled 

To make it as easy as possible to work with components, **all components that are modules gets auto enabled** when cache is cleared. So you should not have any components not in use lying around.

### Code examples

Go to [Drupal 7 kostym component examples](https://github.com/kostym/drupal-7-examples).  
