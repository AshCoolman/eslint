# Migrating to v3.0.0

ESLint v3.0.0 is the third major version release. We have made several breaking changes in this release, however, we believe the changes to be small enough that they should not require significant changes for ESLint users. This guide is intended to walk you through the changes

## Dropping Support for Node.js < 4

With ESLint v3.0.0, we are dropping support for Node.js versions prior to 4. If you are using an older version of Node.js, we recommend upgrading to at least Node.js 4 as soon as possible. If you are unable to upgrade to Node.js 4 or higher, then we recommend continuing to use ESLint v2.x until you are ready to upgrade Node.js.

**Important:** We will not be updating the ESLint v2.x versions going forward. All bug fixes and enhancements will land in ESLint v3.x.

## Changes to `"eslint:recommended"`

```json
{
    "extends": "eslint:recommended"
}
```

In 3.0.0, the following rules were added to `"eslint:recommended"`.

* [`no-unsafe-finally`](http://eslint.org/docs/rules/no-unsafe-finally) helps catch `finally` clauses that may not behave as you think.

The following rules were removed:

* [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle) used to be important because Internet Explorer 8 and earlier threw a syntax error when it found a dangling comma on object literal properties. However, [Internet Explorer 8 was end-of-lifed](https://www.microsoft.com/en-us/WindowsForBusiness/End-of-IE-support) in January 2016 and all other active browsers allow dangling commas.

The following rules were modified:

* [`complexity`](http://eslint.org/docs/rules/complexity) used to have a hardcoded default of 11 in `eslint:recommended` that would be used if you turned the rule on without specifying a maximum. The default is now 20. The rule actually always had a default of 20, but `eslint:recommended` was overriding it by mistake.

**To address:** If you want to mimic how `eslint:recommended` worked in v2.x, you can use the following:

```json
{
    "extends": "eslint:recommended",
    "rules": {
        "no-unsafe-finally": "off",
        "complexity": ["off", 11],
        "comma-dangle": "error"
    }
}
```

## Changes to `CLIEngine#executeOnText()`

The `CLIEngine#executeOnText()` method has changed to work more like `CLIEngine#executeOnFiles()`. In v2.x, `CLIEngine#executeOnText()` warned about ignored files by default and didn't have a way to opt-out of those warnings whereas `CLIEngine#executeOnFiles()` did not warn about ignored files by default and allowed you to opt-in to warning about them. The `CLIEngine#executeOnText()` method now also does not warn about ignored files by default and allows you to opt-in with a new, third argument (a boolean, `true` to warn about ignored files and `false` to not warn).

**To address:** If you are currently using `CLIEngine#executeOnText()` in your project like this:

```js
var result = engine.executeOnText(text, filename);
```

You can get the equivalent behavior using this:

```js
var result = engine.executeOnText(text, filename, true);
```

If you do not want ignored file warnings output to the console, you can omit the third argument of pass `false`.
