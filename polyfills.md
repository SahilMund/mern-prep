# Polyfills Implementation Guide

## 📚 Table of Contents
- [Array Polyfills](#array-polyfills)
- [String Polyfills](#string-polyfills)
- [Object Polyfills](#object-polyfills)
- [Function Polyfills](#function-polyfills)
- [Promise Polyfills](#promise-polyfills)

---

## 🔹1. Array Polyfills ⭐⭐⭐

### 1. **Array.prototype.forEach** ⭐

#### What it does:
Executes a provided function once for each array element.

**Example:**
```javascript
[1, 2, 3].forEach(x => console.log(x * 2)); // 2, 4, 6
```

#### Walkthrough:
1. Check if callback is a function
2. Iterate through array using for loop
3. Call callback with current element, index, and array

#### Implementation:

**Basic:**
```javascript
Array.prototype.myForEach = function(callback) {
  // this refers to the array
  for (let i = 0; i < this.length; i++) {
    // Call callback with current element, index, and array
    callback(this[i], i, this);
  }
};
```

**Advanced:**
```javascript
Array.prototype.myForEach = function(callback, thisArg) {
  // Check if callback is a function
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }
  
  // Handle sparse arrays
  const array = Object(this);
  const length = array.length >>> 0; // Convert to uint32
  
  for (let i = 0; i < length; i++) {
    // Skip empty slots in sparse arrays
    if (i in array) {
      // Call callback with bound thisArg if provided
      callback.call(thisArg, array[i], i, array);
    }
  }
};
```

---

### 2. **Array.prototype.find** ⭐

#### What it does:
Returns the first element that satisfies the testing function.

**Example:**
```javascript
[5, 12, 8, 130, 44].find(x => x > 10); // 12
```

#### Walkthrough:
1. Iterate through array
2. Test each element with predicate function
3. Return first matching element or undefined

#### Implementation:

**Basic:**
```javascript
Array.prototype.myFind = function(callback) {
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {
      return this[i];
    }
  }
  return undefined;
};
```

**Advanced:**
```javascript
Array.prototype.myFind = function(callback, thisArg) {
  if (typeof callback !== 'function') {
    throw new TypeError('predicate must be a function');
  }
  
  const array = Object(this);
  const length = array.length >>> 0;
  
  for (let i = 0; i < length; i++) {
    if (i in array && callback.call(thisArg, array[i], i, array)) {
      return array[i];
    }
  }
  
  return undefined;
};
```

---

### 3. **Array.prototype.includes** ⭐⭐

#### What it does:
Determines whether an array includes a certain value.

**Example:**
```javascript
[1, 2, 3].includes(2); // true
```

#### Walkthrough:
1. Handle fromIndex parameter
2. Use SameValueZero algorithm (NaN === NaN)
3. Return boolean

#### Implementation:

**Basic:**
```javascript
Array.prototype.myIncludes = function(searchElement, fromIndex = 0) {
  const start = fromIndex < 0 ? this.length + fromIndex : fromIndex;
  
  for (let i = start; i < this.length; i++) {
    // Special handling for NaN
    if (Number.isNaN(searchElement) && Number.isNaN(this[i])) {
      return true;
    }
    if (this[i] === searchElement) {
      return true;
    }
  }
  return false;
};
```

**Advanced:**
```javascript
Array.prototype.myIncludes = function(searchElement, fromIndex = 0) {
  const array = Object(this);
  const length = array.length >>> 0;
  
  // Handle negative fromIndex
  let k = Math.max(fromIndex >= 0 ? fromIndex : length + fromIndex, 0);
  
  // Special SameValueZero comparison
  const sameValueZero = (x, y) => {
    return x === y || (typeof x === 'number' && typeof y === 'number' && isNaN(x) && isNaN(y));
  };
  
  while (k < length) {
    if (k in array && sameValueZero(array[k], searchElement)) {
      return true;
    }
    k++;
  }
  
  return false;
};
```

---

### 4. **Array.prototype.map** ⭐⭐

#### What it does:
Creates a new array with results of calling a function on every element.

**Example:**
```javascript
[1, 2, 3].map(x => x * 2); // [2, 4, 6]
```

#### Walkthrough:
1. Create new array
2. Apply callback to each element
3. Store results in new array
4. Return new array

#### Implementation:

**Basic:**
```javascript
Array.prototype.myMap = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }
  return result;
};
```

**Advanced:**
```javascript
Array.prototype.myMap = function(callback, thisArg) {
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }
  
  const array = Object(this);
  const length = array.length >>> 0;
  const result = new Array(length);
  
  for (let i = 0; i < length; i++) {
    if (i in array) {
      result[i] = callback.call(thisArg, array[i], i, array);
    }
  }
  
  return result;
};
```

---

### 5. **Array.prototype.filter** ⭐⭐

#### What it does:
Creates a new array with elements that pass the test.

**Example:**
```javascript
[1, 2, 3, 4].filter(x => x > 2); // [3, 4]
```

#### Walkthrough:
1. Create new array
2. Test each element
3. Push passing elements to new array

#### Implementation:

**Basic:**
```javascript
Array.prototype.myFilter = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};
```

**Advanced:**
```javascript
Array.prototype.myFilter = function(callback, thisArg) {
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }
  
  const array = Object(this);
  const length = array.length >>> 0;
  const result = [];
  
  for (let i = 0; i < length; i++) {
    if (i in array && callback.call(thisArg, array[i], i, array)) {
      result.push(array[i]);
    }
  }
  
  return result;
};
```

---

### 6. **Array.prototype.reduce** ⭐⭐⭐

#### What it does:
Executes a reducer function on each element, returning a single value.

**Example:**
```javascript
[1, 2, 3, 4].reduce((acc, curr) => acc + curr, 0); // 10
```

#### Walkthrough:
1. Handle initial value
2. Iterate through array
3. Accumulate result
4. Return final value

#### Implementation:

**Basic:**
```javascript
Array.prototype.myReduce = function(callback, initialValue) {
  let accumulator = initialValue !== undefined ? initialValue : this[0];
  let startIndex = initialValue !== undefined ? 0 : 1;
  
  for (let i = startIndex; i < this.length; i++) {
    accumulator = callback(accumulator, this[i], i, this);
  }
  
  return accumulator;
};
```

**Advanced:**
```javascript
Array.prototype.myReduce = function(callback, initialValue) {
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }
  
  const array = Object(this);
  const length = array.length >>> 0;
  
  if (length === 0 && initialValue === undefined) {
    throw new TypeError('Reduce of empty array with no initial value');
  }
  
  let accumulator;
  let startIndex;
  
  if (initialValue !== undefined) {
    accumulator = initialValue;
    startIndex = 0;
  } else {
    // Find first existing element
    let k = 0;
    while (k < length && !(k in array)) {
      k++;
    }
    
    if (k >= length) {
      throw new TypeError('Reduce of empty array with no initial value');
    }
    
    accumulator = array[k];
    startIndex = k + 1;
  }
  
  for (let i = startIndex; i < length; i++) {
    if (i in array) {
      accumulator = callback(accumulator, array[i], i, array);
    }
  }
  
  return accumulator;
};
```

---

### 7. **Array.from**

#### What it does:
Creates a new array from array-like or iterable object.

**Example:**
```javascript
Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']
Array.from([1, 2, 3], x => x * 2); // [2, 4, 6]
```

#### Walkthrough:
1. Check if object is iterable
2. Handle map function
3. Create array

#### Implementation:

**Basic:**
```javascript
Array.myFrom = function(arrayLike, mapFn, thisArg) {
  const result = [];
  const length = arrayLike.length;
  
  for (let i = 0; i < length; i++) {
    let value = arrayLike[i];
    if (mapFn) {
      value = mapFn.call(thisArg, value, i);
    }
    result.push(value);
  }
  
  return result;
};
```

**Advanced:**
```javascript
Array.myFrom = function(arrayLike, mapFn, thisArg) {
  // Check constructor
  const C = this;
  
  // Handle null/undefined
  if (arrayLike == null) {
    throw new TypeError('Array.from requires an array-like object');
  }
  
  // Check if mapFn is callable
  if (mapFn !== undefined && typeof mapFn !== 'function') {
    throw new TypeError('Array.from: when provided, the second argument must be a function');
  }
  
  const items = Object(arrayLike);
  const length = items.length >>> 0;
  
  // Check if length is finite
  if (length < 0 || length >= Number.MAX_SAFE_INTEGER) {
    throw new RangeError('Invalid array length');
  }
  
  const result = new C(length);
  
  for (let i = 0; i < length; i++) {
    const value = items[i];
    result[i] = mapFn ? mapFn.call(thisArg, value, i) : value;
  }
  
  result.length = length;
  return result;
};
```

---

### 8. **Array.prototype.flat**

#### What it does:
Flattens nested arrays up to specified depth.

**Example:**
```javascript
[1, [2, [3, [4]]]].flat(2); // [1, 2, 3, [4]]
```

#### Walkthrough:
1. Handle depth parameter
2. Recursively flatten arrays
3. Handle sparse arrays

#### Implementation:

**Basic:**
```javascript
Array.prototype.myFlat = function(depth = 1) {
  const result = [];
  
  const flatten = (arr, currentDepth) => {
    for (const item of arr) {
      if (Array.isArray(item) && currentDepth > 0) {
        flatten(item, currentDepth - 1);
      } else {
        result.push(item);
      }
    }
  };
  
  flatten(this, depth);
  return result;
};
```

**Advanced:**
```javascript
Array.prototype.myFlat = function(depth = 1) {
  const array = Object(this);
  const length = array.length >>> 0;
  const result = [];
  
  const flattenIntoArray = (target, source, sourceLen, start, depth) => {
    let targetIndex = start;
    let sourceIndex = 0;
    
    while (sourceIndex < sourceLen) {
      const P = sourceIndex;
      const exists = P in source;
      
      if (exists) {
        const element = source[P];
        
        if (depth > 0 && Array.isArray(element)) {
          const innerLen = element.length;
          targetIndex = flattenIntoArray(target, element, innerLen, targetIndex, depth - 1);
        } else {
          if (targetIndex >= 2**53 - 1) {
            throw new TypeError('Index too large');
          }
          target[targetIndex] = element;
          targetIndex++;
        }
      }
      sourceIndex++;
    }
    
    return targetIndex;
  };
  
  flattenIntoArray(result, array, length, 0, depth);
  return result;
};
```

---

### 9. **Array.prototype.every**

#### What it does:
Tests whether all elements pass the test.

**Example:**
```javascript
[2, 4, 6].every(x => x % 2 === 0); // true
```

#### Walkthrough:
1. Test each element
2. Return false immediately on first failure
3. Return true if all pass

#### Implementation:

**Basic:**
```javascript
Array.prototype.myEvery = function(callback) {
  for (let i = 0; i < this.length; i++) {
    if (!callback(this[i], i, this)) {
      return false;
    }
  }
  return true;
};
```

**Advanced:**
```javascript
Array.prototype.myEvery = function(callback, thisArg) {
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }
  
  const array = Object(this);
  const length = array.length >>> 0;
  
  for (let i = 0; i < length; i++) {
    if (i in array && !callback.call(thisArg, array[i], i, array)) {
      return false;
    }
  }
  
  return true;
};
```

---

## 🔹2. String Polyfills ⭐⭐

### 1. **String.prototype.includes** ⭐⭐

#### What it does:
Determines whether one string contains another.

**Example:**
```javascript
'Hello World'.includes('World'); // true
```

#### Walkthrough:
1. Handle position parameter
2. Perform substring search
3. Return boolean

#### Implementation:

**Basic:**
```javascript
String.prototype.myIncludes = function(searchString, position = 0) {
  // Convert position to integer
  const pos = Math.max(0, Math.min(position, this.length));
  
  // Check if searchString is found starting from position
  return this.indexOf(searchString, pos) !== -1;
};
```

**Advanced:**
```javascript
String.prototype.myIncludes = function(searchString, position = 0) {
  // Check if called on valid string
  if (this == null) {
    throw new TypeError('String.prototype.includes called on null or undefined');
  }
  
  // Convert to string
  const str = String(this);
  const searchStr = String(searchString);
  
  // Handle position
  const pos = position ? Number(position) : 0;
  const len = str.length;
  const start = Math.min(Math.max(pos, 0), len);
  
  // Perform search
  const searchLen = searchStr.length;
  
  // If search string is longer than remaining string
  if (searchLen > len - start) {
    return false;
  }
  
  // Simple search algorithm
  for (let i = start; i <= len - searchLen; i++) {
    let found = true;
    for (let j = 0; j < searchLen; j++) {
      if (str[i + j] !== searchStr[j]) {
        found = false;
        break;
      }
    }
    if (found) {
      return true;
    }
  }
  
  return false;
};
```

---

### 2. **String.prototype.repeat** ⭐

#### What it does:
Returns a new string repeated specified times.

**Example:**
```javascript
'abc'.repeat(3); // 'abcabcabc'
```

#### Walkthrough:
1. Handle count parameter validation
2. Build repeated string
3. Handle edge cases (0, negative, infinity)

#### Implementation:

**Basic:**
```javascript
String.prototype.myRepeat = function(count) {
  if (count < 0) throw new RangeError('Invalid count');
  
  let result = '';
  for (let i = 0; i < count; i++) {
    result += this;
  }
  return result;
};
```

**Advanced:**
```javascript
String.prototype.myRepeat = function(count) {
  // Check if called on valid string
  const str = String(this);
  
  // Convert count to integer
  const n = Number(count);
  
  // Validate count
  if (n < 0 || n === Infinity) {
    throw new RangeError('Invalid count value');
  }
  
  if (n === 0) {
    return '';
  }
  
  // Handle large counts efficiently
  let result = '';
  let i = n;
  
  // Using exponentiation for efficiency
  if (str.length === 0 || n === 0) {
    return '';
  }
  
  // Maximum length check (2^53 - 1 is max string length in JS)
  if (str.length * n >= 2**53) {
    throw new RangeError('Invalid string length');
  }
  
  // Efficient repetition using doubling
  while (i > 0) {
    if (i & 1) { // If i is odd
      result += str;
    }
    if (i > 1) {
      str += str; // Double the string
    }
    i >>= 1; // Halve i
  }
  
  return result;
};
```

---

### 3. **String.prototype.trim**

#### What it does:
Removes whitespace from both ends of a string.

**Example:**
```javascript
'  hello  '.trim(); // 'hello'
```

#### Walkthrough:
1. Define whitespace characters
2. Find first non-whitespace from start
3. Find last non-whitespace from end
4. Extract substring

#### Implementation:

**Basic:**
```javascript
String.prototype.myTrim = function() {
  // Trim start and end
  return this.replace(/^\s+|\s+$/g, '');
};
```

**Advanced:**
```javascript
String.prototype.myTrim = function() {
  // Check if called on valid string
  const str = String(this);
  
  // List of whitespace characters
  const whitespace = [
    '\u0009', // CHARACTER TABULATION
    '\u000A', // LINE FEED
    '\u000B', // LINE TABULATION
    '\u000C', // FORM FEED
    '\u000D', // CARRIAGE RETURN
    '\u0020', // SPACE
    '\u00A0', // NO-BREAK SPACE
    '\u1680', '\u2000', '\u2001', '\u2002', '\u2003', '\u2004', 
    '\u2005', '\u2006', '\u2007', '\u2008', '\u2009', '\u200A',
    '\u202F', '\u205F', '\u3000', '\uFEFF'  // ZERO WIDTH NO-BREAK SPACE
  ].join('');
  
  const whitespaceRegex = new RegExp(`^[${whitespace}]+|[${whitespace}]+$`, 'g');
  
  return str.replace(whitespaceRegex, '');
};
```

---

## 🔹3. Object Polyfills ⭐⭐

### 1. **Object.entries**

#### What it does:
Returns array of key-value pairs of object's own enumerable properties.

**Example:**
```javascript
Object.entries({a: 1, b: 2}); // [['a', 1], ['b', 2]]
```

#### Walkthrough:
1. Get object's own enumerable properties
2. Create array of [key, value] pairs
3. Handle non-object inputs

#### Implementation:

**Basic:**
```javascript
Object.myEntries = function(obj) {
  const result = [];
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      result.push([key, obj[key]]);
    }
  }
  return result;
};
```

**Advanced:**
```javascript
Object.myEntries = function(obj) {
  // Convert to object
  const object = Object(obj);
  
  // Get own enumerable properties
  const props = Object.keys(object);
  const result = new Array(props.length);
  
  for (let i = 0; i < props.length; i++) {
    const key = props[i];
    result[i] = [key, object[key]];
  }
  
  return result;
};
```

---

### 2. **Object.values**

#### What it does:
Returns array of object's own enumerable property values.

**Example:**
```javascript
Object.values({a: 1, b: 2}); // [1, 2]
```

#### Implementation:

**Basic:**
```javascript
Object.myValues = function(obj) {
  const result = [];
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      result.push(obj[key]);
    }
  }
  return result;
};
```

**Advanced:**
```javascript
Object.myValues = function(obj) {
  const object = Object(obj);
  const props = Object.keys(object);
  const result = new Array(props.length);
  
  for (let i = 0; i < props.length; i++) {
    result[i] = object[props[i]];
  }
  
  return result;
};
```

---

### 3. **Object.keys**

#### What it does:
Returns array of object's own enumerable property names.

**Example:**
```javascript
Object.keys({a: 1, b: 2}); // ['a', 'b']
```

#### Implementation:

**Basic:**
```javascript
Object.myKeys = function(obj) {
  const result = [];
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      result.push(key);
    }
  }
  return result;
};
```

**Advanced:**
```javascript
Object.myKeys = function(obj) {
  // Handle null/undefined
  if (obj == null) {
    throw new TypeError('Cannot convert undefined or null to object');
  }
  
  const object = Object(obj);
  const result = [];
  
  // Get own enumerable properties
  for (const key in object) {
    if (Object.prototype.hasOwnProperty.call(object, key)) {
      result.push(key);
    }
  }
  
  return result;
};
```

---

### 4. **Object.freeze**

#### What it does:
Freezes an object preventing new properties, deletions, and modifications.

**Example:**
```javascript
const obj = {a: 1};
Object.freeze(obj);
obj.a = 2; // Fails silently in non-strict mode
```

#### Walkthrough:
1. Make all properties non-configurable
2. Make all properties non-writable
3. Prevent extensions
4. Handle nested objects

#### Implementation:

**Basic:**
```javascript
Object.myFreeze = function(obj) {
  // Prevent adding/deleting properties
  Object.preventExtensions(obj);
  
  // Make existing properties non-writable and non-configurable
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      Object.defineProperty(obj, key, {
        writable: false,
        configurable: false
      });
    }
  }
  
  return obj;
};
```

**Advanced:**
```javascript
Object.myFreeze = function(obj) {
  // Handle primitives
  if (typeof obj !== 'object' || obj === null) {
    return obj;
  }
  
  // Get own properties (including non-enumerable)
  const props = Object.getOwnPropertyNames(obj);
  const propSymbols = Object.getOwnPropertySymbols(obj);
  const allProps = props.concat(propSymbols);
  
  // Freeze each property
  for (const prop of allProps) {
    const descriptor = Object.getOwnPropertyDescriptor(obj, prop);
    
    // If property is configurable or writable, freeze it
    if (descriptor) {
      if (descriptor.configurable || descriptor.writable) {
        Object.defineProperty(obj, prop, {
          configurable: false,
          writable: false
        });
      }
      
      // Recursively freeze nested objects
      if (typeof descriptor.value === 'object' && descriptor.value !== null) {
        Object.myFreeze(descriptor.value);
      }
    }
  }
  
  // Prevent extensions
  Object.preventExtensions(obj);
  
  return obj;
};
```

---

### 5. **Object.create**

#### What it does:
Creates a new object with specified prototype.

**Example:**
```javascript
const proto = {greet() { return 'Hello'; }};
const obj = Object.create(proto);
obj.greet(); // 'Hello'
```

#### Implementation:

**Basic:**
```javascript
Object.myCreate = function(proto, propertiesObject) {
  // Create constructor function
  function F() {}
  F.prototype = proto;
  
  // Create new object with specified prototype
  const obj = new F();
  
  // Add properties if provided
  if (propertiesObject) {
    Object.defineProperties(obj, propertiesObject);
  }
  
  return obj;
};
```

**Advanced:**
```javascript
Object.myCreate = function(proto, propertiesObject) {
  // Handle null prototype
  if (proto === null) {
    // Create object with null prototype
    const obj = {};
    Object.setPrototypeOf(obj, null);
    
    if (propertiesObject !== undefined) {
      Object.defineProperties(obj, propertiesObject);
    }
    
    return obj;
  }
  
  // Validate proto
  if (typeof proto !== 'object' && typeof proto !== 'function') {
    throw new TypeError('Object prototype may only be an Object or null');
  }
  
  // Create constructor
  function F() {}
  F.prototype = proto;
  
  const obj = new F();
  
  // Handle propertiesObject
  if (propertiesObject !== undefined) {
    if (typeof propertiesObject !== 'object') {
      throw new TypeError('Properties must be an object');
    }
    
    Object.defineProperties(obj, propertiesObject);
  }
  
  return obj;
};
```

---

### 6. **Object.assign** ⭐⭐

#### What it does:
Copies properties from source objects to target object.

**Example:**
```javascript
Object.assign({a: 1}, {b: 2}, {c: 3}); // {a: 1, b: 2, c: 3}
```

#### Walkthrough:
1. Handle multiple source objects
2. Copy enumerable own properties
3. Handle Symbol properties
4. Return target object

#### Implementation:

**Basic:**
```javascript
Object.myAssign = function(target, ...sources) {
  if (target == null) {
    throw new TypeError('Cannot convert undefined or null to object');
  }
  
  const result = Object(target);
  
  for (const source of sources) {
    if (source != null) {
      for (const key in source) {
        if (source.hasOwnProperty(key)) {
          result[key] = source[key];
        }
      }
    }
  }
  
  return result;
};
```

**Advanced:**
```javascript
Object.myAssign = function(target, ...sources) {
  // Handle null/undefined target
  if (target == null) {
    throw new TypeError('Cannot convert undefined or null to object');
  }
  
  // Convert target to object
  const to = Object(target);
  
  for (let i = 0; i < sources.length; i++) {
    const source = sources[i];
    
    if (source != null) {
      // Get all enumerable properties (including Symbols)
      const keys = [
        ...Object.getOwnPropertyNames(source),
        ...Object.getOwnPropertySymbols(source)
      ];
      
      for (const key of keys) {
        // Check if property is enumerable
        const descriptor = Object.getOwnPropertyDescriptor(source, key);
        if (descriptor && descriptor.enumerable) {
          to[key] = source[key];
        }
      }
    }
  }
  
  return to;
};
```

---

### 7. **Object.seal**

#### What it does:
Seals an object preventing new properties and making existing properties non-configurable.

**Example:**
```javascript
const obj = {a: 1};
Object.seal(obj);
obj.b = 2; // Fails silently
delete obj.a; // Fails silently
obj.a = 3; // Works
```

#### Implementation:

**Basic:**
```javascript
Object.mySeal = function(obj) {
  // Prevent adding/deleting properties
  Object.preventExtensions(obj);
  
  // Make existing properties non-configurable
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      Object.defineProperty(obj, key, {
        configurable: false
      });
    }
  }
  
  return obj;
};
```

**Advanced:**
```javascript
Object.mySeal = function(obj) {
  // Handle primitives
  if (typeof obj !== 'object' || obj === null) {
    return obj;
  }
  
  // Get all own properties
  const props = Object.getOwnPropertyNames(obj);
  const propSymbols = Object.getOwnPropertySymbols(obj);
  const allProps = props.concat(propSymbols);
  
  // Make all properties non-configurable
  for (const prop of allProps) {
    const descriptor = Object.getOwnPropertyDescriptor(obj, prop);
    
    if (descriptor && descriptor.configurable) {
      Object.defineProperty(obj, prop, {
        configurable: false
      });
    }
  }
  
  // Prevent extensions
  Object.preventExtensions(obj);
  
  return obj;
};
```

---

## 🔹4. Function Polyfills (High Interview Value) ⭐⭐⭐

### 1. **Function.prototype.call** ⭐

#### What it does:
Calls a function with given this value and arguments.

**Example:**
```javascript
function greet() { return this.name; }
greet.call({name: 'John'}); // 'John'
```

#### Walkthrough:
1. Set this context
2. Pass arguments
3. Execute function

#### Implementation:

**Basic:**
```javascript
Function.prototype.myCall = function(context, ...args) {
  // Use global object if context is null/undefined
  context = context || window;
  
  // Create unique property to avoid conflicts
  const fnSymbol = Symbol('fn');
  context[fnSymbol] = this;
  
  // Call function with context
  const result = context[fnSymbol](...args);
  
  // Clean up
  delete context[fnSymbol];
  
  return result;
};
```

**Advanced:**
```javascript
Function.prototype.myCall = function(context, ...args) {
  // Strict mode handling
  'use strict';
  
  // Handle null/undefined context
  if (context == null) {
    context = (function() { return this; })() || globalThis;
  }
  
  // Convert primitive values to objects
  context = Object(context);
  
  // Create unique Symbol to avoid property conflicts
  const fnSymbol = Symbol('fn');
  
  // Define property with proper descriptors
  Object.defineProperty(context, fnSymbol, {
    value: this,
    enumerable: false,
    configurable: true,
    writable: true
  });
  
  try {
    // Call the function
    const result = context[fnSymbol](...args);
    return result;
  } finally {
    // Always clean up
    delete context[fnSymbol];
  }
};
```

---

### 2. **Function.prototype.apply** ⭐⭐

#### What it does:
Calls a function with given this value and arguments as array.

**Example:**
```javascript
function sum(a, b) { return a + b; }
sum.apply(null, [1, 2]); // 3
```

#### Implementation:

**Basic:**
```javascript
Function.prototype.myApply = function(context, argsArray) {
  context = context || window;
  const fnSymbol = Symbol('fn');
  context[fnSymbol] = this;
  
  const result = context[fnSymbol](...(argsArray || []));
  
  delete context[fnSymbol];
  return result;
};
```

**Advanced:**
```javascript
Function.prototype.myApply = function(context, argsArray) {
  'use strict';
  
  if (context == null) {
    context = globalThis;
  }
  
  context = Object(context);
  
  // Validate argsArray
  if (argsArray !== undefined && argsArray !== null) {
    if (!Array.isArray(argsArray) && !isArrayLike(argsArray)) {
      throw new TypeError('CreateListFromArrayLike called on non-object');
    }
  }
  
  const fnSymbol = Symbol('fn');
  
  Object.defineProperty(context, fnSymbol, {
    value: this,
    enumerable: false
  });
  
  try {
    // Spread argsArray or call without arguments
    const result = argsArray 
      ? context[fnSymbol](...argsArray)
      : context[fnSymbol]();
    return result;
  } finally {
    delete context[fnSymbol];
  }
  
  // Helper function
  function isArrayLike(obj) {
    return obj != null && 
           typeof obj === 'object' && 
           'length' in obj &&
           typeof obj.length === 'number' &&
           obj.length >= 0 &&
           obj.length < Number.MAX_SAFE_INTEGER;
  }
};
```

---

### 3. **Function.prototype.bind** ⭐⭐⭐

#### What it does:
Creates a new function with bound this value and partial arguments.

**Example:**
```javascript
function multiply(a, b) { return a * b; }
const double = multiply.bind(null, 2);
double(5); // 10
```

#### Walkthrough:
1. Create closure with bound context and args
2. Handle new operator (construct calls)
3. Preserve function properties

#### Implementation:

**Basic:**
```javascript
Function.prototype.myBind = function(context, ...args) {
  const originalFunc = this;
  
  return function(...innerArgs) {
    return originalFunc.apply(context, [...args, ...innerArgs]);
  };
};
```

**Advanced:**
```javascript
Function.prototype.myBind = function(context, ...args) {
  'use strict';
  
  // Check if called on a function
  if (typeof this !== 'function') {
    throw new TypeError('Function.prototype.bind called on non-function');
  }
  
  const originalFunc = this;
  
  // Get function name for better debugging
  const funcName = originalFunc.name || '';
  
  // Create bound function
  const boundFunc = function(...innerArgs) {
    // Determine this value based on how bound function is called
    const thisContext = this instanceof boundFunc ? this : context;
    
    // Combine arguments
    const allArgs = args.concat(innerArgs);
    
    // Call original function
    return originalFunc.apply(thisContext, allArgs);
  };
  
  // Preserve prototype chain for instanceof
  if (originalFunc.prototype) {
    // Create intermediate constructor to avoid modifying original prototype
    function Empty() {}
    Empty.prototype = originalFunc.prototype;
    boundFunc.prototype = new Empty();
    Empty.prototype = null; // Clean up
  }
  
  // Preserve function properties
  Object.defineProperties(boundFunc, {
    length: {
      value: Math.max(0, originalFunc.length - args.length),
      writable: false,
      enumerable: false,
      configurable: true
    },
    name: {
      value: funcName ? `bound ${funcName}` : 'bound',
      writable: false,
      enumerable: false,
      configurable: true
    }
  });
  
  return boundFunc;
};
```

---

### 4. **Debounce** ⭐⭐⭐

#### What it does:
Creates a debounced function that delays execution until after wait time.

**Example:**
```javascript
const debouncedSearch = debounce(searchAPI, 300);
input.addEventListener('input', debouncedSearch);
```

#### Walkthrough:
1. Track timeout ID
2. Clear previous timeout on new call
3. Execute after delay
4. Handle immediate execution option

#### Implementation:

**Basic:**
```javascript
function debounce(func, wait) {
  let timeout;
  
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      func.apply(this, args);
    }, wait);
  };
}
```

**Advanced:**
```javascript
function debounce(func, wait, options = {}) {
  let timeout;
  let lastCallTime;
  let lastInvokeTime = 0;
  let leading = false;
  let trailing = true;
  let maxWait;
  
  // Parse options
  if (typeof options === 'object') {
    leading = !!options.leading;
    trailing = 'trailing' in options ? !!options.trailing : true;
    maxWait = 'maxWait' in options ? Math.max(options.maxWait, wait) : maxWait;
  }
  
  // Invoke the function
  function invokeFunc(time, args) {
    lastInvokeTime = time;
    return func.apply(this, args);
  }
  
  // Leading edge invocation
  function leadingEdge(time, args) {
    lastInvokeTime = time;
    if (leading) {
      return invokeFunc(time, args);
    }
  }
  
  // Calculate remaining wait time
  function remainingWait(time) {
    const timeSinceLastCall = time - lastCallTime;
    const timeSinceLastInvoke = time - lastInvokeTime;
    
    const result = wait - timeSinceLastCall;
    
    return maxWait === undefined 
      ? result 
      : Math.min(result, maxWait - timeSinceLastInvoke);
  }
  
  // Check if should invoke
  function shouldInvoke(time) {
    const timeSinceLastCall = time - lastCallTime;
    const timeSinceLastInvoke = time - lastInvokeTime;
    
    return (
      lastCallTime === undefined || 
      timeSinceLastCall >= wait ||
      timeSinceLastCall < 0 ||
      (maxWait !== undefined && timeSinceLastInvoke >= maxWait)
    );
  }
  
  // Timer expired
  function timerExpired() {
    const time = Date.now();
    if (shouldInvoke(time)) {
      return trailingEdge(time);
    }
    timeout = setTimeout(timerExpired, remainingWait(time));
  }
  
  // Trailing edge invocation
  function trailingEdge(time) {
    timeout = undefined;
    
    if (trailing && lastCallTime !== undefined) {
      return invokeFunc(time, lastArgs);
    }
    lastArgs = lastThis = undefined;
  }
  
  // Cancel debounced call
  function cancel() {
    if (timeout !== undefined) {
      clearTimeout(timeout);
    }
    lastInvokeTime = 0;
    lastCallTime = lastArgs = lastThis = timeout = undefined;
  }
  
  // Flush pending execution
  function flush() {
    return timeout === undefined ? result : trailingEdge(Date.now());
  }
  
  let lastArgs;
  let lastThis;
  let result;
  
  // Return debounced function
  const debounced = function(...args) {
    const time = Date.now();
    const isInvoking = shouldInvoke(time);
    
    lastArgs = args;
    lastThis = this;
    lastCallTime = time;
    
    if (isInvoking) {
      if (timeout === undefined) {
        result = leadingEdge(lastCallTime, lastArgs);
      }
      
      if (maxWait !== undefined) {
        timeout = setTimeout(timerExpired, wait);
        return result;
      }
    }
    
    if (timeout === undefined) {
      timeout = setTimeout(timerExpired, wait);
    }
    
    return result;
  };
  
  debounced.cancel = cancel;
  debounced.flush = flush;
  
  return debounced;
}
```

---

### 5. **Throttle** ⭐⭐⭐

#### What it does:
Creates a throttled function that only executes once per time period.

**Example:**
```javascript
const throttledScroll = throttle(handleScroll, 100);
window.addEventListener('scroll', throttledScroll);
```

#### Walkthrough:
1. Track last execution time
2. Schedule trailing execution
3. Handle leading and trailing options

#### Implementation:

**Basic:**
```javascript
function throttle(func, wait) {
  let lastCall = 0;
  let timeout;
  
  return function(...args) {
    const now = Date.now();
    const remaining = wait - (now - lastCall);
    
    if (remaining <= 0) {
      lastCall = now;
      func.apply(this, args);
    } else if (!timeout) {
      timeout = setTimeout(() => {
        lastCall = Date.now();
        timeout = null;
        func.apply(this, args);
      }, remaining);
    }
  };
}
```

**Advanced:**
```javascript
function throttle(func, wait, options = {}) {
  let timeout;
  let lastArgs;
  let lastThis;
  let result;
  let lastCallTime = 0;
  
  let leading = true;
  let trailing = true;
  
  // Parse options
  if (typeof options === 'object') {
    leading = 'leading' in options ? !!options.leading : leading;
    trailing = 'trailing' in options ? !!options.trailing : trailing;
  }
  
  // Invoke function
  function invokeFunc(time) {
    const args = lastArgs;
    const context = lastThis;
    
    lastArgs = lastThis = undefined;
    lastCallTime = time;
    result = func.apply(context, args);
    
    return result;
  }
  
  // Start timer
  function startTimer(pendingFunc, wait) {
    return setTimeout(pendingFunc, wait);
  }
  
  // Leading edge
  function leadingEdge(time) {
    lastCallTime = time;
    if (leading) {
      return invokeFunc(time);
    }
  }
  
  // Remaining wait time
  function remainingWait(time) {
    const timeSinceLastCall = time - lastCallTime;
    return wait - timeSinceLastCall;
  }
  
  // Should invoke
  function shouldInvoke(time) {
    const timeSinceLastCall = time - lastCallTime;
    return (
      lastCallTime === 0 || 
      timeSinceLastCall >= wait || 
      timeSinceLastCall < 0
    );
  }
  
  // Timer expired
  function timerExpired() {
    const time = Date.now();
    if (shouldInvoke(time)) {
      return trailingEdge(time);
    }
    timeout = startTimer(timerExpired, remainingWait(time));
  }
  
  // Trailing edge
  function trailingEdge(time) {
    timeout = undefined;
    
    if (trailing && lastArgs) {
      return invokeFunc(time);
    }
    lastArgs = lastThis = undefined;
  }
  
  // Cancel
  function cancel() {
    if (timeout !== undefined) {
      clearTimeout(timeout);
    }
    lastCallTime = 0;
    lastArgs = lastThis = timeout = undefined;
  }
  
  // Flush
  function flush() {
    return timeout === undefined ? result : trailingEdge(Date.now());
  }
  
  // Throttled function
  const throttled = function(...args) {
    const time = Date.now();
    const isInvoking = shouldInvoke(time);
    
    lastArgs = args;
    lastThis = this;
    
    if (isInvoking) {
      if (timeout === undefined) {
        return leadingEdge(time);
      }
      
      if (trailing) {
        clearTimeout(timeout);
        timeout = startTimer(timerExpired, wait);
      }
      
      return result;
    }
    
    if (timeout === undefined && trailing) {
      timeout = startTimer(timerExpired, wait);
    }
    
    return result;
  };
  
  throttled.cancel = cancel;
  throttled.flush = flush;
  
  return throttled;
}
```

---

## 🔹5. Promise Polyfills (Advanced) ⭐⭐⭐

### 1. **Promise** (Simplified)

#### What it does:
Creates a promise representing eventual completion of async operation.

**Example:**
```javascript
new Promise((resolve, reject) => {
  setTimeout(() => resolve('Done!'), 1000);
});
```

#### Implementation (Simplified):

```javascript
class MyPromise {
  constructor(executor) {
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledCallbacks = [];
    this.onRejectedCallbacks = [];
    
    const resolve = (value) => {
      if (this.state === 'pending') {
        this.state = 'fulfilled';
        this.value = value;
        this.onFulfilledCallbacks.forEach(fn => fn());
      }
    };
    
    const reject = (reason) => {
      if (this.state === 'pending') {
        this.state = 'rejected';
        this.reason = reason;
        this.onRejectedCallbacks.forEach(fn => fn());
      }
    };
    
    try {
      executor(resolve, reject);
    } catch (error) {
      reject(error);
    }
  }
  
  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value;
    onRejected = typeof onRejected === 'function' ? onRejected : reason => { throw reason };
    
    return new MyPromise((resolve, reject) => {
      const handleFulfilled = () => {
        setTimeout(() => {
          try {
            const result = onFulfilled(this.value);
            resolvePromise(result, resolve, reject);
          } catch (error) {
            reject(error);
          }
        });
      };
      
      const handleRejected = () => {
        setTimeout(() => {
          try {
            const result = onRejected(this.reason);
            resolvePromise(result, resolve, reject);
          } catch (error) {
            reject(error);
          }
        });
      };
      
      if (this.state === 'fulfilled') {
        handleFulfilled();
      } else if (this.state === 'rejected') {
        handleRejected();
      } else {
        this.onFulfilledCallbacks.push(handleFulfilled);
        this.onRejectedCallbacks.push(handleRejected);
      }
    });
  }
  
  catch(onRejected) {
    return this.then(null, onRejected);
  }
  
  finally(onFinally) {
    return this.then(
      value => MyPromise.resolve(onFinally()).then(() => value),
      reason => MyPromise.resolve(onFinally()).then(() => { throw reason })
    );
  }
}

// Helper function
function resolvePromise(promise, resolve, reject) {
  if (promise === promise) { // Detect thenable
    let then;
    try {
      then = promise.then;
    } catch (error) {
      return reject(error);
    }
    
    if (typeof then === 'function') {
      let called = false;
      try {
        then.call(
          promise,
          value => {
            if (called) return;
            called = true;
            resolvePromise(value, resolve, reject);
          },
          reason => {
            if (called) return;
            called = true;
            reject(reason);
          }
        );
      } catch (error) {
        if (called) return;
        reject(error);
      }
    } else {
      resolve(promise);
    }
  } else {
    resolve(promise);
  }
}

// Static methods
MyPromise.resolve = function(value) {
  if (value instanceof MyPromise) return value;
  return new MyPromise(resolve => resolve(value));
};

MyPromise.reject = function(reason) {
  return new MyPromise((_, reject) => reject(reason));
};
```

---

### 5. **Promise.all** ⭐⭐⭐

#### What it does:
Waits for all promises to resolve or any to reject.

**Example:**
```javascript
Promise.all([promise1, promise2]).then(values => {
  console.log(values); // [val1, val2]
});
```

#### Implementation:

**Basic:**
```javascript
MyPromise.all = function(promises) {
  return new MyPromise((resolve, reject) => {
    const results = new Array(promises.length);
    let completed = 0;
    
    if (promises.length === 0) {
      resolve(results);
      return;
    }
    
    promises.forEach((promise, index) => {
      MyPromise.resolve(promise).then(
        value => {
          results[index] = value;
          completed++;
          
          if (completed === promises.length) {
            resolve(results);
          }
        },
        reject
      );
    });
  });
};
```

**Advanced:**
```javascript
MyPromise.all = function(iterable) {
  return new MyPromise((resolve, reject) => {
    // Convert iterable to array
    const promises = Array.from(iterable);
    const results = new Array(promises.length);
    let remaining = promises.length;
    
    if (remaining === 0) {
      resolve(results);
      return;
    }
    
    const onFulfilled = (index, value) => {
      try {
        if (value instanceof MyPromise) {
          if (value === results[index]) {
            return reject(new TypeError('Circular reference'));
          }
        }
        
        results[index] = value;
        
        if (--remaining === 0) {
          resolve(results);
        }
      } catch (error) {
        reject(error);
      }
    };
    
    promises.forEach((promise, index) => {
      try {
        // Handle thenables
        if (promise && typeof promise.then === 'function') {
          promise.then(
            value => onFulfilled(index, value),
            reject
          );
        } else {
          onFulfilled(index, promise);
        }
      } catch (error) {
        reject(error);
      }
    });
  });
};
```

---

## 📖 Usage Examples

```javascript
// Test Array polyfills
const arr = [1, 2, 3];
arr.myForEach(x => console.log(x));
const doubled = arr.myMap(x => x * 2);

// Test Function polyfills
function greet(name) {
  return `Hello, ${this.title} ${name}`;
}
const boundGreet = greet.myBind({title: 'Mr.'}, 'John');

// Test Promise polyfills
const p1 = new MyPromise(resolve => setTimeout(() => resolve('First'), 100));
const p2 = new MyPromise(resolve => setTimeout(() => resolve('Second'), 50));

MyPromise.all([p1, p2]).then(console.log);
```

## 📝 Notes

1. **⭐⭐⭐ Most Important**: Focus on `reduce`, `bind`, `debounce`, `throttle`, `Promise.all`
2. **⭐⭐ Moderately Important**: `map`, `filter`, `includes`, `assign`, `call`, `apply`
3. **⭐ Less Important**: Others for completeness

## 🔧 Testing Recommendations

1. Test edge cases (empty arrays, null values, etc.)
2. Test with sparse arrays
3. Test Symbol properties for Object polyfills
4. Test with async operations for Promise polyfills
5. Test with constructor calls for bind polyfill

## ⚠️ Caveats

1. These are educational implementations
2. May not cover 100% of spec edge cases
3. Performance may differ from native implementations
4. Use native implementations in production when available
