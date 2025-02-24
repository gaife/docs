# Validation Rules

## Overview

This guide covers all validation rules that must be satisfied when creating or updating an agent. These rules ensure the integrity and proper functioning of your workflow.

## General Workflow Validation

### Assignment Rules
1. **Minimum Requirement**
   - At least one assignment rule must exist
   - ```json
     {
       "rule_type": "Human Feedback|Exception",
       "assignment_type": "User|Team",
       "assignee_id": "valid_id"
     }
     ```

### Task Dependencies
1. **Cyclic Dependencies**
   - No circular dependencies allowed between tasks
   ```mermaid
   graph TD
      A[Task A] --> B[Task B]
      B --> C[Task C]
      C -.- x[❌] --> A
   ```

2. **Valid Connections**
   - Tasks must be properly connected
   - Output parameters must match input requirements

## Task-Specific Validation

### 1. AI Task Validation
```json
{
  "name": "required|string",
  "instructions": "required|string",
  "input_parameters": "required|array",
  "expected_output": "required|array"
}
```

### 2. Human Task Validation
```json
{
  "name": "required|string",
  "instructions": "required|string",
  "error_policy": "optional|RAISE|IGNORE|RETRY"
}
```

### 3. App Task Validation
```json
{
  "name": "required|string",
  "instructions": "required|string",
  "provider": "required|string",
  "tool_name": "required|string",
  "tool_id": "required|integer",
  "input_parameters": "required|array",
  "expected_output": "required|array"
}
```

### 4. Coder Task Validation
```json
{
  "name": "required|string",
  "instructions": "required|string",
  "code_artifact_id": "required|integer",
  "input_parameters": "required|array",
  "expected_output": "required|array"
}
```

## Parameter Validation

### Input Parameters

#### 1. Basic Types
```json
{
  "name": "required|string",
  "type": "required|STRING|INTEGER|FLOAT|BOOLEAN",
  "description": "required|string",
  "required": "boolean",
  "source": "required|task_config|system_generated|human_input",
  "value": "conditional|based_on_type"
}
```

#### 2. Array Type
```json
{
  "type": "ARRAY",
  "items": {
    "type": "required|STRING|INTEGER|FLOAT|BOOLEAN|ARRAY|OBJECT",
    "items": "required_for_nested_array",
    "properties": "required_for_object"
  }
}
```

#### 3. Object Type
```json
{
  "type": "OBJECT",
  "properties": [
    {
      "name": "required|string",
      "type": "required|STRING|INTEGER|FLOAT|BOOLEAN",
      "description": "required|string",
      "required": "boolean"
    }
  ]
}
```

### Output Parameters

#### 1. Basic Types
```json
{
  "name": "required|string",
  "type": "required|BOOLEAN|INTEGER|FLOAT|STRING|DATETIME",
  "description": "required|string"
}
```

#### 2. File Types
```json
{
  "type": "required|TEXT|MARKDOWN|HTML|CODE|PDF|CSV|XML|FILE|PPT",
  "description": "required|string"
}
```

## Data Source Validation

### 1. Memory Data Source
- Default data source
- No additional validation required

### 2. Data Lake Source
```json
{
  "data_source": "DATA_LAKE",
  "data_lake_id": "required|string",
  "type": "must_be_ARRAY"
}
```

## Type-Specific Validation

### 1. String Values
- Must be valid string
- Length constraints if specified

### 2. Numeric Values
```python
# Integer
if type == "INTEGER":
    must be valid integer or convertible string

# Float
if type == "FLOAT":
    must be valid float or convertible string/integer
```

### 3. Boolean Values
- Must be true/false
- String "true"/"false" accepted

### 4. Array Values
- Must be valid JSON array
- Items must match specified type
- Nested validation for complex types

### 5. Object Values
- Must be valid JSON object
- Required properties must exist
- Property types must match specifications

## Common Validation Patterns

### 1. Required Fields
```json
{
  "required_field": "value is mandatory",
  "optional_field": "can be null"
}
```

### 2. Conditional Requirements
```json
{
  "type": "ARRAY",
  "items": "required when type is ARRAY",
  "properties": "not allowed when type is ARRAY"
}
```

### 3. Type Compatibility
```json
{
  "output": "type STRING",
  "connected_input": "must accept STRING"
}
```

## Error Messages

Common validation errors and their meanings:

| Error | Meaning | Solution |
|-------|---------|----------|
| Missing Required Field | Mandatory field not provided | Add required field |
| Invalid Type | Value type mismatch | Correct value type |
| Cyclic Dependency | Circular task reference | Restructure workflow |
| Invalid Assignment | Missing or invalid assignment rule | Add valid assignment |

## Best Practices

1. **Pre-validation**
   - Validate input data before submission
   - Check type compatibility
   - Verify required fields

2. **Error Handling**
   - Handle validation errors gracefully
   - Provide clear error messages
   - Log validation failures

3. **Testing**
   - Test edge cases
   - Verify all required fields
   - Check type conversions

## Next Steps

1. Review [Assignment Rules](assignment-rules.md)
2. Check [Best Practices](best-practices.md)
3. See [Task Examples](../examples/task-examples.md)