# TypeScript

## Basic Types

```tsx
// Primitive types
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;

// Any (avoid when possible)
let anything: any = "could be anything";

// Arrays
let numbers: number[] = [1, 2, 3];
let strings: Array<string> = ["a", "b", "c"];

// Tuple
let tuple: [string, number] = ["hello", 42];

// Union types
let id: string | number = "abc123";

// Literal types
let direction: "left" | "right" | "up" | "down" = "left";

// Type aliases
type User = {
  name: string;
  age: number;
};

// Interfaces
interface Person {
  name: string;
  age: number;
  email?: string; // Optional property
}

// Function types
function add(a: number, b: number): number {
  return a + b;
}

const multiply = (a: number, b: number): number => a * b;

// Void (no return value)
function log(message: string): void {
  console.log(message);
}

```

## Records

```tsx
// 1. Simple Record with string keys and number values
const ages: Record<string, number> = {
  'Alice': 25,
  'Bob': 30,
  'Charlie': 35
};

type Status = 'active' | 'inactive' | 'pending';
const userStatus: Record<string, Status> = {
  'user1': 'active',
  'user2': 'inactive'
};

// 1. Adding entries
userStatus['user3'] = 'pending';

// 2. Updating entries
userStatus['user1'] = 'inactive';

// 3. Reading entries
const status = userStatus['user1']; // Type: Status
console.log(status); // 'inactive'

// 4. Checking if key exists
if ('user1' in userStatus) {
  console.log('User1 exists');
}

// 5. Deleting entries
delete userStatus['user2'];

// 6. Getting all keys
const keys = Object.keys(userStatus); // string[]

// 7. Getting all values
const values = Object.values(userStatus); // Status[]

// 8. Getting entries (key-value pairs)
const entries = Object.entries(userStatus); // [string, Status][]

// 9. Iterating over Record
for (const [key, value] of Object.entries(userStatus)) {
  console.log(`${key}: ${value}`);
}

// 10. Filtering Record
const activeUsers = Object.fromEntries(
  Object.entries(userStatus).filter(([_, status]) => status === 'active')
) as Record<string, Status>;

// 11. Mapping Record values
const uppercaseStatus = Object.fromEntries(
  Object.entries(userStatus).map(([key, value]) => [key, value.toUpperCase()])
) as Record<string, string>;
```

## String Manipulation

```tsx
const str = "Hello World";

// Length
str.length; // 11

// Access characters
str[0]; // "H"
str.charAt(0); // "H"

// Case conversion
str.toLowerCase(); // "hello world"
str.toUpperCase(); // "HELLO WORLD"

// Search
str.includes("World"); // true
str.startsWith("Hello"); // true
str.endsWith("World"); // true
str.indexOf("World"); // 6
str.lastIndexOf("o"); // 7

// Substring
str.slice(0, 5); // "Hello"
str.substring(0, 5); // "Hello"
str.substr(0, 5); // "Hello" (deprecated)

// Replace
str.replace("World", "TypeScript"); // "Hello TypeScript"
str.replaceAll("o", "0"); // "Hell0 W0rld"

// Split
str.split(" "); // ["Hello", "World"]

// Trim
"  hello  ".trim(); // "hello"
"  hello  ".trimStart(); // "hello  "
"  hello  ".trimEnd(); // "  hello"

// Repeat
"ha".repeat(3); // "hahaha"

// Template literals
const name = "Alice";
const greeting = `Hello, ${name}!`; // "Hello, Alice!"

// Multi-line strings
const multiline = `
  Line 1
  Line 2
`;

// Iterate through string
for (const char of str) {
  console.log(char);
}

// Convert to array
[...str]; // ["H", "e", "l", "l", "o", " ", "W", "o", "r", "l", "d"]
str.split(""); // Same as above

```

## Array Manipulation

```tsx
let arr: number[] = [1, 2, 3, 4, 5];

// Length
arr.length; // 5

// Access elements
arr[0]; // 1
arr.at(-1); // 5 (last element)

// Add elements
arr.push(6); // Add to end, returns new length
arr.unshift(0); // Add to start, returns new length

// Remove elements
arr.pop(); // Remove from end, returns removed element
arr.shift(); // Remove from start, returns removed element
arr.splice(2, 1); // Remove 1 element at index 2

// Insert elements
arr.splice(2, 0, 99); // Insert 99 at index 2

// Replace elements
arr.splice(2, 1, 99); // Replace 1 element at index 2 with 99

// Check if contains
arr.includes(3); // true
arr.indexOf(3); // 2 (index of first occurrence)
arr.lastIndexOf(3); // 2 (index of last occurrence)

// Find elements
arr.find(x => x > 3); // 4 (first element that matches)
arr.findIndex(x => x > 3); // 3 (index of first match)

// Filter
arr.filter(x => x > 3); // [4, 5]

// Map (transform)
arr.map(x => x * 2); // [2, 4, 6, 8, 10]

// Reduce
arr.reduce((sum, x) => sum + x, 0); // 15 (sum of all elements)

// Sort
arr.sort((a, b) => a - b); // Ascending
arr.sort((a, b) => b - a); // Descending

// Reverse
arr.reverse(); // [5, 4, 3, 2, 1]

// Slice (extract portion)
arr.slice(1, 3); // [2, 3] (from index 1 to 3, excluding 3)

// Concatenate
arr.concat([6, 7]); // [1, 2, 3, 4, 5, 6, 7]
[...arr, 6, 7]; // Same as above

// Check if every/some element matches
arr.every(x => x > 0); // true
arr.some(x => x > 4); // true

// Iterate
for (const item of arr) {
  console.log(item);
}

arr.forEach((item, index) => {
  console.log(item, index);
});

// Join to string
arr.join(", "); // "1, 2, 3, 4, 5"

// Flatten arrays
const nested = [[1, 2], [3, 4]];
nested.flat(); // [1, 2, 3, 4]

// Remove duplicates
const unique = [...new Set(arr)];

// Clear array
arr.length = 0; // Clears the array
arr = []; // Creates new array

```

