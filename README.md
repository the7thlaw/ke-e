[![Build Status](https://travis-ci.org/hychen/hycheck.svg?branch=master)](https://travis-ci.org/hychen/hycheck)
[![Coverage STatus](https://coveralls.io/repos/github/hychen/hycheck/badge.svg?branch=master)](https://coveralls.io/github/hychen/hycheck?branch=master)
[![License](http://img.shields.io/:license-mit-blue.svg)](http://badges.mit-license.org/)

HyCheck is a property-based testing library, inspired by QuickCheck, Hypothesis, JSVerify.

- [Documents](http://hychen.me/hycheck/index.html).

## What is property-based testing?

Property-based tests make statements about the output of your code based on
the input, and these statements are verified for many different possible inputs.

According [this article](https://fsharpforfunandprofit.com/posts/property-based-testing-2/),
Categories for properties are:

- Different paths, same destination
- There and back again
- Some things never change
- The more things change, the more they stay the same
- Solve a smaller problem first
- Hard to prove, easy to verify
- The test oracle

## Getting Started

### Node.js

install the module with: npm install hycheck

```javascript
import hc from `hycheck`

describe('Integer', () => {
  hc.hold(
    'x + y = y + x'
    (x, y) => x + y === y + x
    ).over(hc.int, hc.int);
});
```

### Browser using script tag

Download hycheck.js and place it in your project, then add it as the follwoing.

```html
<script src="dist/hycheck.js"></script>
<script>
describe('Integer', () => {
  hc.hold(
    'x + y = y + x'
    (x, y) => x + y === y + x
    ).over(hc.int, hc.int);
});
</script>
```

## Usage

By default, `hc.hold` runs 100 tests. it reports a counter example does not satisfy the property 
if any test fail.

for example, the following test.

```
describe('Int', () => {
  hc.hold(
    '> 0',       // property name.
    x => x > 0   // predicate.
  )
  .over(0)       // first especially case.
  .over(2)       // second especially case.
  .over(hc.int)  // universal case.
})
```

will report:

```
1) Int > 0:

      AssertionError: > 0 doesn't hold, counter example: -5, tried: 1/100
      + expected - actual

      -false
      +true
```

### Exception-based Testing

You can still use `chai` to do the testing with random test values 
generated by `hycheck`.

```
describe('Nat', () => {
  it('>= 0',  () => {
    hc.forall(hc.nat).eval((n) => {
      expect(n >= 0).eq(true);
    });
  }
})
```

### Repeatable

test results are repeatable.

```
$ mocha
1) Int >0:
AssertionError: >0 doesn't hold, seed: -1764850555, counter example: -5265798245849472, tried: 3/3`

```

```
HCSeed=-1764850555 mocha
1) Int >0:
AssertionError: >0 doesn't hold, seed: -1764850555, counter example: -5265798245849472, tried: 3/3`
```

### License

The MIT License (MIT)
