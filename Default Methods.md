# Default Methods

### DataStructure

## **Set**

- **Resource**
    
    [JavaScript Data Structures - Sets](https://youtu.be/bFZjTIQIneY)
    

Set is a collection of unique elements, In sets there will not be any duplicate elements

In JavaScript, a Set is a built-in data structure that represents a collection of unique values. It is similar to an array, but with no duplicate values allowed.

Here's an example of how to use a Set in JavaScript

```jsx
const mySet = new Set();

// Add elements to the set
// We can add with multiple add in same line as well
mySet.add('apple');
mySet.add('banana').add('orange');
mySet.add('apple'); // this will not add a duplicate

// Check the size of the set
console.log(mySet.size); // Output: 3
// Print the set array
console.log(mySet); // ['apple','banana','orange']

// Check if a value is in the set
console.log(mySet.has('apple')); // Output: true
console.log(mySet.has('grape')); // Output: false

console.log(mySet.entries()); // {[ 'apple', 'apple' ],[ 'banana', 'banana' ],[ 'orange', 'orange' ]}
console.log(mySet.keys()); // { 'apple', 'banana', 'orange' }
console.log(mySet.values()); // { 'apple', 'banana', 'orange' }

// Iterate over the set
mySet.forEach((value) => {
  console.log(value);
});

// delete an element from the set
mySet.delete('banana');

const set2 = new Set([1,2,"apple"]);
const union = new Set([...mySet, ...set2]); // { 'apple', 'orange', 1, 2 }
console.log(union); // { 'apple', 'orange', 1, 2 }

// clear all elements from the set
mySet.clear();
```

In this example, we create a new Set using the `new Set()` syntax. We then add four elements to the set using the `add()` method. Notice that we add "apple" twice, but the Set only stores one instance of the value because it doesn't allow duplicates.

We then use the `size` property to check the number of elements in the Set, and the `has()` method to check whether a specific value is in the Set.

To iterate over the Set, we use the `forEach()` method, which takes a callback function that is called for each element in the Set.

We also demonstrate how to remove an element from the Set using the `delete()` method, and how to clear all elements from the Set using the `clear()` method.

## **Map**

The `Map` object in JavaScript is a collection of key-value pairs where both keys and values can be of any type. Unlike objects, Maps maintain insertion order and allow any value (including objects) as keys.

**Core Methods**

1. **Creating a Map**
    
    ```jsx
    // Empty Map
    const map1 = new Map();
    
    // Initialize with entries
    const map2 = new Map([
      ['key1', 'value1'],
      ['key2', 'value2']
    ]);
    ```
    
2. **set(key, value)**
    
    Adds or updates an entry in the map.
    
    ```jsx
    const userMap = new Map();
    userMap.set('name', 'John'); // Map(1) {"name" => "John"}
    userMap.set(42, 'answer'); // Map(2) {"name" => "John", 42 => "answer"}
    ```
    
3. **get(key)**
    
    Returns the value associated with the key, or undefined if the key doesn't exist.
    
    ```jsx
    userMap.get('name'); // "John"
    userMap.get('age'); // undefined
    ```
    
4. **has(key)**
    
    Returns a boolean indicating whether the key exists in the map.
    
    ```jsx
    userMap.has('name'); // true
    userMap.has('age'); // false
    ```
    
5. **delete(key)**
    
    Removes the specified entry and returns true if the entry existed.
    
    ```jsx
    userMap.delete('name'); // true
    userMap.delete('age'); // false (key didn't exist)
    ```
    

6. clear()

Removes all entries from the map.

```jsx
userMap.clear(); // Map(0) {}
```

1. size
    
    Property (not a method) that returns the number of entries.
    
    ```jsx
    const fruitMap = new Map([
      ['apple', 5],
      ['banana', 10]
    ]);
    fruitMap.size; // 2
    ```
    
2. keys()
    
    Returns an iterator of all keys in insertion order.
    
    ```jsx
    const keyIterator = fruitMap.keys();
    Array.from(keyIterator); // ["apple", "banana"]
    ```
    
3. values()
    
    Returns an iterator of all values in insertion order.
    
    ```jsx
    const valueIterator = fruitMap.values();
    Array.from(valueIterator); // [5, 10]
    ```
    
4. entries()
    
    Returns an iterator of [key, value] pairs in insertion order.
    
    ```jsx
    const entryIterator = fruitMap.entries();
    Array.from(entryIterator); // [["apple", 5], ["banana", 10]]
    ```
    
5. forEach(callbackFn)
    
    Executes a function for each key-value pair.
    
    ```jsx
    fruitMap.forEach((value, key) => {
      console.log(`${key}: ${value}`);
    });
    // Output:
    // apple: 5
    // banana: 10
    ```
    

## Map vs. Object

| **Feature** | **Map** | **Object** |
| --- | --- | --- |
| **Key Types** | Any value | Strings, Symbols |
| **Size** | Available via `size` property | Requires manual calculation |
| **Iteration** | Iterates in insertion order | No guaranteed order |
| **Performance** | Better for frequent additions/removals | Better for simple scenarios |

## Common Use Cases

### Caching with Complex Keys

```jsx
javascript
Copy
const requestCache = new Map();

function fetchData(endpoint, params) {
  const key = JSON.stringify({ endpoint, params });
  if (requestCache.has(key)) {
    return requestCache.get(key);
  }

  const data = performActualFetch(endpoint, params);
  requestCache.set(key, data);
  return data;
}

```

### Frequency Counter

```jsx
javascript
Copy
function countCharacters(str) {
  const charMap = new Map();

  for (const char of str) {
    charMap.set(char, (charMap.get(char) || 0) + 1);
  }

  return charMap;
}

countCharacters("hello"); // Map(4) {"h" => 1, "e" => 1, "l" => 2, "o" => 1}

```

### Object as Keys

```jsx
javascript
Copy
const userRoles = new Map();
const user1 = { id: 1, name: "Alice" };
const user2 = { id: 2, name: "Bob" };

userRoles.set(user1, "admin");
userRoles.set(user2, "editor");

userRoles.get(user1); // "admin"

```

### Converting Between Maps and Arrays/Objects

```jsx
javascript
Copy
// Object to Map
const obj = { name: "John", age: 30 };
const map = new Map(Object.entries(obj));

// Map to Object
const objFromMap = Object.fromEntries(map.entries());

// Map to Array
const array = Array.from(map); // [["name", "John"], ["age", 30]]

```

Maps are powerful data structures for many modern JavaScript applications, especially when you need to maintain key order, use non-string keys, or frequently add and remove properties.

## **Array**

- **Resource**
    
    [JavaScript Data Structures - Arrays](https://youtu.be/bzknBUbLYXs)
    

Array is a simple container in memory, that allows us to group elements (or item) together

In JavaScript, an array is a built-in data structure that represents a collection of values, similar to a list. Arrays are ordered collections, meaning that each value in the array has an index that indicates its position in the array.

Note that arrays in JavaScript can hold values of any data type, including numbers, strings, objects, and even other arrays. Additionally, arrays in JavaScript are dynamically sized, meaning that their length can change during runtime.

1. `push()`: Adds one or more elements to the end of an array and returns the new length of the array.

```jsx
const myArray = [1, 2, 3];
myArray.push(4, 5);
console.log(myArray); // Output: [1, 2, 3, 4, 5]
```

1. `pop()`: Removes the last element from an array and returns that element.

```jsx
const myArray = [1, 2, 3];
const lastElement = myArray.pop();
console.log(lastElement); // Output: 3
console.log(myArray); // Output: [1, 2]
```

1. `shift()`: Removes the first element from an array and returns that element.

```jsx
const myArray = [1, 2, 3];
const firstElement = myArray.shift();
console.log(firstElement); // Output: 1
console.log(myArray); // Output: [2, 3]
```

1. `unshift()`: Adds one or more elements to the beginning of an array and returns the new length of the array.

```jsx
const myArray = [1, 2, 3];
myArray.unshift(0);
console.log(myArray); // Output: [0, 1, 2, 3]
```

1. `concat()`: Joins two or more arrays and returns a new array that contains all the elements of the original arrays.

```jsx
const myArray1 = [1, 2];
const myArray2 = [3, 4];
const concatenatedArray = myArray1.concat(myArray2);
console.log(concatenatedArray); // Output: [1, 2, 3, 4]
```

1. `slice()`: Returns a new array that contains a portion of the original array.

```jsx
const myArray = [1, 2, 3, 4, 5];
const slicedArray = myArray.slice(1, 4);
console.log(slicedArray); // Output: [2, 3, 4]
```

1. `splice()`: Modify the original array that contains a portion of the original array.

```jsx
const myArray = [1, 2, 3, 4, 5];
myArray.splice(3, 0, '10');
console.log(myArray); 
// This is how our original array looks now: [ 1, 2, 3, '10', 4, 5 ]
// If we want to remove '10' means
myArray.splice(3, 1); // Output: [ 1, 2, 3, 4, 5 ]
// If you want replace 2nd elemnt 2 with 3 means
myArray.splice(1, 1, 3); // Output: [ 1, 3, 3, 4, 5 ]
```

1. `join():` This method creates and returns a new string by concatenating all of the elements in an array, separated by a specified separator string.

```jsx
const myArray = ['Hello', 'World', '!'];
const result = myArray.join(' ');
console.log(result); // Output: 'Hello World !'
```

1. `indexOf()`: This method returns the index of the first occurrence of a specified value in an array, or -1 if the value is not found.

```jsx
const myArray = ['apple', 'banana', 'orange'];
const index = myArray.indexOf('banana');
console.log(index); // Output: 1
```

1. `lastIndexOf()`: This method returns the index of the last occurrence of a specified value in an array, or -1 if the value is not found.

```jsx
const myArray = ['apple', 'banana', 'orange', 'banana'];
const index = myArray.lastIndexOf('banana');
console.log(index); // Output: 3
```

1. `findIndex()` method returns the index of the first element in the array that satisfies the provided testing function; otherwise, it returns -1.

```jsx
const numbers = [1, 2, 3, 4];
const index = numbers.findIndex((num) => {
  return num > 2;
});

console.log(index); // Output: 2
```

1. `reverse()`: This method reverses the order of the elements in an array.

```jsx
const myArray = ['apple', 'banana', 'orange'];
myArray.reverse();
console.log(myArray); // Output: ['orange', 'banana', 'apple']
```

1. `sort()`: This method sorts the elements in an array in ascending order, or according to a specified sorting function.

```jsx
const myArray = [3, 1, 4, 1, 5, 9];
myArray.sort();
console.log(myArray); // Output: [1, 1, 3, 4, 5, 9]
--------------------AND-------------------
const myArray2 = [3, 1, 4, 1, 11, 5, 9];
myArray2.sort((a, b) => a - b)
console.log(myArray2); // [1, 1, 3, 4, 5, 9, 11]
```

1. `find()`: This method returns the value of the first element in an array that satisfies a provided testing function.

```jsx
const myArray = [1, 2, 3, 4, 5];
const result = myArray.find((element) => element > 3);
console.log(result); // Output: 4
```

1. `filter()`: This method creates a new array with all elements that pass a provided testing function.

```jsx
const myArray = [1, 2, 3, 4, 5];
const result = myArray.filter((element) => element % 2 === 0);
console.log(result); // Output: [2, 4]
```

1. `map()`: This method creates a new array with the results of calling a provided function on every element in the array.

```jsx
const myArray = [1, 2, 3, 4, 5];
const newArray = myArray.map(element => element * 2)
console.log(result); // Output: [2, 4, 6, 8, 10]
```

1. `forEach()`: This method executes a provided function once for each array element.

```jsx
const myArray = [1, 2, 3, 4, 5];

myArray.forEach((element) => {
  console.log(element * 2);
});

// Output 2 4 6 8 10
```

1. `reduce()`: This method applies a function to each element in the array to reduce the array to a single value.

```jsx

const myArray = [1, 2, 3, 4, 5];

const reducedValue = myArray.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 0);

console.log(reducedValue); // 15
```

1. `every()`: This method tests whether all elements in the array pass the test implemented by the provided function.

```jsx
const myArray = [1, 2, 3, 4, 5];

const allGreaterThanZero = myArray.every((element) => {
  return element > 0;
});
console.log(allGreaterThanZero); // Output: true

const allGreaterThanThree = myArray.every((element) => {
  return element > 3;
});
console.log(allGreaterThanThree); // Output: false
```

1. `some()`: This method tests whether at least one element in the array passes the test implemented by the provided function.

```jsx
const myArray = [1, 2, 3, 4, 5];

const someGreaterThanThree = myArray.some((element) => {
  return element > 3;
});
console.log(someGreaterThanThree); // Output: true
```

1. `fill()` method fills all the elements of an array from a start index to an end index with a static value.

```jsx
const numbers = [1, 2, 3, 4];
// The fill() method fills specified elements in an array with a value.
// The fill() method overwrites the original array.
// Start and end position can be specified. If not, all elements will be filled.
// array.fill(value, start, end)
// array.fill(Required, Optional, Optional (defalut end))
numbers.fill(0, 1, 3);

console.log(numbers); // Output: [1, 0, 0, 4]
```

1. `includes()` method determines whether an array includes a certain value among its entries, returning `true` or `false` as appropriate.

```jsx
const numbers = [1, 2, 3, 4];
const hasThree = numbers.includes(3);
const hasTen = numbers.includes(10);

console.log(hasThree); // Output: true
console.log(hasTen); // Output: false
```

1. `flat()` method creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.

```jsx
const numbers = [1, 2, [3, 4], [5, [6, 7]]];

const flattened = numbers.flat(1);
console.log(flattened); // Output: [ 1, 2, 3, 4, 5, [ 6, 7 ] ]

// Infinity means till every Array
const infinityFlayOFArray = numbers.flat(Infinity); 
console.log(infinityFlayOFArray); // Output: [1, 2, 3, 4, 5, 6, 7]
```

1. `reduceRight()` method applies a function against an accumulator and each value of the array (from right-to-left) to reduce it to a single value.

```jsx
const numbers = [1, 2, 3, 4];
const result = numbers.reduceRight((accumulator, currentValue) => {
  return accumulator - currentValue;
});

console.log(result); // Output: -2
```

These are just a few of the many built-in functions available for the Array data structure in JavaScript.

## **String**

1. `charAt()`: This method returns the character at the specified index in a string.
    
    ```jsx
    const str = "hello world";
    console.log(str.charAt(0)); // "h"
    ```
    
2. `concat()`: This method joins two or more strings together.
    
    ```jsx
    const str1 = "hello";
    const str2 = "world";
    console.log(str1.concat(" ", str2)); // "hello world"
    ```
    
3. `includes()`: This method checks whether a string contains another string or not and returns a boolean value.
    
    ```jsx
    const str = "hello world";
    console.log(str.includes("world")); // true
    console.log(str.includes("foo")); // false
    ```
    
4. `indexOf()`: This method returns the index of the first occurrence of a specified substring in a string.
    
    ```jsx
    const str = "hello world";
    console.log(str.indexOf("world")); // 6
    console.log(str.indexOf("foo")); // -1
    ```
    
5. `lastIndexOf()`: This method returns the index of the last occurrence of a specified substring in a string.
    
    ```jsx
    const str = "hello world";
    console.log(str.lastIndexOf("o")); // 7
    console.log(str.lastIndexOf("foo")); // -1
    ```
    
6. `padEnd()`: This method pads a string with a specified character until it reaches a specified length.
    
    ```jsx
    const str = "hello";
    console.log(str.padEnd(10, "*")); // "hello*****"
    ```
    
7. `replace()`: This method replaces a specified substring with another string.
    
    ```jsx
    const str = "hello world";
    console.log(str.replace("world", "foo")); // "hello foo"
    ```
    
8. `slice()`: This method extracts a section of a string and returns a new string.
    
    ```jsx
    const str = "hello world";
    console.log(str.slice(0, 5)); // "hello"
    console.log(str.slice(-5)); // "world"
    ```
    
9. `split()`: This method splits a string into an array of substrings based on a specified separator.
    
    ```jsx
    const str = "hello world";
    console.log(str.split(" ")); // ["hello", "world"]
    ```
    
10. `toLowerCase()`: This method converts a string to lowercase.
    
    ```jsx
    const str = "Hello World";
    console.log(str.toLowerCase()); // "hello world"
    ```
    
11. `toUpperCase()`: This method converts a string to uppercase.
    
    ```jsx
    const str = "Hello World";
    console.log(str.toUpperCase()); // "HELLO WORLD"
    ```
    
12. `substring(startIndex, endIndex)`: This method returns a new substring from a string, starting at the specified start index and ending at the specified end index (exclusive). If the end index is not specified, the method will extract the substring until the end of the string.
    
    ```jsx
    const str = "Hello world";
    const subStr1 = str.substring(1, 4); // "ell"
    const subStr2 = str.substring(3); // "lo world"
    ```
    
13. `trim()`: This method removes whitespace from both ends of a string.
    
    ```jsx
    const str = "   Hello world    ";
    const trimmedStr = str.trim(); // "Hello world"
    ```
    
14. `startsWith(searchString, position)`: This method checks whether a string starts with a specified substring. The `position` parameter is optional, and specifies the position in the string at which to begin searching for the substring.
    
    ```jsx
    const str = "Hello world";
    const startsWithHello = str.startsWith("Hello"); // true
    const startsWithWorld = str.startsWith("World"); // false
    const startsWithWorldAtIndex6 = str.startsWith("World", 6); // true
    ```
    
15. `endsWith(searchString, length)`: This method checks whether a string ends with a specified substring. The `length` parameter is optional, and specifies the number of characters from the end of the string to include in the search.
    
    ```jsx
    const str = "Hello world";
    const endsWithWorld = str.endsWith("world"); // true
    const endsWithHello = str.endsWith("Hello"); // false
    const endsWithLo = str.endsWith("lo", 5); // true
    ```
    
16. `repeat(count)`: This method returns a new string with a specified number of copies of the original string.
    
    ```jsx
    const str = "abc";
    const repeatedStr = str.repeat(3); // "abcabcabc"
    ```
    
17. `substr(startIndex, length)`: This method returns a new substring from a string, starting at the specified start index and with the specified length. If the length is not specified, the method will extract the substring until the end of the string.
    
    ```jsx
    const str = "Hello world";
    const subStr1 = str.substr(1, 3); // "ell"
    const subStr2 = str.substr(3); // "lo world"
    ```
    
18. `search(regexp)`: This method searches a string for a specified pattern (regular expression), and returns the index of the first match.
    
    ```jsx
    const str = "The quick brown fox jumps over the lazy dog";
    const index1 = str.search(/brown/i); // 10 (case-insensitive match)
    const index2 = str.search(/cat/i); // -1 (no match)
    ```
    
19. `match(regexp)`: This method searches a string for a specified pattern (regular expression), and returns an array of all matches.
    
    ```jsx
    const str = "The quick brown fox jumps over the lazy dog";
    const matches = str.match(/the/gi); // ["The", "the"]
    ```
    
20. `endsWith(searchString, length)`: This method checks whether a string ends with a specified substring. The `length` parameter is optional, and specifies the number of characters from the end of the string to include in the search.
    1. **`searchString`** (required): This is the string that we want to search for at the end of the original string.
    2. **`length`** (optional): This is the length of the original string that should be considered for the search. If this parameter is not specified, the entire string will be searched.
    
    ```jsx
    const str = "Hello World";
    console.log(str.endsWith("World")); // true
    console.log(str.endsWith("World", 5)); // false
    ```
    

## **Object**

1. **Object.keys(obj)**
    - Returns an array of a given object's own property names.
    
    ```jsx
    const user = { id: 1, name: "John" };
    console.log(Object.keys(user)); // ['id', 'name']
    ```
    
2. **Object.values(obj)**
    - Returns an array of a given object's own enumerable property values.
    
    ```jsx
    console.log(Object.values(user)); // [1, "John"]
    ```
    
3. **Object.entries(obj)**
    - Returns an array of key-value pairs of a given object's own properties.
    
    ```jsx
    console.log(Object.entries(user)); // [['id', 1], ['name', 'John']]
    ```
    
4. **Object.fromEntries(arr)**
    - Converts an array of key-value pairs into an object.
    
    ```jsx
    const entries = [['id', 1], ['name', 'John']];
    console.log(Object.fromEntries(entries)); // { id: 1, name: 'John' }
    ```
    
5. **Object.assign(target, ...sources)**
    - Copies the values of all enumerable properties from one or more source objects to a target object.
    
    ```jsx
    const obj1 = { a: 1 };
    const obj2 = { b: 2 };
    console.log(Object.assign(obj1, obj2)); // { a: 1, b: 2 }
    ```
    
6. **Object.freeze(obj)**
    - Prevents modification of an object's properties and values.
    
    ```jsx
    const obj = { name: "John" };
    Object.freeze(obj);
    obj.name = "Mike"; // No effect
    console.log(obj); // { name: "John" }
    ```
    
7. **Object.seal(obj)**
    - Prevents new properties from being added but allows modification of existing properties.
    
    ```jsx
    Object.seal(obj);
    obj.name = "Mike"; // Allowed
    obj.age = 30; // Not allowed
    ```
    
8. **Object.hasOwnProperty(prop)**
    - Checks if an object has a property as its own (not inherited).
    
    ```jsx
    console.log(user.hasOwnProperty("name")); // true
    ```
    
9. **Object.getOwnPropertyNames(obj)**
    - Returns an array of all properties (enumerable and non-enumerable) of an object.
    
    ```jsx
    console.log(Object.getOwnPropertyNames(user)); // ['id', 'name']
    ```
    
10. **Object.getOwnPropertyDescriptors(obj)**
    - Returns all own property descriptors of an object.
    
    ```jsx
    console.log(Object.getOwnPropertyDescriptors(user));
    ```
    
11. **Object.create(proto, [propertiesObject])**
    - Creates a new object with the specified prototype object and properties.
    
    ```jsx
    const person = { greet: () => "Hello" };
    const user1 = Object.create(person);
    console.log(user1.greet()); // "Hello"
    ```
    
12. **Object.defineProperty(obj, prop, descriptor)**
    - Defines a new property directly on an object.
    
    ```jsx
    Object.defineProperty(user, "age", { value: 30, writable: false });
    console.log(user.age); // 30
    ```
    
13. **Object.defineProperties(obj, props)**
    - Defines multiple new properties directly on an object.
    
    ```jsx
    Object.defineProperties(user, {
      city: { value: "Chennai", writable: true },
      country: { value: "India", writable: false },
    });
    ```
    
14. **Object.is(value1, value2)**
    - Compares if two values are the same.
    
    ```jsx
    console.log(Object.is(0, -0)); // false
    console.log(Object.is(NaN, NaN)); // true
    ```
    
15. **Object.isFrozen(obj)**
    - Checks if an object is frozen.
    
    ```jsx
    console.log(Object.isFrozen(obj)); // true
    ```
    
16. **Object.isSealed(obj)**
    - Checks if an object is sealed.
    
    ```jsx
    console.log(Object.isSealed(obj)); // true
    ```
    
17. **Object.toString()**
    - Returns a string representation of an object.
    
    ```jsx
    console.log(user.toString()); // "[object Object]"
    ```
    
18. **Object.getOwnPropertyDescriptor(obj, prop)**
    
    Retrieves the descriptor for a specific property on an object, detailing its configuration.
    
    ```jsx
    const user = { name: "Alice" };
    const descriptor = Object.getOwnPropertyDescriptor(user, "name");
    console.log(descriptor);
    // Output: { value: 'Alice', writable: true, enumerable: true, configurable: true }
    ```
    
19. **Object.getOwnPropertySymbols(obj)**
    
    Returns an array of all symbol properties found directly upon a given object.
    
    ```jsx
    const sym1 = Symbol("id");
    const user = { [sym1]: 123 };
    console.log(Object.getOwnPropertySymbols(user)); // [Symbol(id)]
    ```
    
20. **Object.getPrototypeOf(obj)**
    
    Returns the prototype (i.e., the internal `[[Prototype]]` property) of the specified object.
    
    ```jsx
    const user = {};
    const proto = Object.getPrototypeOf(user);
    console.log(proto === Object.prototype); // true
    ```
    
21. **Object.hasOwn(obj, prop)**
    
    Determines whether the object has the specified property as its own property (as opposed to inheriting it).
    
    ```jsx
    const user = { name: "Alice" };
    console.log(Object.hasOwn(user, "name")); // true
    console.log(Object.hasOwn(user, "toString")); // false
    ```
    
22. **Object.isExtensible(obj)**
    
    Determines if an object is extensible (i.e., whether new properties can be added to it).
    
    ```jsx
    const user = {};
    console.log(Object.isExtensible(user)); // true
    Object.preventExtensions(user);
    console.log(Object.isExtensible(user)); // false
    ```
    
23. **Object.preventExtensions(obj)**
    
    Prevents new properties from ever being added to an object (i.e., prevents future extensions to the object).
    
    ```jsx
    const user = { name: "Alice" };
    Object.preventExtensions(user);
    user.age = 30; // This will not be added
    console.log(user.age); // undefined
    ```
    
24. **Object.setPrototypeOf(obj, prototype)**
    
    Sets the prototype (i.e., the internal `[[Prototype]]` property) of a specified object to another object or `null`.
    
    ```jsx
    const user = {};
    const proto = { greet() { return "Hello"; } };
    Object.setPrototypeOf(user, proto);
    console.log(user.greet()); // "Hello"
    ```
    

These methods provide deeper control and introspection capabilities over JavaScript objects, which are crucial for advanced programming tasks. Understanding and effectively utilizing these methods will be beneficial for your interview preparation.

## **Number**

- Resource
    
    [📌12 JavaScript Number Methods Cheatsheet](https://dev.to/catherineisonline/12-javascript-number-methods-cheatsheet-1oie)
    
1. `Number.isFinite()` is a static method in JavaScript that determines whether the passed value is a finite number. It returns `true` if the passed value is a number that is neither `Infinity` nor `NaN`, otherwise it returns `false`.
    
    Here's an example usage of `Number.isFinite()`:
    
    ```jsx
    console.log(Number.isFinite(42)); // true
    console.log(Number.isFinite(Infinity)); // false
    console.log(Number.isFinite(-Infinity)); // false
    console.log(Number.isFinite(NaN)); // false
    console.log(Number.isFinite('42')); // false
    console.log(Number.isFinite(null)); // false
    ```
    
    In the example above, `Number.isFinite()` returns `true` for the number `42`, because it is a finite number. It returns `false` for `Infinity`, `-Infinity`, and `NaN`, because they are not finite numbers. It also returns `false` for non-numeric values like the string `'42'` and `null`.
    
2. `Number.isInteger()` is a built-in method in JavaScript that checks whether a given value is an integer or not. It returns `true` if the value is an integer, and `false` if it is not.
    
    The method checks if the given value is a number and if it has no fractional part. If the value is not a number or has a fractional part, then `Number.isInteger()` returns `false`.
    
    Here's an example:
    
    ```jsx
    console.log(Number.isInteger(5)); // true
    console.log(Number.isInteger(5.0)); // true
    console.log(Number.isInteger(5.1)); // false
    console.log(Number.isInteger('5')); // false
    console.log(Number.isInteger(null)); // false
    ```
    
    In this example, `Number.isInteger()` returns `true` for `5` and `5.0` because both are integers. However, it returns `false` for `5.1` because it has a fractional part. Similarly, it returns `false` for `'5'` because it is a string, not a number. Finally, it returns `false` for `null` because `null` is not a number.
    
3. `Number.isNaN()` is a method in JavaScript used to determine whether a given value is `NaN` (Not-A-Number) or not. This method returns `true` if the given value is `NaN`, and `false` if the value is not `NaN`.
    
    The `Number.isNaN()` method is different from the global `isNaN()` function in JavaScript. The `isNaN()` function also returns `true` if the value passed to it is not a number (such as a string or boolean value), in addition to returning `true` for `NaN`.
    
    Here is an example of using `Number.isNaN()`:
    
    ```jsx
    console.log(Number.isNaN(NaN)); // true
    console.log(Number.isNaN(5)); // false
    console.log(Number.isNaN('hello')); // false
    console.log(Number.isNaN(true)); // false
    console.log(isNaN('hello')); // true (using global isNaN() function)
    ```
    
    In the above example, `Number.isNaN()` returns `true` for `NaN`, and `false` for all other values. The global `isNaN()` function, on the other hand, returns `true` for the string `'hello'` because it is not a number.
    
4. The `Number.isSafeInteger()` method in JavaScript is used to determine whether the provided value is a safe integer or not. A safe integer is an integer that can be exactly represented as a 53-bit signed integer.
    
    The method returns `true` if the value is a safe integer, `false` otherwise. The method does not perform any type coercion, meaning that the value passed to it must be of the `number` type and not a string or any other type.
    
    Here's an example usage of `Number.isSafeInteger()`:
    
    ```jsx
    console.log(Number.isSafeInteger(1234567890123456)); // true
    console.log(Number.isSafeInteger(12345678901234567890)); // false
    console.log(Number.isSafeInteger(1.23)); // false
    console.log(Number.isSafeInteger('123')); // false
    ```
    
    In the above example, the first call to `Number.isSafeInteger()` returns `true` because `1234567890123456` is a safe integer. The second call returns `false` because `12345678901234567890` is not a safe integer. The third and fourth calls return `false` because the passed values are not of the `number` type.
    
5. `Number.parseFloat()` is a built-in function in JavaScript that parses a string argument and returns a floating point number. The function parses a string until it reaches a character that cannot be converted to a number and returns the number that it has parsed so far.
    
    For example:
    
    ```jsx
    const num1 = Number.parseFloat('10.5');
    console.log(num1); // 10.5
    
    const num2 = Number.parseFloat('10.5 is a float');
    console.log(num2); // 10.5
    ```
    
    In the first example, the function returns the number 10.5 after parsing the string '10.5'. In the second example, the function stops parsing the string at the space character after the number 10.5 and returns the number 10.5.
    
    If the first character of the string cannot be converted to a number, `Number.parseFloat()` returns `NaN` (Not a Number):
    
    ```jsx
    const num3 = Number.parseFloat('not a number');
    console.log(num3); // NaN
    ```
    
6. `Number.parseInt()` is a built-in method in JavaScript used to parse a string and convert it to an integer. It takes two arguments: the first argument is the string to be parsed and the second argument is an optional radix (base) of the number.
    
    The syntax for `Number.parseInt()` is:
    
    ```jsx
    Number.parseInt(string, radix)
    ```
    
    Here, `string` is the string to be parsed, and `radix` is an optional argument that specifies the base of the number to be parsed. The value of radix can be between 2 and 36. If `radix` is not specified, `Number.parseInt()` assumes a base of 10.
    
    For example, consider the following code:
    
    ```jsx
    const str = '123';
    const num = Number.parseInt(str);
    console.log(num); // Output: 123
    ```
    
    In the above example, we have a string `'123'` which is converted to an integer using `Number.parseInt()`. The resulting value is assigned to the variable `num`, which is then logged to the console.
    
    If the string to be parsed is not a valid number or if the radix is invalid, `Number.parseInt()` returns `NaN`.
    
    ```jsx
    const str = 'abc';
    const num = Number.parseInt(str);
    console.log(num); // Output: NaN
    ```
    
    In this example, the string `'abc'` cannot be parsed as an integer, so `Number.parseInt()` returns `NaN`.
    
7. `Number.prototype.toExponential()` is a method in JavaScript that returns a string representing the number in exponential notation (scientific notation) with one digit before the decimal point and a specified number of digits after the decimal point.
    
    The syntax of the `toExponential()` method is as follows:
    
    ```jsx
    num.toExponential(fractionDigits)
    ```
    
    Here, `num` is the number you want to convert to exponential notation, and `fractionDigits` is the number of digits after the decimal point. If `fractionDigits` is not specified, it defaults to as many digits as necessary to represent the value.
    
    For example:
    
    ```jsx
    let num = 123456;
    
    console.log(num.toExponential()); // Output: 1.23456e+5
    console.log(num.toExponential(2)); // Output: 1.23e+5
    console.log(num.toExponential(6)); // Output: 1.234560e+5
    ```
    
    In the first example, `fractionDigits` is not specified, so the method uses as many digits as necessary to represent the value. In the second example, `fractionDigits` is 2, so the method rounds the number to two digits after the decimal point. In the third example, `fractionDigits` is 6, so the method adds zeros after the fifth digit to have six digits after the decimal point.
    
8. `Number.prototype.toFixed()` is a built-in function in JavaScript that returns a string representation of a given number in fixed-point notation. It takes an argument `digits` which specifies the number of digits to appear after the decimal point.
    
    Syntax:
    
    ```jsx
    numObj.toFixed(digits)
    ```
    
    Parameters:
    
    - `digits` (optional) - The number of digits to appear after the decimal point. This parameter is optional and its default value is 0.
    
    Return value:
    
    - A string representation of the given number in fixed-point notation.
    
    Example:
    
    ```jsx
    const num = 3.14159;
    console.log(num.toFixed());    // Output: 3
    console.log(num.toFixed(2));  // Output: 3.14
    console.log(num.toFixed(4));  // Output: 3.1416
    ```
    
    In the above example, `toFixed()` method is used to format the given number `num` with specific number of digits after the decimal point. If the `digits` parameter is not passed or set to 0, then `toFixed()` will return a whole number. If the `digits` parameter is greater than the number of digits after the decimal point, then `toFixed()` will add zeros after the last digit. If the `digits` parameter is less than the number of digits after the decimal point, then `toFixed()` will round the last digit according to standard rounding rules.
    
9. The `Number.prototype.toLocaleString()` method is used to return a string with a language-sensitive representation of the number. It converts the number to a string using a specific locale and format. The locale is determined by the user's browser settings or can be set manually.
    
    The `toLocaleString()` method has several parameters, but the most commonly used parameter is the `locales` parameter, which specifies the locale to use. The locale can be specified as a string or an array of strings, with each string representing a specific language or region.
    
    Here's an example of how to use `toLocaleString()` to format a number with a specific locale:
    
    ```jsx
    const number = 123456.789;
    const formattedNumber = number.toLocaleString('en-US'); // "123,456.789"
    ```
    
    In this example, `toLocaleString()` formats the number `123456.789` using the `en-US` locale, which uses a comma as the thousands separator and a dot as the decimal separator.
    
    The `toLocaleString()` method also has additional parameters for specifying the number of decimal places, the style of currency, and other formatting options.
    
10. `Number.prototype.toPrecision()` is a method in JavaScript that returns a string representing a number in exponential notation, with the specified number of digits.
    
    Syntax: `numObj.toPrecision([precision])`
    
    Here, `numObj` is the number object, and `precision` is an optional argument that specifies the number of significant digits. It must be an integer value between 1 and 21.
    
    If the `precision` argument is not specified, the `toPrecision()` method returns the number as is, without any rounding or truncation.
    
    Example:
    
    ```jsx
    let num = 12345.6789;
    
    console.log(num.toPrecision());   // 12345.6789
    console.log(num.toPrecision(2));  // 1.2e+4
    console.log(num.toPrecision(4));  // 1.235e+4
    console.log(num.toPrecision(8));  // 12345.679
    ```
    
    In the first example, since no precision is specified, the number is returned as is.
    
    In the second example, the number is rounded off to two significant digits, resulting in the scientific notation representation `1.2e+4`.
    
    In the third example, the number is rounded off to four significant digits, resulting in the scientific notation representation `1.235e+4`.
    
    In the fourth example, the number is rounded off to eight significant digits, resulting in the decimal representation `12345.679`.
    
11. `Number.prototype.toString()` is a method that returns a string representing the specified number object. It can take a single argument, which specifies the radix (base) of the number system to be used when converting the number to a string. If the radix is not specified or is 10, the number is converted to a decimal (base 10) string.
    
    Here's an example:
    
    ```jsx
    const num = 42;
    console.log(num.toString()); // "42"
    console.log(num.toString(2)); // "101010"
    console.log(num.toString(16)); // "2a"
    ```
    
    In the first line, we create a variable `num` with the value `42`. We then call `toString()` on `num` with no arguments, which converts the number to a decimal string.
    
    In the second line, we call `toString()` with an argument of `2`, which converts the number to a binary string.
    
    In the third line, we call `toString()` with an argument of `16`, which converts the number to a hexadecimal string.
    
12. `Number.prototype.valueOf()` is a method in JavaScript that returns the primitive value of a `Number` object. The `valueOf()` method is called automatically by JavaScript when a `Number` object is used in a context where a primitive value is expected, such as when using arithmetic operators.
    
    Here is an example:
    
    ```jsx
    const numObj = new Number(10);
    console.log(numObj.valueOf()); // 10
    ```
    
    In the example above, we create a new `Number` object `numObj` with a value of 10. The `valueOf()` method is then called on `numObj`, which returns the primitive value of 10.
    
    The `valueOf()` method can be used to extract the primitive value of a `Number` object and perform arithmetic operations on it.
    
    Here's an example:
    
    ```jsx
    const numObj1 = new Number(5);
    const numObj2 = new Number(10);
    console.log(numObj1.valueOf() + numObj2.valueOf()); // 15
    ```
    
    In the example above, we create two `Number` objects `numObj1` and `numObj2` with values of 5 and 10 respectively. We then extract the primitive value of each `Number` object using the `valueOf()` method and perform an addition operation on them. The result of the addition is 15.
    

## **Queue**

- **Resource**
    
    [JavaScript Data Structures - Queues](https://youtu.be/yKhJHsW0Zm4)
    

Queues are known as First In First Out also known as FIFO data structure

A queue works based on the first-in, first-out (FIFO) principle, which is different from a stack, which works based on the last-in, first-out (LIFO) principle.

Here is an example of a Queue data structure implemented in JavaScript

```jsx
class Queue {
  constructor() {
    this.items = [];
  }

  enqueue(item) {
		// push() will add the element in the end of the array
    this.items.push(item);
  }

  dequeue() {
    if (this.isEmpty()) {
      return null;
    }
		// shift() will remove the first element in the original array
		// Note: shift() change the original array by removing the first 
		// elemet in array
    return this.items.shift();
  }

	// front() method is used to check which element is 
	// going to be removed next
  front() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items[0];
  }

  isEmpty() {
    return this.items.length === 0;
  }

	print() {
    for(let i=0; this.items.length > i; i++){
			console.log(this.items[i])
		}
  }

  size() {
    return this.items.length;
  }
}

// Example usage:
const queue = new Queue();
queue.enqueue('a');
queue.enqueue('b');
queue.enqueue('c');
queue.print()
console.log(queue.size()); // Output: 3
console.log(queue.front()); // Output: 'a'
console.log(queue.dequeue()); // Output: 'a'
console.log(queue.dequeue()); // Output: 'b'
console.log(queue.size()); // Output: 1
```

In this example, the `Queue` class has several methods for adding, removing, and inspecting elements in the queue:

- `enqueue(item)` adds an item to the end of the queue.
- `dequeue()` removes and returns the item at the front of the queue. If the queue is empty, it returns `null`.
- `front()` returns the item at the front of the queue without removing it. If the queue is empty, it returns `null`.
- `isEmpty()` returns a boolean indicating whether the queue is empty.
- `size()` returns the number of elements in the queue.

In the example usage, we create a new queue, add three items to it, and then use the various methods to manipulate and inspect the queue.

## **Stack**

- **Resource**
    
    

Stack are known as Last In First Out also known as LIFO data structure

In JavaScript, a Stack is a data structure that follows the Last-In-First-Out (LIFO) principle, where the last element added to the stack is the first element to be removed. A real-world example of a stack is a stack of books where the last book added is the first one to be taken out.

Here's an example of how to use a Stack in JavaScript:

```jsx
class Stack {
  constructor() {
    this.items = [];
  }

  push(item) {
		// push() method adds new items to the end of an array.
		// push() method changes the length of the array.
		// push() method returns the new length.
    this.items.push(item);
  }

  pop() {
    if (this.isEmpty()) {
      return null;
    }
		// pop() method removes the last element of an array.
		// pop() method changes the original array.
		// pop() method returns the removed element.
    return this.items.pop();
  }

  peek() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items[this.items.length - 1];
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }
}

// Example usage:
const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);
console.log(stack.size()); // Output: 3
console.log(stack.peek()); // Output: 3
console.log(stack.pop()); // Output: 3
console.log(stack.pop()); // Output: 2
console.log(stack.size()); // Output: 1
```

In this example, we define a `Stack` class with several methods for adding, removing, and inspecting elements in the stack:

- `push(item)` adds an item to the top of the stack.
- `pop()` removes and returns the item at the top of the stack. If the stack is empty, it returns `null`.
- `peek()` returns the item at the top of the stack without removing it. If the stack is empty, it returns `null`.
- `isEmpty()` returns a boolean indicating whether the stack is empty.
- `size()` returns the number of elements in the stack.

we create a new stack, pushed three items to it, and then use the various methods to manipulate and inspect the queue

## **LinkedList**

- **Resource**
    
    [JavaScript Data Structures - Linked Lists](https://youtu.be/ab0XzfD7iC0)
    

A linked list is a linear data structure in which elements are stored in nodes and each node points to the next node in the sequence. In JavaScript, linked lists are typically implemented using objects with `value` and `next` properties. The `value` property stores the data and the `next` property is a reference to the next node in the list.

One real-world example of a linked list is a music playlist (Spotify). Each song in the playlist is represented by a node, with a value containing the song's metadata (such as the artist, album, and title) and a next property pointing to the next song in the playlist.

When a new song is added to the playlist, a new node is created and added to the end of the linked list. If a song is removed, its node is removed from the linked list and the next node of the previous node is updated to point to the next song in the playlist.

Linked lists are useful for playlists because they allow for easy insertion and removal of songs without having to move large amounts of data in memory. Each song node only needs to store information about the song itself and the location of the next song in the playlist. This makes linked lists a more efficient data structure for handling dynamic lists of items like playlists.

Here is an example of a singly linked list in JavaScript

```jsx
class LinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }

  // method to add an element to the linked list
  add(element) {
    const node = {
      value: element,
      next: null
    };
    if (!this.head) {
      this.head = node;
    } else {
      let current = this.head;
      while (current.next) {
        current = current.next;
      }
      current.next = node;
    }
    this.size++;
  }

  // method to remove an element from the linked list
  remove(element) {
    let current = this.head;
    let previous = null;
    while (current) {
      if (current.value === element) {
        if (!previous) {
          this.head = current.next;
        } else {
          previous.next = current.next;
        }
        this.size--;
        return current.value;
      }
      previous = current;
      current = current.next;
    }
    return null;
  }

  // method to check if the linked list is empty
  isEmpty() {
    return this.size === 0;
  }

  // method to get the size of the linked list
  getSize() {
    return this.size;
  }

  // method to get the head node of the linked list
  getHead() {
    return this.head;
  }

  // method to get a string representation of the linked list
  toString() {
    let current = this.head;
    let string = '';
    while (current) {
      string += current.value + ' -> ';
      current = current.next;
    }
    string += 'null';
    return string;
  }
}

const list = new LinkedList();
list.add(1);
list.add(2);
list.add(3);
console.log(list.toString()); // Output: '1 -> 2 -> 3 -> null'
list.remove(2);
console.log(list.toString()); // Output: '1 -> 3 -> null'
console.log(list.isEmpty()); // Output: false
console.log(list.getSize()); // Output: 2
console.log(list.getHead().value); // Output: 1
```

- `constructor` method sets the initial state of the linked list with a `head` property set to `null` and a `size` property set to `0`.
    
    ```json
    // Constructor value value stored like
    {
      "head": { "value": 1,"next": { "value": 2, "next": { "value": 3, "next": null }}},
    	"size": 3
    }
    ```
    
- `add` method takes an element as a parameter, creates a new node object with that element value, and adds it to the end of the linked list. If the linked list is empty, the new node becomes the head. Otherwise, the method traverses the linked list to find the last node and adds the new node as its next node.
- `remove` method takes an element as a parameter and removes the first occurrence of a node with that value from the linked list. It traverses the linked list to find the node to remove and reassigns the next node of the previous node to the next node of the removed node. It also decreases the size of the linked list.
- `isEmpty` method checks whether the linked list is empty by comparing the `size` property to `0`.
- `getSize` method returns the current size of the linked list.
- `getHead` method returns the head node of the linked list.
- `toString` method returns a string representation of the linked list by traversing the linked list and concatenating the value of each node followed by '->', except for the last node, which is followed by 'null'.

## **DoublyLinkedList**

- **Resource**
    
    

A doubly linked list is a type of linked list where each node contains two pointers: one that points to the next node in the list and another that points to the previous node in the list. This allows for traversal in both directions, making it a more flexible data structure than a singly linked list.

One real-world example of a linked list is a music playlist (Spotify). Each song in the playlist is represented by a node, with a value containing the song's metadata (such as the artist, album, and title) and a next property pointing to the next song in the playlist.

When a new song is added to the playlist, a new node is created and added to the end of the linked list. If a song is removed, its node is removed from the linked list and the next node of the previous node is updated to point to the next song in the playlist.

Doubly Linked lists are useful for playlists because they allow for easy insertion and removal of songs without having to move large amounts of data in memory. Each song node only needs to store information about the song itself and the location of the next song in the playlist. This makes linked lists a more efficient data structure for handling dynamic lists of items like playlists.

Here's an example implementation of a doubly linked list in JavaScript

```jsx
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
    this.prev = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  // method to add a node to the end of the list
  add(value) {
    const node = new Node(value);
    if (!this.head) {
      this.head = node;
      this.tail = node;
    } else {
      node.prev = this.tail;
      this.tail.next = node;
      this.tail = node;
    }
    this.size++;
  }

  // method to remove a node from the list
  remove(value) {
    let current = this.head;
    while (current) {
      if (current.value === value) {
        if (current === this.head) {
          this.head = current.next;
          if (this.head) {
            this.head.prev = null;
          } else {
            this.tail = null;
          }
        } else if (current === this.tail) {
          this.tail = current.prev;
          this.tail.next = null;
        } else {
          current.prev.next = current.next;
          current.next.prev = current.prev;
        }
        this.size--;
        return true;
      }
      current = current.next;
    }
    return false;
  }

  // method to check if the list is empty
  isEmpty() {
    return this.size === 0;
  }

  // method to get the size of the list
  getSize() {
    return this.size;
  }

  // method to get a string representation of the list
  toString() {
    let current = this.head;
    let string = '';
    while (current) {
      string += current.value + ' <-> ';
      current = current.next;
    }
    string += 'null';
    return string;
  }
}

const list = new DoublyLinkedList();
list.add(1);
list.add(2);
list.add(3);
console.log(list.toString()); // Output: '1 <-> 2 <-> 3 <-> null'
list.remove(2);
console.log(list.toString()); // Output: '1 <-> 3 <-> null'
console.log(list.isEmpty()); // Output: false
console.log(list.getSize()); // Output: 2
```

In this example, the `Node` class represents each node in the doubly linked list, with a `value` property and `next` and `prev` pointers. The `DoublyLinkedList` class contains methods to add and remove nodes from the list, as well as check if the list is empty, get the size of the list, and get a string representation of the list.

When a node is added to the list, it is added to the end of the list by updating the `next` and `prev` pointers of the previous and next nodes, respectively. When a node is removed from the list, the `next` and `prev` pointers of the previous and next nodes are updated to remove the node from the list.

## **Binary Tree**

A binary tree is a type of data structure that is composed of nodes, where each node has at most two children, referred to as the left and right child. The topmost node of the tree is known as the root node, and nodes with no children are called leaf nodes.

In JavaScript, a binary tree can be implemented using a class with properties for the left and right child and a value for the node itself. Here's an example implementation:

```jsx
class BinaryTreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
```

This creates a class for a binary tree node with a constructor that takes a value as a parameter and initializes properties for the left and right child to null.

To create a binary tree, we can instantiate instances of this class and set their left and right children as needed. Here's an example of a binary tree with root value 1, left child 2, and right child 3:

```jsx
const root = new BinaryTreeNode(1);
root.left = new BinaryTreeNode(2);
root.right = new BinaryTreeNode(3);
```

This creates a binary tree with the following structure:

```jsx
     1
   /   \
  2     3
```

We can continue to add nodes to the tree by setting the left and right children of existing nodes. For example, we can add a left child to the node with value 2:

```jsx
root.left.left = new BinaryTreeNode(4);
```

This results in the following tree:

```jsx
     1
   /   \
  2     3
 /
4
```

Binary trees can be used for a variety of applications, such as representing hierarchical data or implementing binary search algorithms.

Tasks: 

- Task - Invert Binary Tree JavaScript Solution
    
    Inverting a binary tree means that for every node in the tree, its left and right subtrees are swapped. This can be achieved recursively by traversing the tree and swapping the left and right children of each node. Here's a JavaScript solution to invert a binary tree:
    
    ```jsx
    function invertTree(root) {
      if (root === null) {
        return null;
      }
      // Swap left and right subtrees recursively
      const left = invertTree(root.left);
      const right = invertTree(root.right);
      root.left = right;
      root.right = left;
      return root;
    }
    ```
    
    In this solution, the function `invertTree` takes the root of the binary tree as input and returns the inverted binary tree.
    
    The function starts by checking if the input `root` is `null`. If it is, the function simply returns `null`.
    
    If the `root` is not `null`, the function recursively calls itself to invert the left and right subtrees of the `root`. The result of these recursive calls are stored in `left` and `right` variables.
    
    Then, the left and right subtrees of the `root` are swapped by assigning the `right` subtree to the `root.left` property and the `left` subtree to the `root.right` property.
    
    Finally, the function returns the inverted `root`.