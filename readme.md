# Cashify 💸

> Lightweight currency conversion library, successor of money.js

[![Build Status](https://travis-ci.org/xxczaki/cashify.svg?branch=master)](https://travis-ci.org/xxczaki/cashify)
[![Coverage Status](https://coveralls.io/repos/github/xxczaki/cashify/badge.svg?branch=master)](https://coveralls.io/github/xxczaki/cashify?branch=master)
[![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo)
[![install size](https://packagephobia.now.sh/badge?p=cashify)](https://packagephobia.now.sh/result?p=cashify)
![minified size](https://img.shields.io/bundlephobia/min/cashify)

This package was created, because the popular [money.js](http://openexchangerates.github.io/money.js/) library:
* is not maintained (last commit was ~5 years ago)
* has over 20 open issues
* does not support TypeScript
* has implicit globals
* does not have any unit tests
* [has floating point issues](#floating-point-issues)

## Highlights

- Simple API
- 0 dependencies
- Actively maintained
- Well tested
- [Easy migration from money.js](#migrating-from-moneyjs)
- Written in TypeScript

## Install

```
$ npm install cashify
```

## Usage

```js
const {Cashify} = require('cashify');

const rates = {
	GBP: 0.92,
	EUR: 1.00,
	USD: 1.12
};

const cashify = new Cashify({base: 'EUR', rates});

const result = cashify.convert(10, {from: 'EUR', to: 'GBP'});

console.log(result); //=> 9.2
```

Using the `Cashify` constructor is not required. Instead, you can use the `convert` function:

```js
const {convert} = require('cashify');

const rates = {
	GBP: 0.92,
	EUR: 1.00,
	USD: 1.12
};

const result = convert(10, {from: 'EUR', to: 'GBP', base: 'EUR', rates});

console.log(result); //=> 9.2
```

## API

### Cashify({base, rates})

Constructor

##### base

Type: `string`

Base currency

##### rates

Type: `object`

Object containing currency rates (for example from an API, such as Open Exchange Rates)

### convert(amount, {from, to}) *`with constructor`*

Returns conversion result (`number`)

##### amount

Type: `number`

Amount of money you want to convert

##### from

Type: `string`

Currency from which you want to convert

##### to

Type: `string`

Currency to which you want to convert

### convert(amount, {from, to, base, rates}) *`without constructor`*

Returns conversion result (`number`)

##### amount

Type: `number`

Amount of money you want to convert

##### from

Type: `string`

Currency from which you want to convert

##### to

Type: `string`

Currency to which you want to convert

##### base

Type: `string`

Base currency

##### rates

Type: `object`

Object containing currency rates (for example from an API, such as Open Exchange Rates)

## Migrating from [money.js](http://openexchangerates.github.io/money.js/)

With `Cashify` constructor:

```diff
- const fx = require('money');
+ const {Cashify} = require('cashify');

- fx.base = 'EUR';
- fx.rates = {
-	GBP: 0.92,
-	EUR: 1.00,
-	USD: 1.12
- };

+ const rates = {
+	 GBP: 0.92,
+	 EUR: 1.00,
+	 USD: 1.12
+ };

+ const cashify = new Cashify({base: 'EUR', rates});

- fx.convert(10, {from: 'GBP', to: 'EUR'});
+ cashify.convert(10, {from: 'GBP', to: 'EUR'});
```

With `convert` function:

```diff
- const fx = require('money');
+ const {convert} = require('cashify');

- fx.base = 'EUR';
- fx.rates = {
-	GBP: 0.92,
-	EUR: 1.00,
-	USD: 1.12
- };

+ const rates = {
+	 GBP: 0.92,
+	 EUR: 1.00,
+	 USD: 1.12
+ };

- fx.convert(10, {from: 'GBP', to: 'EUR'});
+ convert(10, {from: 'GBP', to: 'EUR', base: 'EUR', rates});
```

## Floating point issues

When working with currencies, decimals only need to be precise up to the smallest cent value while avoiding common floating point errors when performing basic arithmetic.

Let's take a look at the following example:

```js
const fx = require('money');
const {Cashify} = require('cashify');

const rates = {
	GBP: 0.92,
	USD: 1.12
};

fx.rates = rates;
fx.base = 'EUR';

const cashify = new Cashify({base: 'EUR', rates});

fx.convert(10, {from: 'EUR', to: 'GBP'}); //=> 9.200000000000001
cashify.convert(10, {from: 'EUR', to: 'GBP'}); //=> 9.2
```

As you can see, money.js doesn't handle currencies correctly and therefore a floating point issues are occuring. Even though there's just a minor discrepancy between the results, if you're converting large amounts, that can add up.

Cashify solves this problem the same way as [currency.js](https://github.com/scurker/currency.js/) - by working with integers behind the scenes. This should be okay for most reasonable values of currencies.

## Related projects

If you want to handle stuff, like currency formatting, I recommend checking the following projects (they work great with cashify :unicorn:):

* [currency.js](https://github.com/scurker/currency.js/)

## License

MIT © [Antoni Kepinski](https://kepinski.me)
