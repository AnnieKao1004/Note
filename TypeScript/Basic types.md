# TyeScript Basic types
**Type annotation** & **type inference** (大多數情況下，不須做型別註解，TypeScript 會自動做型別推論)

## Primitive Types
JavaScript primitive:
- `string`
- `number`
- `boolean`

## Arrays
e.g., The type of `[1, 2, 3]` => `number[]` or `Array<number>`

The syntax also works for `string[]`

## any
不特別定義 type

`noImplicitAny: true` in `tsconfig.json` will raise error with an `any` type

## Type annotation
- variables
  ```typescript
  let myName: string = "Annie";
  ```
- function parameters & return
  ```typescript
  function greet(name: string) {
    console.log(`Hello, ${name.toUpperCase()}!!`);
  }
  ```
  ```typescript
  function getFavoriteNumber(): number {
    return 26;
  }
  ```
  > 什麼情況下即使 function parameter 沒有宣告型別, TypeScript 也知道 parameter 的 type ?
   
  ```typescript
  const names = ["Alice", "Bob", "Eve"];
  // names: string[]


  names.forEach(function (s) {
    // Contextual typing: TypeScript 會自動判斷 typeof parameter s is string
    console.log(s.toUpperCase());
  }
  ```
  
## Obejct
若 parameter object property 沒有宣告型別，TypeScript 會自動推論為 `any`
```typescript
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```
### Optional properties ( `?` )
```typescript
function printName(obj: { firstL string; last?: string }) {
  console.log(obj.last?.toUpperCase()); // optional chaining
}

// Both OK
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
```

## Union Types ( `|` )
Build new types with existing one
- 使用在 union type 上的 method 須為每一種 type 都可以使用的 method
- 否則需要做 *Narrowing* ，用 `if {} else {}` 判斷式來根據不同 type 使用不同 method
```typescript
function printId(id: number | string) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}
```

## Type Aliases ( `type` )
自定義 type name (use the same type more than once and refer to it by a single name)
```typescript
type Point = {
  x: number;
  y: number;
};

// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

```typescript
type ID = number | string;
```

extending a type
```typescript
type Animal = {
  name: string
}

type Bear = Animal & { 
  honey: Boolean 
}
```

## Interfaces ( `interface` )
也是自定義 type name
```typescript
interface Point {
  x: number;
  y: number;
}
```
extending an interface
```typescript
interface Animal {
  name: string
}

interface Bear extends Animal { 
  honey: Boolean 
}
```

Add new field to existing interface (type 不行這樣寫，會 `Error: Duplicate identifier`)
```typescript
interface Animal {
  name: string
}

interface Animal { 
 size: string 
}
```
