# ember-native-class-codemod

[![Build Status](https://travis-ci.org/ember-codemods/ember-native-class-codemod.svg?branch=master)](https://travis-ci.org/ember-codemods/ember-native-class-codemod)
[![npm version](https://badge.fury.io/js/ember-native-class-codemod.svg)](https://badge.fury.io/js/ember-native-class-codemod)

A codemod for transforming your ember app code to native JavaScript class syntax
with decorators!

## Usage

First, install the dependencies that the codemod relies on. These are
addons that the codemod will add imports from:

```
ember install ember-classic-decorator
ember install ember-decorators 
```

Then, boot up your application. Then, the codemod can be run using the
following command:

```
npx ember-native-class-codemod http://localhost:4200/path/to/server [OPTIONS] path/of/files/ or/some**/*glob.js
```

The codemod accepts the following options:

| Option              | Value   | Default                         | Details                                                                                                                                          |
| ------------------- | ------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| --class-fields      | boolean | true                            | Enable/disable transformation using class fields                                                                                                 |
| --decorators        | boolean | true                            | Enable/disable transformation using decorators                                                                                                   |
| --classic-decorator | boolean | true                            | Enable/disable adding the [`@classic` decorator](https://github.com/pzuraq/ember-classic-decorator), which helps with transitioning Ember Octane |
| --type              | String  | Empty (match all types in path) | Apply transformation to only passed type. The type can be one of `services`, `routes`, `components`, `controllers`                               |
| --quote             | String  | 'single'                        | Whether to use double or single quotes by default for new statements that are added during the codemod.                                          |

### Gathering Runtime Data

The first argument that you must pass to the codemod is the URL of a running
instance of your application. The codemod opens up your application and analyzes
the classes directly, so it can transform them, which is why it needs this URL.
Any classes that were not analyzed will not be transformed. This includes
classes that are private to a module and never exported.

If you have any _lazily loaded_ modules, such as modules from Ember Engines,
you'll need to make sure that the URL you provide loads these modules as well.
Otherwise, the codemod will not be able to detect them or analyze them.

### Types

The `type` option can be used to further transforms to a particular type of
ember object within the application or addon. The types can be any of the
following:

| Type        | Option             |
| ----------- | ------------------ |
| Services    | --type=services    |
| Routes      | --type=routes      |
| Components  | --type=components  |
| Controllers | --type=controllers |

The path of the file being transformed is matched against the glob pattern of
the type to determine whether to run the specific transforms.

If a type is not provided, the codemods will run against all the **_types_** in
the path provided.

## Debugging

The codemods log execution information in the `codemods.log` file in the current
directory where the codemods are being executed. Specifically, details such as
failures and reasons for failures, are logged. This would be the recommended
starting point for debugging issues related to these codemods.

## Unsupported Types

While the codemods transforms many types of ember objects, it does not support
transformation of

- `ember-data` classes such as `DS.Model`, `DS.Adapter` etc
- Mixins
- Ember Objects with objects or arrays as direct properties (`actions` and
  `queryParams` are the exception). See `eslint-plugin-ember/avoid-leaking-state-in-ember-objects`
  for more details.
- Ember objects with computed properties that use the `meta` or `property`
  modifiers.

## More Transform Examples

<!--TRANSFORMS_START-->
* [ember-object](transforms/ember-object/README.md)
* [helpers](transforms/helpers)
<!--TRANSFORMS_END-->

## Contributing

### Installation

- clone the repo
- change into the repo directory
- `yarn`

### Running Tests

- `yarn test`

### Update Documentation

- `yarn update-docs`
