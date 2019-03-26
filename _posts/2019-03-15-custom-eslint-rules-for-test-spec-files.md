---
layout: post
title: Custom ESLint Rules For Test/Spec Files
source: 
author: 
categories: [Tech]
note: 'March 15th, 2019'
---

ESLint allows you to override rules for a specific directory by adding an additional `.eslint.js` file to that directory, but what if your specs live within the same directory as your source?

ESLint v4.1.0+ provides a way:

```json
{
  "extends": [ "airbnb" ],
  "env": { "browser": true },
  "rules": {
    "react/jsx-filename-extension": [ 1, { "extensions": [ ".js", ".jsx" ] } ],
    "comma-dangle": [ "error", "never" ]
  },
  "overrides": [
    {
      "files": [ "*.spec.js" ],
      "env": { "mocha": true },
      "plugins": [ "mocha" ],
      "rules": {
        "func-names": "off",
        "prefer-arrow-callback": "off",
        "one-var": [ "error", { "uninitialized": "consecutive" } ],
        "one-var-declaration-per-line": [ "error", "initializations" ]
      }
    }
  ]
}
```

The `overrides` key takes in an array of override objects where you can specify a glob pattern to test file names against and then standard config options to apply if the pattern matches. All config options are valid with the the exception of the `extends`, `overrides` and `root` keys.

More info [here](https://eslint.org/docs/user-guide/configuring#configuration-based-on-glob-patterns).
