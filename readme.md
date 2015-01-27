# Hyena Generator

> Yeoman generator for Hyena Apps @ UNL. Based on the Yeoman managed AngularJS generator.

## Usage

Install `generator-hyena`:
```
npm install -g generator-hyena
```

Make a new directory, and `cd` into it:
```
mkdir my-new-project && cd $_
```

Run `yo hyena`, optionally passing an app name:
```
yo hyena [app-name]
```

Run `grunt` for building and `grunt serve` for preview


## Generators

Available generators:

* [hyena](#app) (aka [hyena:app](#app))
* [hyena:controller](#controller)
* [hyena:directive](#directive)
* [hyena:filter](#filter)
* [hyena:route](#route)
* [hyena:service](#service)
* [hyena:provider](#service)
* [hyena:factory](#service)
* [hyena:value](#service)
* [hyena:constant](#service)
* [hyena:decorator](#decorator)
* [hyena:view](#view)

**Note: Generators are to be run from the root directory of your app.**

### App
Sets up a new AngularJS app, generating all the boilerplate you need to get started. The app generator also optionally installs Bootstrap and additional AngularJS modules, such as hyena-resource (installed by default).

Example:
```bash
yo hyena
```

### Route
Generates a controller and view, and configures a route in `app/scripts/app.js` connecting them.

Example:
```bash
yo hyena:route myroute
```

Produces `app/scripts/controllers/myroute.js`:
```javascript
hyena.module('myMod').controller('MyrouteCtrl', function ($scope) {
  // ...
});
```

Produces `app/views/myroute.html`:
```html
<p>This is the myroute view</p>
```

**Explicitly provide route URI**

Example:
```bash
yo hyena:route myRoute --uri=my/route
```

Produces controller and view as above and adds a route to `app/scripts/app.js`
with URI `my/route`

### Controller
Generates a controller in `app/scripts/controllers`.

Example:
```bash
yo hyena:controller user
```

Produces `app/scripts/controllers/user.js`:
```javascript
hyena.module('myMod').controller('UserCtrl', function ($scope) {
  // ...
});
```
### Directive
Generates a directive in `app/scripts/directives`.

Example:
```bash
yo hyena:directive myDirective
```

Produces `app/scripts/directives/myDirective.js`:
```javascript
hyena.module('myMod').directive('myDirective', function () {
  return {
    template: '<div></div>',
    restrict: 'E',
    link: function postLink(scope, element, attrs) {
      element.text('this is the myDirective directive');
    }
  };
});
```

### Filter
Generates a filter in `app/scripts/filters`.

Example:
```bash
yo hyena:filter myFilter
```

Produces `app/scripts/filters/myFilter.js`:
```javascript
hyena.module('myMod').filter('myFilter', function () {
  return function (input) {
    return 'myFilter filter:' + input;
  };
});
```

### View
Generates an HTML view file in `app/views`.

Example:
```bash
yo hyena:view user
```

Produces `app/views/user.html`:
```html
<p>This is the user view</p>
```

### Service
Generates an AngularJS service.

Example:
```bash
yo hyena:service myService
```

Produces `app/scripts/services/myService.js`:
```javascript
hyena.module('myMod').service('myService', function () {
  // ...
});
```

You can also do `yo hyena:factory`, `yo hyena:provider`, `yo hyena:value`, and `yo hyena:constant` for other types of services.

### Decorator
Generates an AngularJS service decorator.

Example:
```bash
yo hyena:decorator serviceName
```

Produces `app/scripts/decorators/serviceNameDecorator.js`:
```javascript
hyena.module('myMod').config(function ($provide) {
    $provide.decorator('serviceName', function ($delegate) {
      // ...
      return $delegate;
    });
  });
```

## Options
In general, these options can be applied to any generator, though they only affect generators that produce scripts.

### CoffeeScript
For generators that output scripts, the `--coffee` option will output CoffeeScript instead of JavaScript.

For example:
```bash
yo hyena:controller user --coffee
```

Produces `app/scripts/controller/user.coffee`:
```coffeescript
hyena.module('myMod')
  .controller 'UserCtrl', ($scope) ->
```

A project can mix CoffeScript and JavaScript files.

To output JavaScript files, even if CoffeeScript files exist (the default is to output CoffeeScript files if the generator finds any in the project), use `--coffee=false`.

### Minification Safe

**tl;dr**: You don't need to write annotated code as the build step will
handle it for you.

By default, generators produce unannotated code. Without annotations, AngularJS's DI system will break when minified. Typically, these annotations that make minification safe are added automatically at build-time, after application files are concatenated, but before they are minified. The annotations are important because minified code will rename variables, making it impossible for AngularJS to infer module names based solely on function parameters.

The recommended build process uses `ng-annotate`, a tool that automatically adds these annotations. However, if you'd rather not use it, you have to add these annotations manually yourself. Why would you do that though? If you find a bug
in the annotated code, please file an issue at [ng-annotate](https://github.com/olov/ng-annotate/issues).


### Add to Index
By default, new scripts are added to the index.html file. However, this may not always be suitable. Some use cases:

* Manually added to the file
* Auto-added by a 3rd party plugin
* Using this generator as a subgenerator

To skip adding them to the index, pass in the skip-add argument:
```bash
yo hyena:service serviceName --skip-add
```

## Bower Components

The following packages are always installed by the [app](#app) generator:

* angular
* angular-mocks
* angular-scenario


The following additional modules are available as components on bower, and installable via `bower install`:

* angular-animate
* angular-aria
* angular-cookies
* angular-messages
* angular-resource
* angular-sanitize

All of these can be updated with `bower update` as new versions of AngularJS are released.

## Configuration
Yeoman generated projects can be further tweaked according to your needs by modifying project files appropriately.

### Output
You can change the `app` directory by adding a `appPath` property to `bower.json`. For instance, if you wanted to easily integrate with Express.js, you could add the following:

```json
{
  "name": "yo-test",
  "version": "0.0.0",
  ...
  "appPath": "public"
}

```
This will cause Yeoman-generated client-side files to be placed in `public`.

Note that you can also achieve the same results by adding an `--appPath` option when starting generator:
```bash
yo hyena [app-name] --appPath=public
```

## Testing
Running `grunt test` will run the unit tests with karma.
