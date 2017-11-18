# Flatry 

[![Build Status](https://travis-ci.org/ymatuhin/flatry.svg?branch=master)](https://travis-ci.org/ymatuhin/flatry)
[![codecov](https://codecov.io/gh/ymatuhin/flatry/branch/master/graph/badge.svg)](https://codecov.io/gh/ymatuhin/flatry)
[![GitHub license](https://img.shields.io/github/license/ymatuhin/flatry.svg)](https://github.com/ymatuhin/flatry/blob/master/LICENSE)

Flatry (flat try) converting promise or function to flat array response with `[err, result]`.

Inspired by golang style error handling without try/catch.

&nbsp;

## Install

```bash
npm install flatry
# or
yarn add flatry
```

&nbsp;

## Use

```js
import flatry from 'flatry';
// or
const flatry = require('flatry');
```

&nbsp;

## Examples

### Asynchronous (async/await)

Before:
```js
async asyncData({ app, error }) {
  try {
    const categories = (await app.$axios.$get('forum')).sections;
    return { categories };
  } catch (err) {
    return error({ statusCode: err.statusCode });
  }
}
```

After:
```js
async asyncData({ app, error }) {
  const [err, res] = await flatry(app.$axios.$get('forum'));
  if (err) return error({ statusCode: err.statusCode });
  return { categories: res.sections };
}
```


### Synchronous

Before:
```js
function throwErrorIn10percent() {
  if (Math.random() > 0.9) throw new Error('Random error');
  return true;
}

let result = false;
try {
    result = throwErrorIn10percent()
} catch (error) {
    console.log('Error catched', error)
}
console.log('result', result);
```

After:
```js
function throwErrorIn10percent() {
  if (Math.random() > 0.9) throw new Error('Random error');
  return true;
}

const [err, result] = flatry(throwErrorIn10percent);
if (err) console.log('Error catched', err)
console.log('result', result);
```
