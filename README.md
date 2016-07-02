In response to [the response](https://github.com/stevemao/You-Dont-Know-Lodash-Underscore): no

I respectfully disagree with all of these rebuttals as presented.  Sorry `:(`

----

## _.each

> Underscore/Lodash may exit iteration early by explicitly returning `false`.

This is a disadvantage.  You can bail on for loops if you want to be able to bail.  Part of the value of map is to assure the programmer that the predicate passed to the iteration cannot do this even in incorrect behavior.

Much of the purpose of the functional calls (`.map`, `.reduce`, `.filter`, and so on) is in distinguishing the behavior of the application of a functor from the quality of the functor.  You should be able to have a function that identifies whether a thing is a duck, and then filter on duck-as-a-quality.

There are important, subtle advantages to this.  For example, `.map` is inherently parallelizable - though underscore's abortable version is not.  In Mozilla Servo, which is written in a lightweight process oriented language, this is a significant performance lose.  Given the nature of parallelism, and that Microsoft Edge is moving in the same direction, and that this is becoming a common language thing (used to be esoteric to languages like erlang and mozart-oz and gambit scheme; now common in things like c# and google go.)

I believe that this will be the norm relatively soon.

Comparison performance as provided is nonsensical, since the two functions being compared aren't doing the same work.  One only applies the functor to the first item; the other to all.

----

## _.map

> Native doesn't support the `_.property` iteratee shorthand.

It doesn't need to.  

```javascript
const arr = users.map(u => u.user);
```

Shorter, clearer, and faster than the underscore approach, with the bonus of being standards defined and validated by the browser vendors' test sets.

Your code:

  ```js
  // Underscore/Lodash
  var users = [
    { 'user': 'barney' },
    { 'user': 'fred' }
  ];
  var arr = _.map(users, 'user');
  console.log(arr);
  // output: ['barney', 'fred']

  // Native
  var users = [
    { 'user': 'barney' },
    { 'user': 'fred' }
  ];
  var arr = users.map('user');
  // error!
  ```

----

## _.some

> Native doesn't support the `_.matches` iteratee shorthand.

Doesn't need it.

```javascript
peeps.some(p => p.user === 'barney' && p.active === true);
```

Native, faster, more easily optimized, better defined.

Your code:

  ```js
  // Underscore/Lodash
  var users = [
    { 'user': 'barney', 'active': true },
    { 'user': 'fred',   'active': false }
  ];
  
  // using the `_.matches` iteratee shorthand
  _.some(users, { 'user': 'barney', 'active': false });
  // output: false

  // Native
  var users = [
    { 'user': 'barney', 'active': true },
    { 'user': 'fred',   'active': false }
  ];
  var result = array.some({ 'user': 'barney', 'active': false }); // error!
  ```


## _.reduce

> (no comment made by rebuttal document; js is ~12% faster)

I would point out that JS `.reduce` is more strictly defined than `_.reduce`.

## _.reduceRight

> Todo

Should be the same thing

## _.filter

> Native doesn't support the `_.matches` iteratee shorthand.

Doesn't need to.

```javascript
users.filter(u => u.age === 36 && u.active === 'true');
```

Because this can logical shortcut on age, which should have very low cardinality, in most cases the string comparison will never come to pass; there will also be no attempt to object identity match, and this relies on more easily optimized built-ins.  

Almost certainly faster, much better defined, and easy to read for anyone who knows the language, whether or not they know a utility library.

Your code:

  ```js
  var users = [
    { 'user': 'barney', 'age': 36, 'active': true },
    { 'user': 'fred',   'age': 40, 'active': false }
  ];
  // using the `_.matches` iteratee shorthand
  _.filter(users, { 'age': 36, 'active': true });
  // output: objects for ['barney']

  // Native
  var users = [
    { 'user': 'barney', 'age': 36, 'active': true },
    { 'user': 'fred',   'age': 40, 'active': false }
  ];
  var filtered = users.filter({ 'age': 36, 'active': true }); // error!
  ```

## _.find

> No comment given in rebuttal document

## _.findIndex

> No comment given in rebuttal document

## _.indexOf

> No comment given in rebuttal document

## _.lastIndexOf

> No comment given in rebuttal document

## _.isNaN

> Native coerces the argument to a `Number`.

That's because you're using `isNaN/1` instead of [`Number.isNaN/1`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN).  `isNaN/1` is a compatibility fallback for jscript from the 1990s.

Two of the four references you gave point this out at the very top of the page.  

It's worth noting that as the language progresses, `Number.isNaN/1` will always have correct behavior for the types available for an individual browser, which is not the case for a library with a local install that may fall out of date.  As JavaScript progresses into having native type support (such as u32/s32 for asm.js,) this seems like an unimportant niche correctness concern.

More importantly, the builtins will be more robust, better tested, and much faster.

```js
// es3
console.log(Number.isNaN("blabla"));
// output: false
```

Your code:

  ```js
  // Underscore/Lodash
  console.log(_.isNaN("blabla"));
  // output: false

  // Native [ed: this is not actually javascript, but rather inherited gross jscript]
  // Confusing special-case behavior
  console.log(isNaN("blabla"));
  // output: true
  // surprise!
  
  // ES6
  console.log(Number.isNaN("blabla"));
  // output: false
  // but browser support maybe the problem
  ```

# License

> MIT

Good call.  So's mine.

MIT
