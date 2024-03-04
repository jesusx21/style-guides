# Javascript Style Guide

This style guide document is based on the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript#airbnb-javascript-style-guide-)(with some minor changes), and it's the style guide I prefer to use, having applied it in the companies I've worked for.

## Table of Content

  1. [**References**](#references)
  1. [**Object**](#object)

## References

  #### Prefer Const:
  - Use `const` for all of your references; avoid using `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign)
    > Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

    ```js
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    ```

  #### Disallow Var:
  - If you must reassign references, use `let` instead of `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var)
    > Why? let is block-scoped rather than function-scoped like var.

    ```js
    // bad
    var count = 1;
    if (true) {
      count +=1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count +=1;
    }
    ```

  #### Block Scope
  - Note that both `let` and `const` are block-scoped, whereas `var` is function-scoped.
    ```js
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
      var c = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    console.log(c); // Prints 1
    ```

    In the above code, you can see that referencing `a` and `b` will produce a ReferenceError, while `c` contains the number. This is because `a` and `b` are block scoped, while `c` is scoped to the containing function.

[`Back to Top`](#table-of-content)

## Object

  #### Object Literal Syntax
  - Use the literal syntax for object creation. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object)
    ```js
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  #### Computed Property Names
  - Use computed property names when creating objects with dynamic property names.
    > Why? They allow you to define all the properties of an object in one place.

    ```js
    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true
    };
    ```

  #### Object Method Shorthand
  - Use object method shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)
    ```js
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      }
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      }
    };
    ```

  #### Object Property Shorthand
  - Use property value shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)
    > Why? It is shorter and descriptive.

    ```js
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker
    };

    // good
    const obj = {
      lukeSkywalker
    };
    ```

  #### Group Shorthand
  - Group your shorthand properties at the beginning of your object declaration.
    > Why? It’s easier to tell which properties are using the shorthand.

    ```js
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4
    };
    ```

  #### Quote Properties
  - Only quote properties that are invalid identifiers. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props)
    > Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

    ```js
    // bad
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5
    };

    // good
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5
    };
    ```

  #### Order Keys
  - Order the keys of the object. There can be two types of ordering, alphabetically or by priority.
    ```js
    // bad
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeThree: 3,
      twoJediWalkIntoACantina: 2,
      mayTheFourth: 4,
      episodeOne: 1
    };

    // good
    const obj = {
      anakinSkywalker,
      lukeSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4
    };
    ```

    See how in the code above the first two items does not seem to have a priority, that is to say, by priority there is no difference if `anakinSkywalker` is before or after `lukeSkywalker` so in this case we order those two fields alphabetically. But the rest of the keys mark an obvious priority, so instead of order those keys alphabetically we order them by priority.

  #### Spread Syntax
  - Prefer the object spread syntax over [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) to shallow-copy objects. Use the object rest parameter syntax to get a new object with certain properties omitted. eslint: [`prefer-object-spread`](https://eslint.org/docs/rules/prefer-object-spread)
    ```js
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
    delete copy.a; // so does this

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

  #### Reassing Property
  - Prefer reassigning a property to null or undefined instead of deleting the property from an object
    > Why? In modern JavaScript engines, changing the number of properties on an object is much slower than reassigning the values. The delete keyword should be avoided except when it is necessary to remove a property from an object's iterated list of keys, or to change the result of `if (key in obj)`.

[`Back to Top`](#table-of-content)
