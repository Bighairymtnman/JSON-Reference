# JSON Basics Quick Reference

## Table of Contents
- [JSON Syntax](#json-syntax)
- [Data Types](#data-types)
- [Objects](#objects)
- [Arrays](#arrays)
- [Nesting](#nesting)
- [Common Patterns](#common-patterns)

## JSON Syntax
```javascript
// Basic JSON structure - Every JSON starts with either {} or []
{
    "string": "Hello, JSON!",  // Text must use double quotes
    "number": 42,             // Numbers don't need quotes
    "boolean": true,          // true or false, no quotes
    "null": null             // null represents no value
}

// Important: JSON requires double quotes for property names
{
    "valid": "Using double quotes",    // Correct
    "numbers": 123,                    // Numbers are fine without quotes
    "noQuotes": true                   // Booleans never use quotes
}
```

## Data Types
```javascript
// JSON supports six main data types
{
    "strings": "Text data",            // For text values
    "numbers": 42,                     // Whole numbers
    "integers": 123,                   // Another number example
    "decimals": 3.14,                 // Decimal numbers
    "booleans": true,                 // true or false
    "nullValue": null                 // Represents no value
}

// Numbers can be written in several ways
{
    "integer": 42,                    // Simple whole number
    "negative": -17,                  // Negative numbers
    "decimal": 3.14,                 // Decimal numbers
    "scientific": 1.2e-4             // Scientific notation
}
```

## Objects
```javascript
// Objects group related data together
{
    "user": {                         // Objects use curly braces
        "name": "John Doe",           // Each property needs a name
        "age": 30,
        "email": "john@example.com",
        "isActive": true
    },
    "settings": {                     // Objects can be nested
        "theme": "dark",
        "notifications": true,
        "language": "en"
    }
}
```

## Arrays
```javascript
// Arrays store lists of values
{
    "numbers": [1, 2, 3, 4, 5],              // Array of numbers
    "strings": ["apple", "banana", "orange"], // Array of strings
    "mixed": [1, "two", true, null],         // Arrays can mix types
    "users": [                               // Array of objects
        {"id": 1, "name": "John"},
        {"id": 2, "name": "Jane"}
    ]
}
```

## Nesting
```javascript
// Example of deeply nested JSON structure
{
    "company": {                              // Main object
        "name": "Tech Corp",
        "employees": [                        // Array of employee objects
            {
                "id": 1,
                "name": "John Doe",
                "department": {               // Nested object
                    "id": "IT",
                    "location": "Floor 3"
                },
                "skills": ["JavaScript", "Python", "SQL"]  // Array
            }
        ],
        "locations": {                       // Nested object structure
            "headquarters": {
                "city": "San Francisco",
                "address": {
                    "street": "123 Tech St",
                    "zip": "94105"
                }
            },
            "branches": [                    // Array within nested structure
                "New York",
                "London",
                "Tokyo"
            ]
        }
    }
}
```

## Common Patterns
```javascript
// Standard API Response Pattern - Used when receiving data from servers
{
    "status": "success",              // Response status
    "code": 200,                     // HTTP status code
    "data": {                        // Main data payload
        "id": 123,
        "content": "Some data"
    },
    "message": "Operation successful" // Human-readable message
}

// Collection Pattern - For lists of items with metadata
{
    "items": [                       // Array of items
        {
            "id": 1,
            "name": "Item 1"
        },
        {
            "id": 2,
            "name": "Item 2"
        }
    ],
    "total": 2,                      // Metadata about the collection
    "page": 1
}

// Configuration Pattern - Common for app settings
{
    "app": {
        "name": "MyApp",
        "version": "1.0.0",
        "settings": {                // Grouped settings
            "theme": "light",
            "language": "en",
            "notifications": true
        },
        "features": {                // Feature flags
            "chat": true,
            "videoCall": false,
            "fileSharing": true
        }
    }
}

// Error Response Pattern - Standard way to return errors
{
    "status": "error",               // Error status
    "code": 400,                    // Error code
    "errors": [                     // Array of specific errors
        {
            "field": "email",
            "message": "Invalid email format"
        },
        {
            "field": "password",
            "message": "Password too short"
        }
    ]
}
```
