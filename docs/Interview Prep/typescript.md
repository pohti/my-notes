# TypeScript

## Core Concepts

### 1. Difference Between `interface` and `type`

`interface` and `type` are both used to define the shape of objects in TypeScript, but they have key differences:

* **`interface`**
  Best for defining the shape of an object and preferred for **public APIs or class implementations**.

  * Supports **declaration merging** — multiple interface declarations with the same name automatically merge into one.
* **`type`**
  More versatile. It can define:

  * Object shapes
  * **Union types (`|`)**
  * **Intersection types (`&`)**
  * **Aliases** for primitives or tuples
    Unlike `interface`, a `type` cannot be merged.

**Rule of thumb:**
Use `interface` for objects and `type` when you need features like unions or intersections.

---

### 2. Type Inference

TypeScript automatically **infers types** based on values or context, reducing the need for explicit annotations.

#### Variable Declarations

```ts
let message = "hello"; // inferred as string
```

If no initial value is given, the type defaults to `any`.

#### Function Return Types

```ts
function add(a: number, b: number) {
  return a + b; // inferred as number
}
```

#### Contextual Typing

Type inference based on surrounding context:

```ts
const numbers = [1, 2, 3];
const doubled = numbers.map(num => num * 2); // num inferred as number
```

#### Control Flow Analysis

TypeScript narrows types based on conditions:

```ts
function processValue(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase()); // value: string
  } else {
    console.log(value.toFixed(2)); // value: number
  }
}
```

---

### 3. `any`, `unknown`, and `never`

| Type          | Description                        | Behavior                          | Use Case                                |
| ------------- | ---------------------------------- | --------------------------------- | --------------------------------------- |
| **`any`**     | Disables type checking             | Can access any property or method | Use only when necessary (unsafe)        |
| **`unknown`** | Type-safe counterpart of `any`     | Must narrow type before using     | When type is uncertain (e.g., API data) |
| **`never`**   | Represents values that never occur | Used in unreachable code          | Enforce exhaustiveness in unions        |

#### Examples

```ts
let a: any = 'hello';
a = 123;
a.doSomething(); // No compile-time error

let b: unknown = 'hello';
// b.toLowerCase(); // Error
if (typeof b === 'string') console.log(b.toLowerCase());

function fail(message: string): never {
  throw new Error(message);
}
```

---

### 4. Type Guards

**Type guards** help TypeScript narrow down variable types inside conditional blocks.

#### Common Type Guards

* `typeof` — for primitives

  ```ts
  if (typeof x === "string") { ... }
  ```
* `instanceof` — for class instances

  ```ts
  if (obj instanceof MyClass) { ... }
  ```
* Property checks — for structural types

  ```ts
  if ('property' in obj) { ... }
  ```

#### Example

```ts
type APIResult = { message: string } | { error: string };

function processResult(result: APIResult) {
  if ('message' in result) {
    console.log(result.message);
  } else {
    console.log(result.error);
  }
}
```

---

### 5. Handling `null` and `undefined`

#### 1. Strict Null Checks

Enable `"strictNullChecks": true` in `tsconfig.json`.

```ts
let name: string = "Alice";
// name = null; // Error

let optionalName: string | null = "Bob";
optionalName = null; // Allowed
```

#### 2. Type-Narrowing Techniques

* **Conditional checks**

  ```ts
  function greet(name: string | null) {
    if (name !== null) console.log(`Hello, ${name.toUpperCase()}`);
  }
  ```
* **Non-null assertion (`!`)**

  ```ts
  console.log(name!.toUpperCase());
  ```
* **Optional chaining (`?.`)**

  ```ts
  const user = null;
  const name = user?.profile?.name;
  ```

---

## Advanced Concepts

### 1. Generics

Generics let you create **reusable components** that work with various data types.

#### Example

```ts
function identity<T>(value: T): T {
  return value;
}

interface ApiResponse<T> {
  data: T;
  success: boolean;
}
```

---

### 2. Declaration Merging

TypeScript can **merge multiple declarations** into a single definition.

#### Example

```ts
interface User {
  name: string;
}

interface User {
  age: number;
}

// Merged result: { name: string; age: number }
const user: User = { name: "Alice", age: 30 };
```

---

### 3. Utility Types

Utility types help transform existing types.

#### `Partial<T>`

Makes all properties optional.

```ts
interface User { name: string; age: number; }
type PartialUser = Partial<User>; // { name?: string; age?: number; }
```

#### `Readonly<T>`

Makes all properties immutable.

```ts
interface Product { id: number; name: string; }
type ImmutableProduct = Readonly<Product>;
```

#### `Pick<T, K>`

Selects specific properties.

```ts
interface Post { id: number; title: string; author: string; content: string; }
type PostPreview = Pick<Post, "id" | "title" | "author">;
```

#### `Omit<T, K>`

Removes specific properties.

```ts
interface Car { make: string; model: string; year: number; color: string; }
type CarWithoutColor = Omit<Car, "color">;
```

---

### 4. Decorators

Decorators are **functions that modify classes, methods, or properties** by adding metadata or behavior.

> ⚠️ Decorators are experimental in TypeScript and not yet a finalized JavaScript feature.

#### Example

```ts
function Log(target: any, methodName: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function(...args: any[]) {
    console.log(`Calling ${methodName} with`, args);
    const result = original.apply(this, args);
    console.log(`${methodName} returned`, result);
    return result;
  };
}

class Calculator {
  @Log
  add(a: number, b: number) {
    return a + b;
  }
}
```

#### Common Use Cases

* **Dependency Injection** → Angular (`@Injectable()`)
* **Validation** → Custom decorators for form checks
* **Routing** → NestJS (`@Controller()`, `@Get()`)
* **ORM Mapping** → TypeORM (`@Entity()`, `@Column()`)

---

### 5. `enum` vs Union of String Literals

| Feature     | `enum`                      | String Literal Union             |         |
| ----------- | --------------------------- | -------------------------------- | ------- |
| Definition  | Groups named constants      | Defines a union of string values |         |
| Example     | `enum Role { Admin, User }` | `type Role = "Admin"             | "User"` |
| Type-safety | Medium                      | Stronger (literal checks)        |         |
| Flexibility | Less flexible               | More composable                  |         |
| Verbosity   | More verbose                | Cleaner and simpler              |         |

**Recommendation:**
Use **string literal unions** for flexibility and type safety unless you specifically need `enum` semantics.

---

Would you like me to format it with **syntax highlighting and section navigation (Markdown table of contents)** for easier reading or export to a `.md` file?
