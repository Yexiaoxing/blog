---
layout: draft
title: Sorting Array in JavaScript
date: 2022-04-21 00:25:35
categories:
  - Algorithms
tags:
  - Frontend
  - JavaScript
  - Array
---

Sorting an array in JavaScript sounds like an easy job, right? Just use `Array.prototype.sort` - wait, is it correct?

## What's wrong with `Array.prototype.sort`?

### Confusing default behaviour
Before we continue, try it in your browser or a node.js console.

```javascript
const array = [1, 42, 5, -3];
array.sort();
console.log(array);
```

The result is amazing - `[-3, 1, 42, 5]`. What's wrong here? If you check [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort), you will find that the sort method accepts an optional compare function. The compare function is called for each pair of elements and the result is used to sort the array. If the function is omitted, the default sorting behaviour is:

- Convert the elements to strings
- Sort the array according to each character's Unicode code point value.

So, the default behavior is for sorting string, not number. 

If you want to use this method, you need to instead provide a compare function for number,

```javascript
const array = [1, 42, 5, -3];
array.sort((a, b) => a - b);
console.log(array);
```

It becomes `[-3, 1, 5, 42]`. Amazing.

### Stable sorting?

### Performance?

### Alternative - `TypedArray.prototype.sort`
