# set, map, weak

Sets and maps will be (are) finally available in ES6! No more spartan way to manipulate data structures. This chapter explains how we can deal with **Map**, **Set**, **WeakMap** and **WeakSet**.

## Map

Maps are a store for **key** / **value** pairs. Key and value could be a primitives or object references.

Let's create a map:

```javascript
let map = new Map(),
    val2 = 'val2',
    val3 = {
      key: 'value'
    };

map.set(0, 'val1');
map.set('1', val2);
map.set({ key: 2 }, val3);

console.log(map); // Map {0 => 'val1', '1' => 'val2', Object {key: 2} => Object {key: 'value'}}
```

We can also use a constructor to create the sam map, based on array param passed to the constructor:

```javascript
let map,
    val2 = 'val2',
    val3 = {
      key: 'value'
    };

map = new Map([[0, 'val1'], ['1', val2], [{ key: 2 }, val3]]);

console.log(map); // Map {0 => 'val1', '1' => 'val2', Object {key: 2} => Object {key: 'value'}}
```

To get a value by using a key, we have to use a **get()** method to do it (surprising):

```javascript
let map = new Map(),
    val2 = 'val2',
    val3 = {
      key: 'value'
    };

map.set(0, 'val1');
map.set('1', val2);
map.set({ key: 2 }, val3);

console.log(map.get('1')); // val2
```

To iterate over the map collection, we can use built-in **forEach** method or use new **for..of** structure:

```javascript
// forEach
let map = new Map(),
    val2 = 'val2',
    val3 = {
      key: 'value'
    };

map.set(0, 'val1');
map.set('1', val2);
map.set({ key: 2 }, val3);

map.forEach(function (value, key) {
  console.log(`Key: ${key} has value: ${value}`);
  // Key: 0 has value: val1
  // Key: 1 has value: val2
  // Key: [object Object] has value: [object Object]
});
```

```javascript
// for..of
let map = new Map(),
    val2 = 'val2',
    val3 = {
      key: 'value'
    };

map.set(0, 'val1');
map.set('1', val2);
map.set({ key: 2 }, val3);

for (let entry of map) {
  console.log(`Key: ${entry[0]} has value: ${entry[1]}`);
  // Key: 0 has value: val1
  // Key: 1 has value: val2
  // Key: [object Object] has value: [object Object]
};
```

We can also use a couple of methods to iterate:

* **entries()** — get all entries
* **keys()** — get only all keys
* **values()** — get only all values

To check if value is stored in our map, we can use **has()** method:

```javascript
let map = new Map(),
    val2 = 'val2',
    val3 = {
      key: 'value'
    };

map.set(0, 'val1');
map.set('1', val2);
map.set({ key: 2 }, val3);

console.log(map.has(0));     // true
console.log(map.has('key')); // false
```

To delete entry, we have **delete()** method:

```javascript
let map = new Map(),
    val2 = 'val2',
    val3 = {
      key: 'value'
    };

map.set(0, 'val1');
map.set('1', val2);
map.set({ key: 2 }, val3);

console.log(map.size); // 3

map.delete('1');

console.log(map.size); // 2
```

..and to clear all collection, we use clear() method:

```javascript
 let map = new Map(),
     val2 = 'val2',
     val3 = {
       key: 'value'
     };

 map.set(0, 'val1');
 map.set('1', val2);
 map.set({ key: 2 }, val3);

 console.log(map.size); // 3

 map.clear();

 console.log(map.size); // 0
```

## Set

It's a collection for *unique* values. The values could be also a primitives or object references.

```javascript
let set = new Set();

set.add(1);
set.add('1');
set.add({ key: 'value' });

console.log(set); // Set {1, '1', Object {key: 'value'}}
```

Like a map, set allows to create collection by passing an array to its constructor:

```javascript
let set = new Set([1, '1', { key: 'value' }]);

console.log(set); // Set {1, '1', Object {key: 'value'}}
```

To iterate over sets we have the same two options — built-in **forEach** function or **for..of** structure:

```javascript
// forEach
let set = new Set([1, '1', { key: 'value' }]);

set.forEach(function (value) {
  console.log(value);
  // 1
  // '1'
  // Object {key: 'value'}
});
```

```javascript
// for..of
let set = new Set([1, '1', { key: 'value' }]);

for (let value of set) {
  console.log(value);
  // 1
  // '1'
  // Object {key: 'value'}
};
```

**Set doesn't allow to add duplicates.**

```javascript
let set = new Set([1, 1, 1, 2, 5, 5, 6, 9]);
console.log(set.size); // 5!
```

We can also use **has()**, **delete()**, **clear()** methods, which are similar to the Map versions.

## WeakMap

WeakMaps provides leak-free object keyed side tables. It's a Map that doesn't prevent its keys from being **garbage-collected**. We don't have to worry about memory leaks.

If the object is destroyed, the garbage collector removes an entry from the WeakMap and frees memory.

**Keys must be objects.**

It has almost the same API like a Map, but we **can't iterate** over the WeakMap collection. We can't even determine the length of the collection because we don't have **size** attribute here.

The API looks like this:

```javascript
new WeakMap([iterable])

WeakMap.prototype.get(key)        : any
WeakMap.prototype.set(key, value) : this
WeakMap.prototype.has(key)        : boolean
WeakMap.prototype.delete(key)     : boolean
```

```javascript
let wm = new WeakMap(),
    obj = {
      key1: {
        k: 'v1'
      },
      key2: {
        k: 'v2'
      }
   };

wm.set(obj.key1, 'val1');
wm.set(obj.key2, 'val2');

console.log(wm); // WeakMap {Object {k: 'v1'} => 'val1', Object {k: 'v2'} => 'val2'}
console.log(wm.has(obj.key1)); // true

delete obj.key1;

console.log(wm.has(obj.key1)); // false
```

## WeakSet

Like a WeakMap, WeakSet is a Seat that doesn't prevent its values from being garbage-collected. It has simpler API than WeakMap, because has only three methods:

```javascript
new WeakSet([iterable])

WeakSet.prototype.add(value)    : any
WeakSet.prototype.has(value)    : boolean
WeakSet.prototype.delete(value) : boolean
```

**WeakSets are collections of unique objects only.**

WeakSet collection can't be iterated and we cannot determine its size.

```javascript
let ws = new WeakSet(),
    obj = {
      key1: {
        k: 'v1'
      },
      key2: {
        k: 'v2'
      }
   };

ws.add(obj.key1);
ws.add(obj.key2);

console.log(ws); // WeakSet {Object {k: 'v1'}, Object {k: 'v2'}}
console.log(ws.has(obj.key1)); // true

delete obj.key1;

console.log(ws.has(obj.key1)); // false
```
