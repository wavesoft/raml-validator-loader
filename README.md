# raml-validator-loader

A webpack plugin that converts RAML rules into pure javascript-only validation routines

## Usage

```js
import TypeValidators from './ramls/myTypes.raml';

// The raml-validator-loader will convert the RAML document into an object with
// a validation function for every type defined in your RAML.

var userInput = read_user_input();
var validationErrors = TypeValidators.MyType(userInput);

// Display errors
validationErrors.forEach((error) => {
  console.log('Error message: /' + error.path.join('/'));
  console.log('  Error path : ' + error.message);
});

```

## Installation

First install the module

```
npm install --save-dev raml-validator-loader
```

And then use it in your `webpack.config.js` like so:

```js
    module: {
        loaders: [
            { test: /\.raml$/, loader: "raml-validator-loader" }
        ]
    }
```

## API Reference

The RAML document will be translated in a module with the following structure:

```js
module.exports = {

  /**
   * For every type in the RAML document, an equivalent validation function
   * will be generated by the loader.
   */
  RamlTypeName: function( data ) {
    ...

    // Each validation function will return an array of RAMLError objects
    // with the information for the validation errors occured.
    return [ RAMLError() ];
  },

}
```

The `RAMLError` class exposes the following properties:

<table>
  <tr>
    <th>Name</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <th><code>.path</code></th>
    <td><code>Array</code></td>
    <td>Returns the path in the object where the error is located. For example <code>['arrayProperty', 3, 'childProperty']</code></td>
  </tr>
  <tr>
    <th><code>.message</code></th>
    <td><code>String</code></td>
    <td>Returns the human-readable error message.</td>
  </tr>
</table>
