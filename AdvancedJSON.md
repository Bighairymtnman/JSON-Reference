# Advanced JSON Concepts

## Table of Contents
- [JSON Schema](#json-schema)
- [Validation Patterns](#validation-patterns)
- [Advanced Transformations](#advanced-transformations)
- [Performance Optimization](#performance-optimization)
- [Security Considerations](#security-considerations)
- [Best Practices](#best-practices)

## JSON Schema
```json
// JSON Schema helps validate data structure and enforce data rules
// Think of it as a blueprint for your JSON data
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "title": "User Profile Schema",
    "type": "object",
    "required": ["id", "name", "email"],    // These fields must exist
    "properties": {
        "id": {
            "type": "integer",              // Must be a whole number
            "description": "Unique identifier"
        },
        "name": {
            "type": "string",
            "minLength": 2,                 // Name must be at least 2 chars
            "maxLength": 100                // And no more than 100
        },
        "email": {
            "type": "string",
            "format": "email"              // Must match email format
        },
        "age": {
            "type": "integer",
            "minimum": 0,                  // Age can't be negative
            "maximum": 120                 // Setting reasonable limits
        },
        "address": {                      // Nested object definition
            "type": "object",
            "properties": {
                "street": { "type": "string" },
                "city": { "type": "string" },
                "country": { "type": "string" }
            }
        }
    }
}
```

## Validation Patterns
```javascript
// Advanced techniques to ensure JSON data is valid and safe

// Custom validation function for user data
function validateUser(user) {
    const errors = [];
    
    // Check required fields first
    if (!user.name) errors.push("Name is required");
    if (!user.email) errors.push("Email is required");
    
    // Email format validation using regex
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (user.email && !emailRegex.test(user.email)) {
        errors.push("Invalid email format");
    }
    
    // Business logic validation
    if (user.age && (user.age < 0 || user.age > 120)) {
        errors.push("Age must be between 0 and 120");
    }
    
    return {
        isValid: errors.length === 0,
        errors                           // Return all errors found
    };
}

// Validates deeply nested objects recursively
function validateNestedData(data, path = '') {
    const errors = [];
    
    Object.entries(data).forEach(([key, value]) => {
        const currentPath = path ? `${path}.${key}` : key;
        
        if (typeof value === 'object' && value !== null) {
            // Dive deeper into nested objects
            errors.push(...validateNestedData(value, currentPath));
        } else if (value === undefined || value === null) {
            // Track missing required values
            errors.push(`${currentPath} is required`);
        }
    });
    
    return errors;
}
```

## Advanced Transformations
```javascript
// Complex ways to transform and manipulate JSON data

// Deeply merges two objects, handling nested properties
function deepMerge(target, source) {
    Object.keys(source).forEach(key => {
        if (source[key] instanceof Object && key in target) {
            // Recursively merge nested objects
            Object.assign(source[key], deepMerge(target[key], source[key]));
        }
    });
    return { ...target, ...source };
}

// Converts nested data into a flat, normalized structure
function normalizeData(data) {
    const entities = {
        users: {},
        posts: {},
        comments: {}
    };
    
    data.forEach(item => {
        // Separate nested data into individual collections
        const { comments, ...post } = item;
        entities.posts[post.id] = post;
        
        comments.forEach(comment => {
            entities.comments[comment.id] = comment;
            entities.users[comment.userId] = comment.user;
        });
    });
    
    return entities;
}

// Custom serializer for handling special data types
const serializer = {
    // Convert special types to JSON-safe format
    serialize(data) {
        return JSON.stringify(data, (key, value) => {
            if (value instanceof Date) {
                // Handle Date objects specially
                return { _type: 'date', value: value.toISOString() };
            }
            return value;
        });
    },
    
    // Convert back from JSON to proper types
    deserialize(json) {
        return JSON.parse(json, (key, value) => {
            if (value && value._type === 'date') {
                // Convert back to Date object
                return new Date(value.value);
            }
            return value;
        });
    }
};
```

## Performance Optimization
```javascript
// Techniques for handling large JSON data efficiently

// Stream large JSON data instead of loading all at once
async function* streamJSON(url) {
    const response = await fetch(url);
    const reader = response.body.getReader();
    const decoder = new TextDecoder();
    let buffer = '';
    
    while (true) {
        const { done, value } = await reader.read();
        if (done) break;
        
        // Process data in chunks as it arrives
        buffer += decoder.decode(value, { stream: true });
        const chunks = buffer.split('\n');
        
        buffer = chunks.pop();
        for (const chunk of chunks) {
            yield JSON.parse(chunk);
        }
    }
}

// Create proxy for lazy loading of large arrays
function createLazyArray(array) {
    return new Proxy(array, {
        get(target, prop) {
            // Only access elements when needed
            console.log(`Accessing: ${prop}`);
            return target[prop];
        }
    });
}

// Parse large JSON files in smaller chunks
function parseJSONInChunks(jsonString, chunkSize = 1000) {
    const chunks = [];
    let position = 0;
    
    // Break down large JSON into manageable pieces
    while (position < jsonString.length) {
        const chunk = jsonString.slice(position, position + chunkSize);
        chunks.push(JSON.parse(chunk));
        position += chunkSize;
    }
    
    return chunks;
}
```

## Security Considerations
```javascript
// Protect against JSON-related security vulnerabilities

// Clean potentially dangerous JSON input
function sanitizeJSON(input) {
    // Remove characters that could be harmful
    const cleaned = input.replace(/[^\w\s:,{}\[\]".-]/g, '');
    
    try {
        const parsed = JSON.parse(cleaned);
        // Check for potentially malicious content
        if (hasExecutableContent(parsed)) {
            throw new Error('Potentially harmful content detected');
        }
        return parsed;
    } catch (error) {
        throw new Error('Invalid JSON format');
    }
}

// Safely parse JSON while preventing injection attacks
function safeJSONParse(str) {
    try {
        return JSON.parse(str, (key, value) => {
            if (typeof value === 'string') {
                // Remove potential script tags
                if (value.includes('<script>')) {
                    return undefined;
                }
                // Clean HTML content
                return value.replace(/[<>]/g, '');
            }
            return value;
        });
    } catch (error) {
        return null;
    }
}
```

## Best Practices
```javascript
// Advanced patterns for robust JSON handling

// Update nested properties immutably
function updateNestedJSON(obj, path, value) {
    const pathArray = path.split('.');
    return pathArray.reduceRight((val, key, i) => ({
        ...obj,
        [key]: i === pathArray.length - 1 ? value : val
    }), obj);
}

// Utility for reliable type checking
const typeCheckers = {
    isString: value => typeof value === 'string',
    isNumber: value => typeof value === 'number' && !isNaN(value),
    isBoolean: value => typeof value === 'boolean',
    isObject: value => value && typeof value === 'object' && !Array.isArray(value),
    isArray: value => Array.isArray(value)
};

// Safe JSON operations with fallback
function safeJSONOperation(operation, fallback = null) {
    try {
        return operation();
    } catch (error) {
        console.error('JSON operation failed:', error);
        return fallback;
    }
}

// Handle JSON data structure versions
function migrateJSON(data, fromVersion, toVersion) {
    // Define how to upgrade data between versions
    const migrations = {
        '1to2': data => ({
            ...data,
            newField: 'default'
        }),
        '2to3': data => ({
            ...data,
            timestamp: new Date().toISOString()
        })
    };
    
    // Apply migrations sequentially
    let current = { ...data };
    while (fromVersion < toVersion) {
        const migrationKey = `${fromVersion}to${fromVersion + 1}`;
        current = migrations[migrationKey](current);
        fromVersion++;
    }
    
    return current;
}
```