## Set Manipulation

```tsx
// Create Set
const set = new Set<string>();
const set2 = new Set(["apple", "banana", "orange"]);

// Add elements (duplicates ignored)
set.add("apple");
set.add("banana");
set.add("apple"); // Won't be added

// Check if exists
set.has("apple"); // true

// Remove element
set.delete("banana"); // Returns true if deleted

// Clear all
set.clear();

// Size
set.size; // Number of elements

// Iterate
for (const item of set) {
  console.log(item);
}

set.forEach(item => {
  console.log(item);
});

// Convert to Array
const arr = [...set];
const arr2 = Array.from(set);

// Set operations
const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);

// Union
const union = new Set([...setA, ...setB]); // {1, 2, 3, 4, 5}

// Intersection
const intersection = new Set([...setA].filter(x => setB.has(x))); // {3}

// Difference
const difference = new Set([...setA].filter(x => !setB.has(x))); // {1, 2}

```

## Map Manipulation

```tsx
// Create Map
const map = new Map<string, number>();
const map2 = new Map([
  ["apple", 1],
  ["banana", 2],
  ["orange", 3]
]);

// Add/Update entry
map.set("key", 100);
map.set("key", 200); // Overwrites

// Get value
map.get("key"); // 200 (or undefined if not found)

// Check if key exists
map.has("key"); // true

// Delete entry
map.delete("key"); // Returns true if deleted

// Clear all
map.clear();

// Size
map.size; // Number of entries

// Iterate over entries
for (const [key, value] of map) {
  console.log(key, value);
}

map.forEach((value, key) => {
  console.log(key, value);
});

// Iterate over keys
for (const key of map.keys()) {
  console.log(key);
}

// Iterate over values
for (const value of map.values()) {
  console.log(value);
}

// Convert to Array
const entries = [...map]; // [[key1, val1], [key2, val2]]
const keys = [...map.keys()];
const values = [...map.values()];

// Create Map from Object
const obj = { a: 1, b: 2 };
const mapFromObj = new Map(Object.entries(obj));

// Create Object from Map
const objFromMap = Object.fromEntries(map);

```

## Object Manipulation

```tsx
const obj = {
  name: "John",
  age: 30,
  city: "New York"
};

// Access properties
obj.name; // "John"
obj["name"]; // "John"

// Add/Update property
obj.email = "john@example.com";
obj["phone"] = "123-456-7890";

// Delete property
delete obj.city;

// Check if property exists
"name" in obj; // true
obj.hasOwnProperty("name"); // true

// Get keys
Object.keys(obj); // ["name", "age", "email", "phone"]

// Get values
Object.values(obj); // ["John", 30, "john@example.com", "123-456-7890"]

// Get entries
Object.entries(obj); // [["name", "John"], ["age", 30], ...]

// Iterate
for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}

// Copy/Clone (shallow)
const copy = { ...obj };
const copy2 = Object.assign({}, obj);

// Merge objects
const merged = { ...obj, city: "Boston", country: "USA" };

// Freeze (make immutable)
Object.freeze(obj);

// Seal (prevent add/delete, allow modification)
Object.seal(obj);

```

## Control Flow

```tsx
// If-else
if (age > 18) {
  console.log("Adult");
} else if (age > 12) {
  console.log("Teen");
} else {
  console.log("Child");
}

// Ternary operator
const status = age > 18 ? "Adult" : "Minor";

// Switch
switch (day) {
  case "Monday":
    console.log("Start of week");
    break;
  case "Friday":
    console.log("End of week");
    break;
  default:
    console.log("Midweek");
}

// For loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// For...of (iterate values)
for (const item of arr) {
  console.log(item);
}

// For...in (iterate keys/indices)
for (const key in obj) {
  console.log(key, obj[key]);
}

// While
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}

// Do-while
do {
  console.log(i);
  i++;
} while (i < 5);

// Break and continue
for (let i = 0; i < 10; i++) {
  if (i === 3) continue; // Skip 3
  if (i === 7) break; // Stop at 7
  console.log(i);
}

```

## Common Patterns

```tsx
// Nullish coalescing
const value = null ?? "default"; // "default"
const value2 = 0 ?? "default"; // 0 (only null/undefined use default)

// Optional chaining
const city = user?.address?.city; // Safe navigation

// Type assertion
const input = document.getElementById("myInput") as HTMLInputElement;

// Type guards
function isString(value: any): value is string {
  return typeof value === "string";
}

// Generics
function identity<T>(arg: T): T {
  return arg;
}

// Async/Await
async function fetchData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error(error);
  }
}

// Promise
const promise = new Promise<string>((resolve, reject) => {
  setTimeout(() => resolve("Done!"), 1000);
});

promise.then(result => console.log(result));

// Destructuring
const { name, age } = obj;
const [first, second] = arr;

// Spread operator
const newArr = [...arr, 6, 7];
const newObj = { ...obj, email: "new@example.com" };

// Rest parameters
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}

```

## Useful Utility Types

```tsx
// Partial (all properties optional)
type PartialUser = Partial<User>;

// Required (all properties required)
type RequiredUser = Required<User>;

// Readonly (all properties readonly)
type ReadonlyUser = Readonly<User>;

// Pick (select specific properties)
type UserNameOnly = Pick<User, "name">;

// Omit (exclude specific properties)
type UserWithoutAge = Omit<User, "age">;

// Record (create object type)
type UserRoles = Record<string, string>;

```