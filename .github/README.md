# @jetvil/validate

[![Bundle size](https://img.shields.io/bundlephobia/min/@jetvil/validate/latest?label=Bundle%20Size&style=for-the-badge)](https://bundlephobia.com/package/@jetvil/validate@latest)
[![Version](https://img.shields.io/npm/v/@jetvil/validate?style=for-the-badge&color=cb3837&logo=npm)](https://www.npmjs.com/package/@jetvil/validate)&nbsp;
![Downloads](https://img.shields.io/npm/dt/@jetvil/validate?style=for-the-badge)&nbsp;
[![GitHub](https://img.shields.io/github/license/jetvil/validate?style=for-the-badge)](https://github.com/jetvil/validate/blob/main/LICENSE)&nbsp;
[![GitHub Repo stars](https://img.shields.io/github/stars/jetvil/validate?color=E9E9E9&logo=Github&style=for-the-badge)](https://www.github.com/jetvil/validate)&nbsp;
[![GitHub issues](https://img.shields.io/github/issues-raw/jetvil/validate?label=issues&style=for-the-badge)](https://github.com/jetvil/validate/issues)&nbsp;

✅ Core features of the `@jetvil` packages.

# Features

- 🚀 **Easy to use**: Easy to install in your project.
- ✅ **ES6+ && TS**: TypeScript and ES6+ support(JS).
- 🐭 **Small footprint**: With less then 20kb, you won't even notice.
- 📦 **No dependencies**: You don't depend on anything else.
- 💵 **Free**: It's free and always will be, the beauty of open source.

# Getting Started

## Installation

To use this package, **install** using `npm`, `yarn` or `pnpm`📥:

```bash
# npm
npm install @jetvil/validate
# yarn
yarn add @jetvil/validate
# pnpm
pnpm install @jetvil/validate
```

## Usage

```js
// ES6+ JavaScript CommonsJs
const validate = require("@jetvil/validate");
// TypeScript || ES6+ JavaScript module
import * as validate from "@jetvil/validate";
```

### Documentation

- [VListener](#vlistener)
  - [parameters](#parameters)
  - [options](#options-vlistener)
- [matchers](#matchers)
  - [isValidEmail](#isvalidemail)
  - [isValidPassword](#isvalidpassword)
    - [options](#options-isvalidpassword)
  - [isValidUrl](#isvalidurl)
  - [isUnique](#isunique)
  - [isSoftMatch](#issoftmatch)
  - [isDeepMatch](#isdeepmatch)

### VListener

Adding validation over your array before your program starts.

#### **parameters**:

- `array`: array to validate.
- `callback`: function to validate each item in the array, must return boolean, can only take 1 parameter(for now!).  
 Use @jetvil/validate `isTruthy` or `isFalsy` for example.
<!-- To Be Extended with callback array to validate even more! -->

#### **options-vlistener**:

- `strict`: `boolean`, default: `false`, strict mode; skip push if validation fails in 1 or more items.
- `condition`: `boolean`, default: `true`, condition to validate; check if callback with `condition` is met.

#### **functions**:

- `register`: register listeners for given arrays.
- `remove`: remove listeners for given arrays.

```js
const { VListener } = require("@jetvil/validate");

const vListener = new VListener();
const arr1 = [];
const arr2 = [{ a: 1 }, { b: 2 }]; // items already added will not be validated.

const customCallback = (value) => {
  ...
}; // create your own callback

const customCallbackWithDefaultValues = (value, checkOnThis = {id:1}) => {
  ...
}; // create your own callback with default values to check against

vListener.register([
  { array: arr1, callback: isTruthy, options: { strict: true } },
  { array: arr2, callback: isFalsyExtended, options: { condition: false } },
  { array: arr2, callback: [isTruthy, customCallback, customCallbackWithDefaultValues], options: { strict: true } },
]);

arr1.push(1); // should be pushed, [1]
arr2.push({ c: 3 }, { d: 4 }, null); // should not be pushed, strict mode is on, [{ a: 1 }, { b: 2 }]
arr1.push(0); // should not be pushed, [1]
arr2.push({}); // should not be pushed, [{ a: 1 }, { b: 2 }]

vListener.remove([{ array: arr1 }]);
arr1.push(0); // can now be pushed again, [1, 0]

vListener.remove([{ array: arr2 }]);
arr2.push({}); // can now be pushed again, [{ a: 1 }, { b: 2 }, {}]
```

### Matchers

Matchers are functions that check if a value matches a certain pattern or value.

#### **isUnique**:

Check if value is unique in array or object(key mostly).

```js
const { isUnique } = require("@jetvil/validate");
isUnique([1, 2, 3, 4], 1); // false
isUnique([1, 2, 3, 4], { a: 1 }); // throws error
isUnique({ a: 1, b: 2 }, { a: 1 }); // false
isUnique({ a: 1, b: 2 }, { c: 1 }); // true
```

#### **isSoftMatch**:

Check if two values soft match with each other.

```js
const { isSoftMatch } = require("@jetvil/validate");
isSoftMatch("string", "STRING"); // true
isSoftMatch("string", "abc"); // false
isSoftMatch(1, 1.0); // true
```

#### **isDeepMatch**:

Check if two values deep match with each other.

```js
const { isDeepMatch } = require("@jetvil/validate");
isDeepMatch({ a: 1 }, { a: 1 }); // true
isDeepMatch({ a: 1 }, { a: 2 }); // false
isDeepMatch([1, 2, { a: 3 }], [1, 2, { a: 3 }]); // true

// without the await it will return a promise holding the boolean.
await isDeepMatch(Promise.resolve(1), Promise.resolve(1)); // true
```

#### **isValidEmail**:

Check if value is a valid email.

```js
const { isValidEmail } = require("@jetvil/validate");
isValidEmail("test@test.test"); // true
isValidEmail(""); // false
isValidEmail("test@test"); // false
isValidEmail("test@test.t"); // false
isValidEmail(123); // throws error
```

#### **isValidPassword**:

Check if value is a valid password.

##### **options-isvalidpassword**:

- `minLength`: number, default `8`
- `maxLength`: number, default `32`
- `numbers`: number, default `1`
- `specialChars`: number, default `1`

```js
const { isValidPassword } = require("@jetvil/validate");
isValidPassword("test"); // false
isValidPassword("te!1", { minLength: 4 }); // true
isValidPassword(123); // throws error
isValidPassword("t", { minLength: 1, numbers: 0, specialChars: 0 }); // true
```

#### **isValidUrl**:

Check if value is a valid url.

> IPV4 and IPV6 are not supported.

```js
const { isValidUrl } = require("@jetvil/validate");
isValidUrl("https://google.com"); // true
isValidUrl("google.com"); // true
isValidUrl("google"); // false
isValidUrl(123); // throws error
```

#### **isValidDate**:

Check if value is a valid date.
This function takes any input and will parse it to a Date, `-1` and `1` will work because they are valid dates.

```js
const { isValidDate } = require("@jetvil/validate");
isValidDate(new Date()); // true
isValidDate("1"); //true: because it will be parsed to a date starting from 1970.
isValidDate(-1); //true : because it will be parsed to a date starting from 1970, moving backwards.
isValidDate("string"); // false
```

# Contributing

Found a bug🦟? or want to suggest a new feature🆕? or just want to help🆘? </br>
Feel free to open an issue or a pull request.

Contributions are always welcome!🎉

- Fork the project [here](https://github.com/jetvil/validate/fork).
- Create a new branch like this: `git checkout -b feature/featureName`.
- Commit your changes to your branch: `git commit -m 'Create AwesomeFeature'`⚙️.
- Push your branch: `git push origin feature/featureName`.
- Open a pull request on the `dev` branch [here](https://github.com/jetvil/validate/pulls)🔃.

📒**Note:** Make sure to add tests for your changes ✅.
