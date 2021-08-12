## Immutability do(s)

### readonly object

This rule enforces to treat object as `readonly`.

You might think that using `const` would eliminate mutation from your JavaScript/TypeScript code. **Wrong.** Turns out that there's a pretty big loophole in `const`.

```typescript
interface Point {
  x: number;
  y: number;
}
const point: Point = { x: 23, y: 44 };
point.x = 99; // This is legal
```

We can proceed an effective approach using Object.freeze() to prevent mutations in your Redux reducers or any store reducers. Or this one:

```typescript
interface Point {
   x: number;
   y: number;
}
const point: Point = { x: 23, y: 44 };
const transformedPoint = { ...point, x: 99 };
```

### readonly array

This rule enforces to treat object as `readonly`.

Even if an array is declared with `const` it is still possible to mutate the contents of the array.

```typescript
interface Point {
   x: number;
   y: number;
}
const points: Array<Point> = [{ x: 23, y: 44 }];
points.push({ x: 1, y: 2 }); // This is legal
```

We can proceed with this approach:

```typescript
const addNewInsideArray = (arr, newData) => {
    const newArray = [].concat(arr);
    newArray.push(newData);
    return newArray;
}

interface Point {
   x: number;
   y: number;
}

const points: Array<Point> = [{ x: 23, y: 44 }];
const newPoints = addNewInsideArray(points, { x: 1, y: 2 });
```

### no-let

There's no reason to use `let` in a Redux/React or anyStateManagement/React application, because all your state is managed by either Redux or React. Use `const` instead, and avoid state bugs altogether.

```typescript
let x = 5; // <- Unexpected let or var, use const.
```

What about `for` loops? Loops can be replaced with the Array methods like `map`, `filter`, and so on. If you find the built-in JS Array methods lacking, use [ramda](http://ramdajs.com/), or [lodash-fp](https://github.com/lodash/lodash/wiki/FP-Guide).

```typescript
const SearchResults = ({ results }) => (
  <ul>
    {results.map(result => (
      <li>result</li>
    )) // <- Who needs let?
    }
  </ul>
);
```

### no-array-mutation


This rule prohibits mutating an array via assignment to or deletion of their elements/properties. This rule enforces array immutability. (as apposed to [readonly-array](#readonly-array)).

```typescript
const x = [0, 1, 2];

x[0] = 4; // <- Mutating an array is not allowed.
x.length = 1; // <- Mutating an array is not allowed.
x.push(3); // <- Mutating an array is not allowed.
```

### no-object-mutation

This rule prohibits syntax that mutates existing objects via assignment to or deletion of their properties. Forbidding forms like `a.b = 'c'` is one way to plug this hole.

```typescript
const x = { a: 1 };

x.foo = "bar"; // <- Modifying properties of existing object not allowed.
x.a += 1; // <- Modifying properties of existing object not allowed.
delete x.a; // <- Modifying properties of existing object not allowed.
Object.assign(x, { b: 2 }); // <- Modifying properties of existing object not allowed.
```

### no-delete

The delete operator allows for mutating objects by deleting keys. This rule disallows any delete expressions.

```typescript
delete object.property; // Unexpected delete, objects should be considered immutable.
```

As an alternative the spread operator can be used to delete a key in an object (as noted [here](https://stackoverflow.com/a/35676025/2761797)):

```typescript
const { [action.id]: deletedItem, ...rest } = state;
```
