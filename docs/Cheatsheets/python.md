# Python

## 1. Variables and Data Types

Variables store data. Python figures out the type automatically.

```python
# Numbers
age = 25                    # Integer
price = 19.99              # Float (decimal)

# Text
name = "Alice"             # String
message = 'Hello World'    # Single or double quotes work

# Boolean (True/False)
is_student = True
is_working = False

```

## 2. Basic Operations

```python
# Math
x = 10 + 5        # Addition: 15
y = 10 - 3        # Subtraction: 7
z = 4 * 3         # Multiplication: 12
w = 15 / 3        # Division: 5.0
power = 2 ** 3    # Exponent: 8
remainder = 10 % 3 # Modulo (remainder): 1

# String operations
greeting = "Hello" + " " + "World"  # Concatenation
repeated = "Ha" * 3                  # "HaHaHa"

```

## 3. Printing Output

```python
print("Hello, World!")
print("My age is:", age)
print(f"My name is {name}")  # f-string (formatted string)

```

## 4. Getting Input

```python
name = input("What's your name? ")
age = int(input("What's your age? "))  # Convert to integer

```

## 5. Lists (Collections)

Lists store multiple items in order.

```python
# Creating lists
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]

# Accessing items (indexing starts at 0)
first_fruit = fruits[0]      # "apple"
last_fruit = fruits[-1]       # "cherry"

# Modifying lists
fruits.append("orange")       # Add to end
fruits.insert(1, "mango")     # Insert at position
fruits.remove("banana")       # Remove item
fruits.pop()                  # Remove last item

# List length
count = len(fruits)

```

## 6. Conditional Statements (if/else)

Make decisions in your code.

```python
age = 18

if age >= 18:
    print("You are an adult")
elif age >= 13:
    print("You are a teenager")
else:
    print("You are a child")

# Comparison operators: ==, !=, <, >, <=, >=
# Logical operators: and, or, not

```

## 7. Loops

Repeat actions multiple times.

```python
# For loop - iterate over a sequence
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Loop with range
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 6):     # 1, 2, 3, 4, 5
    print(i)

# While loop - repeat while condition is true
count = 0
while count < 5:
    print(count)
    count += 1

```

## 8. Functions

Reusable blocks of code.

```python
# Define a function
def greet(name):
    print(f"Hello, {name}!")

# Call the function
greet("Alice")

# Function with return value
def add(a, b):
    return a + b

result = add(5, 3)  # result = 8

# Function with default parameter
def greet_with_title(name, title="Mr."):
    print(f"Hello, {title} {name}")

```

## 9. Dictionaries

Store key-value pairs.

```python
# Creating a dictionary
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

# Accessing values
print(person["name"])      # "Alice"
print(person.get("age"))   # 25

# Adding/modifying
person["job"] = "Engineer"
person["age"] = 26

# Looping through dictionary
for key, value in person.items():
    print(f"{key}: {value}")
```

## 10. Common String Methods

```python
text = "Hello World"

text.lower()           # "hello world"
text.upper()           # "HELLO WORLD"
text.replace("World", "Python")  # "Hello Python"
text.split()           # ["Hello", "World"]
text.startswith("Hello")  # True
len(text)              # 11

```

## Practice Exercise

Try this simple program:

```python
# Simple calculator
def calculator():
    num1 = float(input("Enter first number: "))
    num2 = float(input("Enter second number: "))
    operation = input("Enter operation (+, -, *, /): ")

    if operation == "+":
        result = num1 + num2
    elif operation == "-":
        result = num1 - num2
    elif operation == "*":
        result = num1 * num2
    elif operation == "/":
        result = num1 / num2
    else:
        print("Invalid operation")
        return

    print(f"Result: {result}")

calculator()
```

## Dictionaries

