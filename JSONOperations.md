# JSON Operations Quick Reference

## Table of Contents
- [Parsing & Stringifying](#parsing--stringifying)
- [Accessing Data](#accessing-data)
- [Modifying Data](#modifying-data)
- [Array Operations](#array-operations)
- [Object Operations](#object-operations)
- [Common Transformations](#common-transformations)

## Parsing & Stringifying
```javascript
// Converting between JSON strings and JavaScript objects
const jsonString = '{"name": "John", "age": 30}';
const jsonObject = JSON.parse(jsonString);    // String to object
const backToString = JSON.stringify(jsonObject, null, 2);  // Object to formatted string

// Pretty printing JSON with indentation
const prettyJson = JSON.stringify({
    user: { name: "John", age: 30 }
}, null, 2);  // The '2' adds 2 spaces for indentation

// Handling special cases
try {
    // Parse JSON safely with error handling
    const data = JSON.parse(potentiallyInvalidJson);
} catch (error) {
    console.error('Invalid JSON:', error.message);
}
```

## Accessing Data
```javascript
// Different ways to access JSON data
const data = {
    user: {
        name: "John",
        address: {
            city: "New York",
            zip: "10001"
        },
        hobbies: ["reading", "music"]
    }
};

// Direct property access
const name = data.user.name;              // "John"
const city = data.user.address.city;      // "New York"
const firstHobby = data.user.hobbies[0];  // "reading"

// Optional chaining for safe access
const country = data.user.address?.country;  // undefined (no error)

// Destructuring for multiple values
const { name, address: { city: userCity } } = data.user;
```

## Modifying Data
```javascript
// Adding and updating JSON data
const user = {
    name: "John",
    age: 30
};

// Adding new properties
user.email = "john@example.com";          // Add single property
Object.assign(user, {                     // Add multiple properties
    phone: "123-456-7890",
    country: "USA"
});

// Updating nested values
const company = {
    details: {
        employees: [
            { id: 1, name: "John" }
        ]
    }
};

// Update nested property
company.details.employees[0].name = "John Doe";

// Create copy with modifications
const updatedUser = {
    ...user,                              // Spread existing properties
    age: 31,                             // Override specific property
    verified: true                       // Add new property
};
```

## Array Operations
```javascript
// Working with JSON arrays
const users = [
    { id: 1, name: "John", active: true },
    { id: 2, name: "Jane", active: false },
    { id: 3, name: "Bob", active: true }
];

// Finding elements
const john = users.find(user => user.name === "John");
const activeUsers = users.filter(user => user.active);

// Transforming arrays
const userNames = users.map(user => user.name);  // Extract names
const usersByID = users.reduce((acc, user) => {  // Create lookup object
    acc[user.id] = user;
    return acc;
}, {});

// Sorting arrays
users.sort((a, b) => a.name.localeCompare(b.name));  // Sort by name
```

## Object Operations
```javascript
// Common object manipulations
const product = {
    id: "123",
    name: "Laptop",
    price: 999.99,
    specs: {
        cpu: "i7",
        ram: "16GB"
    }
};

// Creating object copies
const shallowCopy = { ...product };                    // Shallow copy
const deepCopy = JSON.parse(JSON.stringify(product));  // Deep copy

// Merging objects
const defaultSettings = { theme: "light", notifications: true };
const userSettings = { theme: "dark" };
const finalSettings = { ...defaultSettings, ...userSettings };

// Removing properties
const { specs, ...productWithoutSpecs } = product;     // Remove specs
delete product.price;                                  // Delete property
```

## Common Transformations
```javascript
// Useful data transformations
const data = [
    { id: 1, name: "John", role: "admin" },
    { id: 2, name: "Jane", role: "user" },
    { id: 3, name: "Bob", role: "user" }
];

// Group by property
const groupByRole = data.reduce((groups, item) => {
    const role = item.role;
    groups[role] = groups[role] ?? [];
    groups[role].push(item);
    return groups;
}, {});

// Convert array to lookup object
const userLookup = data.reduce((lookup, user) => {
    lookup[user.id] = user;
    return lookup;
}, {});

// Flatten nested arrays
const nested = [1, [2, 3], [4, [5, 6]]];
const flat = nested.flat(2);  // [1, 2, 3, 4, 5, 6]

// Transform object structure
const user = {
    firstName: "John",
    lastName: "Doe",
    age: 30
};

const transformed = {
    fullName: `${user.firstName} ${user.lastName}`,
    isAdult: user.age >= 18
};
```
