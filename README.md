In response to the response: no

----

## _.each

> Underscore/Lodash may exit iteration early by explicitly returning `false`.

This is a disadvantage.  You can bail on for loops if you want to be able to bail.  Part of the value of map is to assure the programmer that the predicate passed to the iteration cannot do this even in incorrect behavior.

Comparison performance is nonsensical, since the two functions being compared aren't doing the same work.

----

## _.map

> Native doesn't support the `_.property` iteratee shorthand.

It doesn't need to.  You just wrote a bad vanilla replacement.

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

The sensible Vanilla approach:

```javascript
const arr = users.map(u => u.user);
```

Shorter, clearer, and faster than the underscore approach, with the bonus of being standards defined and validated by the browser vendors' test sets.

----

## _.some

> Native doesn't support the `_.matches` iteratee shorthand.

That's okay.  We have `.filter/1`, which is shorter, clearer, and faster than the underscore approach.

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

The sensible Vanilla implementation:

```javascript
const result = array.filter(i => i.active === true).map(i =>
```

I prefer the sensible implementation for two reasons: one, it's easy as pie to read and understand, and represents the two behaviors distinctly.  Two, it's very easy to modify or extend, and as such seems to me to be more maintainable than the efficient vanilla implementation (though admittedly this second criterion is no advantage over underscore.)

This approach is admittedly less efficient: you have two traversals, one of the full container and one of the filtered container, which is potentially wasteful up to an entire traversal of the original container.  

... to which a great "meh" was heard throughout the force.  Real world JS doesn't work on large datastructures and shouldn't really care about stuff like that.

However, if you do, you can write this efficiently in JS too; indeed significantly more efficiently than in `_`, since it's implemented in-engine native, and is mostly about container allocation and fill, which benefits immensely from native optimizations.

The smart but moderately opaque Vanilla implementation:

```javascript
const result = array.reduce( /* COMEBACK */
```
  
### Performance

Todo

**[⬆ back to top](#quick-links)**


## _.reduce

### Performance

Lodash|Underscore|Native 
--- | --- | ---
8,734|5,481|7,467

**[⬆ back to top](#quick-links)**


## _.reduceRight

Todo

**[⬆ back to top](#quick-links)**


## _.filter

Native doesn't support the `_.matches` iteratee shorthand.

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

### Performance

Todo

**[⬆ back to top](#quick-links)**


## _.find

### Performance

Todo

**[⬆ back to top](#quick-links)**


## _.findIndex

### Performance

Todo

**[⬆ back to top](#quick-links)**


## _.indexOf

### Performance

Todo

**[⬆ back to top](#quick-links)**


## _.lastIndexOf

### Performance

Todo

**[⬆ back to top](#quick-links)**


## _.isNaN

Native coerces the argument to a `Number`.

  ```js
  // Underscore/Lodash
  console.log(_.isNaN("blabla"));
  // output: false

  // Native
  // Confusing special-case behavior
  console.log(isNaN("blabla"));
  // output: true
  // surprise!
  
  // ES6
  console.log(Number.isNaN("blabla"));
  // output: false
  // but browser support maybe the problem
  ```

**[⬆ back to top](#quick-links)**


## References

* [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
* [Underscore.js](http://underscorejs.org)
* [Lodash.js](https://lodash.com/docs)
* [jsPerf](https://jsperf.com)


# License

MIT
