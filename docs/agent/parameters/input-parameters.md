# Input Parameters

Input parameters define the data that your tasks will process. Understanding how to configure them correctly is crucial for building effective workflows.

## Parameter Types

=== "Basic Types"
    | Type | Description | Example |
    |------|-------------|---------|
    | STRING | Text data | "Hello World" |
    | INTEGER | Whole numbers | 42 |
    | FLOAT | Decimal numbers | 3.14 |
    | BOOLEAN | True/false values | true |

=== "Complex Types"
    | Type | Description | Example | Restrictions |
    |------|-------------|---------|--------------|
    | ARRAY | List of items | `["item1", "item2"]` | Maximum 2D arrays allowed |
    | OBJECT | Structured data | `{"name": "John"}` | Properties must be basic types |
    | ENUM | Limited choices | `"APPROVED"` | Fixed set of values |

=== "Special Types"
    | Type | Description | Use Case |
    |------|-------------|----------|
    | OUTPUT_ARTIFACT | Reference to task output | Inter-task data flow |
    | KNOWLEDGE_ARTIFACT | Knowledge base reference | Accessing stored knowledge |

## Complex Type Configuration

=== "User Guide"
    ### Array Parameters
    ⚠️ **Important Rules**:
    - Maximum 2D arrays allowed
    - Inner array elements can be basic types or objects
    - No 3D or deeper arrays

    ### Object Parameters
    ⚠️ **Important Rules**:
    - Properties must use only:
      - STRING
      - INTEGER
      - FLOAT
      - BOOLEAN
    - No nested objects allowed
    - Each property needs name, description, type, and required fields

=== "Developer Reference"
    ### Array Structure
    ```json
    // 1D Array
    {
      "type": "array",
      "items": {
        "type": "string",  // Any basic type
        "items": null,
        "properties": null
      },
      "properties": null
    }

    // 2D Array (Maximum depth)
    {
      "type": "array",
      "items": {
        "type": "array",
        "items": {
          "type": "string",  // Any basic type
          "items": null,
          "properties": null
        },
        "properties": null
      },
      "properties": null
    }

    // Array of Objects
    {
      "type": "array",
      "items": {
        "type": "object",
        "items": null,
        "properties": [
          {
            "name": "field",
            "description": "Description",
            "required": false,
            "type": "string"  // Must be basic type
          }
        ]
      },
      "properties": null
    }
    ```

    ### Object Structure
    ```json
    {
      "type": "object",
      "items": null,
      "properties": [
        {
          "name": "fieldName",
          "description": "Field description",
          "required": false,
          "type": "string"  // Must be one of: "string", "integer", "float", "boolean"
        }
      ]
    }
    ```

## Parameter Sources

=== "User Guide"
    ### Available Sources
    1. **Task Config**
       - Set during task setup
       - Fixed values
       - Template values

    2. **System Generated**
       - Created during execution
       - Dynamic values
       - No manual input needed

    3. **Human Input**
       - Provided during workflow
       - User interaction required
       - Interactive forms

=== "Developer Reference"
    ```json
    // Task Config
    {
      "source": "task_config",
      "value": "configured_value"
    }

    // System Generated
    {
      "source": "system_generated"
    }

    // Human Input
    {
      "source": "human_input"
    }
    ```

## Data Sources

=== "User Guide"
    ### Memory Storage
    - Default storage method
    - For regular data volumes
    - No special configuration needed

    ### Data Lake Storage
    - For large datasets
    - Persistent storage
    - Requires data_lake_id

=== "Developer Reference"
    ```json
    // Memory Storage
    {
      "data_source": "MEMORY"
    }

    // Data Lake Storage
    {
      "data_source": "DATA_LAKE",
      "data_lake_id": "12345"
    }
    ```

## Configuration Examples

=== "Basic Examples"
    ### String Parameter
    ```json
    {
      "name": "username",
      "description": "User's login name",
      "type": "STRING",
      "required": true,
      "source": "task_config",
      "value": "john_doe"
    }
    ```

    ### Number Parameter
    ```json
    {
      "name": "age",
      "description": "User's age",
      "type": "INTEGER",
      "required": true,
      "source": "task_config",
      "value": 25
    }
    ```

=== "Complex Examples"
    ### 2D Array Example
    ```json
    {
      "name": "matrix",
      "type": "array",
      "items": {
        "type": "array",
        "items": {
          "type": "integer",
          "items": null,
          "properties": null
        },
        "properties": null
      },
      "properties": null,
      "description": "2D matrix of values",
      "required": true
    }
    ```

    ### Object Example
    ```json
    {
      "name": "userProfile",
      "type": "object",
      "items": null,
      "properties": [
        {
          "name": "firstName",
          "type": "string",
          "description": "User's first name",
          "required": true
        },
        {
          "name": "age",
          "type": "integer",
          "description": "User's age",
          "required": false
        }
      ]
    }
    ```

## Validation Rules

=== "Type Rules"
    1. **Object Properties**
       - Only basic types allowed (string, integer, float, boolean)
       - All fields required (name, description, type, required)
       - No nested objects

    2. **Array Items**
       - Maximum 2D arrays
       - Basic types or objects for elements
       - Proper null values for unused fields

    3. **Type Consistency**
       - Must follow exact schema
       - No additional fields
       - Proper null handling

=== "Invalid Examples"
    ```json
    // ❌ 3D Array (Not Allowed)
    {
      "type": "array",
      "items": {
        "type": "array",
        "items": {
          "type": "array",  // Third level not allowed
          "items": {
            "type": "string"
          }
        }
      }
    }

    // ❌ Nested Object (Not Allowed)
    {
      "type": "object",
      "properties": [
        {
          "name": "address",
          "type": "object",  // Cannot use object type in properties
          "required": true
        }
      ]
    }
    ```

## Best Practices

1. **Naming Conventions**
   - Use clear, descriptive names
   - Follow consistent conventions
   - Indicate purpose in name

2. **Documentation**
   - Be specific and clear
   - Include format requirements
   - Note any dependencies

3. **Type Selection**
   - Use simplest type possible
   - Consider data flow requirements
   - Plan for scale

4. **Validation**
   - Set appropriate required flags
   - Include format constraints
   - Consider edge cases

## Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| Type mismatch | Verify data type matches configuration |
| Missing required value | Check source configuration |
| Invalid format | Review validation rules |
| Data source error | Verify data lake configuration |
| Invalid object property | Use only allowed basic types |
| Array depth exceeded | Restructure to 2D maximum |

## Next Steps

- Learn about [Output Parameters](output-parameters.md)
- Understand [Validation Rules](../guides/validation-rules.md)
- Check [Task Examples](../examples/task-examples.md)