```tsx
# Python Dictionary Operations - Complete Guide

# Creating a dictionary
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

print("Original dictionary:")
print(person)
print()

# ========== INSERTING / ADDING NEW ATTRIBUTES ==========

# Method 1: Using bracket notation (most common)
person["job"] = "Engineer"
print("After adding 'job':")
print(person)
print()

# Method 2: Using update() - add single item
person.update({"email": "alice@example.com"})
print("After adding 'email' with update():")
print(person)
print()

# Method 3: Using update() - add multiple items at once
person.update({
    "phone": "555-1234",
    "country": "USA"
})
print("After adding multiple items:")
print(person)
print()

# ========== EDITING / UPDATING EXISTING ATTRIBUTES ==========

# Method 1: Direct assignment (overwrites existing value)
person["age"] = 26
print("After updating 'age':")
print(person)
print()

# Method 2: Using update() for one or more items
person.update({"city": "Los Angeles", "job": "Senior Engineer"})
print("After updating 'city' and 'job':")
print(person)
print()

# Method 3: Conditional update (only if key exists)
if "age" in person:
    person["age"] = person["age"] + 1
print("After incrementing age (if exists):")
print(person)
print()

# ========== DELETING / REMOVING ATTRIBUTES ==========

# Method 1: Using del keyword
del person["phone"]
print("After deleting 'phone' with del:")
print(person)
print()

# Method 2: Using pop() - removes and returns the value
removed_country = person.pop("country")
print(f"Removed 'country': {removed_country}")
print("Dictionary after pop():")
print(person)
print()

# Method 3: Using pop() with default value (safe - no error if key doesn't exist)
removed_salary = person.pop("salary", "Key not found")
print(f"Tried to remove 'salary': {removed_salary}")
print("Dictionary unchanged (key didn't exist):")
print(person)
print()

# Method 4: Using popitem() - removes and returns last inserted key-value pair
last_item = person.popitem()
print(f"Removed last item: {last_item}")
print("Dictionary after popitem():")
print(person)
print()

# Method 5: Clear all items
backup = person.copy()  # Save a copy first
person.clear()
print("After clear():")
print(person)
print()

# Restore from backup for more examples
person = backup
print("Restored dictionary:")
print(person)
print()

# ========== SAFE OPERATIONS (AVOID ERRORS) ==========

# Check if key exists before accessing
if "name" in person:
    print(f"Name: {person['name']}")

# Get with default value (won't error if key doesn't exist)
salary = person.get("salary", "Not specified")
print(f"Salary: {salary}")
print()

# Set default value if key doesn't exist (doesn't overwrite if exists)
person.setdefault("hobby", "Reading")
person.setdefault("name", "Bob")  # Won't change because 'name' already exists
print("After setdefault():")
print(person)
print()

# ========== PRACTICAL EXAMPLES ==========

print("=" * 50)
print("PRACTICAL EXAMPLES")
print("=" * 50)

# Example 1: User profile management
user_profile = {"username": "john_doe", "email": "john@example.com"}

# Add new information
user_profile["bio"] = "Software developer"
user_profile["followers"] = 150

# Update existing info
user_profile["followers"] = 151

# Remove sensitive data
user_profile.pop("email", None)  # Safe removal

print("\nUser Profile:")
print(user_profile)

# Example 2: Shopping cart
cart = {
    "apple": 3,
    "banana": 5,
    "orange": 2
}

print("\nShopping Cart:")
print(cart)

# Add new item
cart["grape"] = 10

# Update quantity
cart["apple"] = cart.get("apple", 0) + 2  # Add 2 more apples

# Remove item
cart.pop("banana")

print("Updated Cart:")
print(cart)

# Example 3: Bulk operations
inventory = {"laptop": 10, "mouse": 50}

# Add multiple items
new_items = {"keyboard": 30, "monitor": 15, "cable": 100}
inventory.update(new_items)

# Update multiple items
updates = {"laptop": 8, "mouse": 45}
inventory.update(updates)

print("\nInventory:")
print(inventory)

# ========== KEY POINTS SUMMARY ==========
print("\n" + "=" * 50)
print("KEY POINTS")
print("=" * 50)
print("""
INSERT:
  - dict[key] = value          # Simplest way
  - dict.update({key: value})  # Single or multiple

EDIT:
  - dict[key] = new_value      # Direct overwrite
  - dict.update({key: value})  # Update one or more

DELETE:
  - del dict[key]              # Raises error if key doesn't exist
  - dict.pop(key)              # Returns value, error if not found
  - dict.pop(key, default)     # Safe, returns default if not found
  - dict.popitem()             # Removes last item
  - dict.clear()               # Removes all items

SAFE OPERATIONS:
  - key in dict                # Check if exists
  - dict.get(key, default)     # Get with fallback
  - dict.setdefault(key, val)  # Set only if doesn't exist
""")
```