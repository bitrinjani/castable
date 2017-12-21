[![Build Status](https://travis-ci.org/bitrinjani/castable.svg?branch=master?i=2)](https://travis-ci.org/bitrinjani/castable) [![Coverage Status](https://coveralls.io/repos/github/bitrinjani/castable/badge.svg?branch=master&i=2)](https://coveralls.io/github/bitrinjani/castable?branch=master) [![npm version](https://badge.fury.io/js/%40bitr%2Fcastable.svg)](https://badge.fury.io/js/%40bitr%2Fcastable)

# Castable TypeScript Library
Castable sanitizes dirty external data by casting all properties at run time to the types specified at compile time.

# Why do you need this library?
A lot of web services return numbers **with double-quotes**. If you convert them by JSON.stringify, the double-quoted numbers will be string type!!

```JavaScript
const serverResponse = `{
　　"name": "Milk", 
　　"price": "200", 
　　"tax": "10", 
}`;
const product = JSON.parse(serverResponse);
const sum = product.price + product.tax;
console.log(`sum: ${sum}`); // "200" + "10" = "20010"⛔️
```

TypeScript type annotation can help it? No, TypeScript cannot check such run-time type mismatch. You will get the exactly same result even type annotation is perfect.

That's why I've made this library. Castable can convert those types at run time. All fields will be converted to the annotated types.

```TypeScript
import { cast, Castable } from 'castable';

class Product extends Castable { 
  @cast name: string;
  @cast price: number;
  @cast tax: number;
}

const serverResponse = `{"name": "Milk", "price": "200", "tax": "10"}`;
const product = new Product(JSON.parse(serverResponse));
const sum = product.price + product.tax;
console.log(`sum: ${sum}`); // 200 + 10 = 210👍
```

This library also can convert:
- Nested type
- Array<T>
- Multi-dementional Array<T> 

# Install

```bash
npm install @bitr/castable
```

# Usage
1. Extend Castable
2. Add `@cast` decorator to primitive type field (string, number, boolean)
3. Add `@cast(Date)` decorator to Date type field
4. Add `@cast @element(T)` to Array<T> type field
4. Add `@cast` decorator to nested type
5. Do same to all nested types


