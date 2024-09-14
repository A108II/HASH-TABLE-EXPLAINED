## Hash Tables and Hash Functions

- **Hash Functions**: Typically one-way functions. They map keys to hash values (numbers), but the process is non-reversible, meaning you can't retrieve the original key from the hash value.

- **Determinism**: Hash tables are deterministic, meaning given the same key and hash function, the table will always produce the same hash value. This ensures:

  - **Consistent Lookups**: The same key always hashes to the same location, allowing efficient retrieval of values.

## Collision Resolution Techniques

1. **Separate Chaining**: 
   - Each bucket stores a linked list of key-value pairs.
   - In case of collisions, the new key-value pair is added to the list within that bucket.

2. **Linear Probing**: 
   - Searches for the next available slot in the hash table linearly when a collision occurs and places the key-value pair in the first empty bucket.

## Time Complexity of Hash Tables

- The time complexity of the **_hash()** function depends on the **length of the key** (e.g., the string or data you're hashing), not on the **number of elements** stored in the hash table or the **length of the hash table** (i.e., the number of buckets). The process of generating a hash value is independent of the size of the table or how many items it holds.

- The time complexity of **set(key, value)** and **get(key)** functions is **O(1)** in the **average case**, but can be **O(n)** in the **worst case** due to collisions. In the average case, efficient hashing minimizes collisions, while the worst case occurs when multiple keys hash to the same bucket.


# HashTable Class Documentation

## Overview
This JavaScript implementation of a Hash Table uses an array-based structure to store key-value pairs. It supports basic operations such as setting and retrieving values based on keys, as well as fetching all stored keys.

## Features
- **Efficient Storage**: Hash table maps keys to array indices using a hash function.
- **Collision Handling**: Stores multiple key-value pairs at the same index using arrays.
- **Basic Methods**: Includes `set`, `get`, and `keys` methods for adding, retrieving, and listing keys.

---

## Code Implementation

```javascript
class HashTable {
    constructor(size = 7) {
        this.dataMap = new Array(size);
    }

    _hash(key) {
        let hash = 0;
        for(let i = 0; i < key.length; i++) {
            hash = (hash + key.charCodeAt(i) * 17) % this.dataMap.length;
        }
        return hash;
    }

    set(key, value) {
        let index = this._hash(key);
        if(!this.dataMap[index]) {
            this.dataMap[index] = [];
        }
        this.dataMap[index].push([key, value]);
        return this;
    }

    get(key) {
        let index = this._hash(key);
        if(this.dataMap[index]) {
            for(let i = 0; i < this.dataMap[index].length; i++) {
                if(this.dataMap[index][i][0] === key) {
                    return this.dataMap[index][i][1];
                }
            }
        }
        return undefined;
    }

    keys() {
        let keysList = [];
        for(let i = 0; i < this.dataMap.length; i++) {
            if(this.dataMap[i]) {
                for(let j = 0; j < this.dataMap[i].length; j++) {
                    keysList.push(this.dataMap[i][j][0]);
                }
            }
        }
        return keysList;
    }
}
```

---

## Explanation

### 1. **Constructor**

```javascript
constructor(size = 7) {
    this.dataMap = new Array(size);
}
```
- Initializes the hash table with a default size of 7. `dataMap` is an array that holds key-value pairs.

### 2. **Hash Function**

```javascript
_hash(key) {
    let hash = 0;
    for(let i = 0; i < key.length; i++) {
        hash = (hash + key.charCodeAt(i) * 17) % this.dataMap.length;
    }
    return hash;
}
```
- Generates a hash for a given string key. Each character's ASCII value is multiplied by 17 (a prime number for better distribution) and summed up, modulo the array length.
- **Purpose**: Maps the key to a valid array index.

### 3. **`set` Method**

```javascript
set(key, value) {
    let index = this._hash(key);
    if(!this.dataMap[index]) {
        this.dataMap[index] = [];
    }
    this.dataMap[index].push([key, value]);
    return this;
}
```
- **Purpose**: Adds a key-value pair to the hash table.
- If the index is empty, an array is created at that position. The key-value pair is stored in the array (to handle collisions).
- **Example**: 
  ```javascript
  thisHashTable.set('MacBook', 1200);
  ```

### 4. **`get` Method**

```javascript
get(key) {
    let index = this._hash(key);
    if(this.dataMap[index]) {
        for(let i = 0; i < this.dataMap[index].length; i++) {
            if(this.dataMap[index][i][0] === key) {
                return this.dataMap[index][i][1];
            }
        }
    }
    return undefined;
}
```
- **Purpose**: Retrieves the value associated with the given key.
- It hashes the key, finds the correct array at that index, and returns the corresponding value.
- **Example**: 
  ```javascript
  thisHashTable.get('Asus'); // Returns 800
  ```

### 5. **`keys` Method**

```javascript
keys() {
    let keysList = [];
    for(let i = 0; i < this.dataMap.length; i++) {
        if(this.dataMap[i]) {
            for(let j = 0; j < this.dataMap[i].length; j++) {
                keysList.push(this.dataMap[i][j][0]);
            }
        }
    }
    return keysList;
}
```
- **Purpose**: Returns a list of all the keys stored in the hash table.
- Iterates through each bucket and retrieves all stored keys.
  
### Example Usage:

```javascript
const thisHashTable = new HashTable();
thisHashTable.set('MacBook', 1200);
thisHashTable.set('Asus', 800);
console.log(thisHashTable.get('Asus'));  // Outputs: 800
console.log(thisHashTable.keys());       // Outputs: ['MacBook', 'Asus']
```

