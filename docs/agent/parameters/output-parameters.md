# Output Parameters

## Overview

Output parameters define the expected output format and structure from tasks in your workflow. This guide covers all available output parameter types and their configurations.

## Output Parameter Types

### Basic Types
```python
BOOLEAN  # True/False values
INTEGER  # Whole numbers
FLOAT    # Decimal numbers
STRING   # Text values
DATETIME # Date and time values
```

### Structured Types
```python
ENTITY   # Entity references
JSON     # JSON formatted data
ARRAY    # List of items
OBJECT   # Complex objects
ENUM     # Enumerated values
```

### File Types
```python
TEXT     # Plain text files
MARKDOWN # Markdown formatted text
HTML     # HTML content
CODE     # Source code
PDF      # PDF documents
CSV      # CSV data
XML      # XML data
FILE     # Generic files
PPT      # PowerPoint presentations
```

### Media Types
```python
IMAGE    # Image files
VIDEO    # Video files
AUDIO    # Audio files
GIF      # GIF animations
```

### Web Types
```python
URL      # Web URLs
EMAIL    # Email addresses
```

### Special Types
```python
TABLE    # Tabular data
DOCUMENT # Document files
```

## Configuration Examples

### 1. Basic Type Configuration
```json
{
  "name": "is_valid",
  "type": "BOOLEAN",
  "description": "Indicates if the process was successful"
}
```

### 2. Array Type Configuration
```json
{
  "name": "results",
  "type": "ARRAY",
  "items": {
    "type": "STRING"
  },
  "description": "List of processed items"
}
```

### 3. Object Type Configuration
```json
{
  "name": "user_data",
  "type": "OBJECT",
  "properties": {
    "id": {
      "type": "INTEGER",
      "description": "User ID"
    },
    "email": {
      "type": "STRING",
      "description": "User email"
    }
  },
  "description": "User information object"
}
```

### 4. File Output Configuration
```json
{
  "name": "report",
  "type": "PDF",
  "description": "Generated report file",
  "content": null,
  "gaife_internal_is_data_visible": true,
  "gaife_internal_is_data_editable": false
}
```

## Task-Specific Examples

### AI Task Output
```json
{
  "name": "sentiment_analysis",
  "type": "OBJECT",
  "properties": {
    "sentiment": {
      "type": "STRING",
      "description": "Detected sentiment"
    },
    "confidence": {
      "type": "FLOAT",
      "description": "Confidence score"
    }
  }
}
```

### App Task Output
```json
{
  "name": "api_response",
  "type": "JSON",
  "description": "API response data",
  "gaife_internal_is_data_visible": true
}
```

### Coder Task Output
```json
{
  "name": "calculation_result",
  "type": "OBJECT",
  "properties": {
    "result": {
      "type": "FLOAT",
      "description": "Calculated value"
    },
    "metadata": {
      "type": "OBJECT",
      "description": "Additional information"
    }
  }
}
```

## Visibility and Editability

Control output parameter visibility and editability:
```json
{
  "name": "sensitive_data",
  "type": "OBJECT",
  "gaife_internal_is_data_visible": false,
  "gaife_internal_is_data_editable": false
}
```

## Data Source Configuration

### Memory Storage
```json
{
  "name": "result",
  "type": "OBJECT",
  "data_source": "MEMORY"
}
```

### Data Lake Storage
```json
{
  "name": "large_dataset",
  "type": "ARRAY",
  "data_source": "DATA_LAKE",
  "data_lake_id": "12345"
}
```

## Validation Rules

### Type-Specific Validation
1. **Basic Types**
   - BOOLEAN: true/false
   - INTEGER: whole numbers
   - FLOAT: decimal numbers
   - STRING: text values
   - DATETIME: valid date/time format

2. **Array Validation**
   - Must have defined item type
   - Items must match specified type
   - Valid for nested arrays

3. **Object Validation**
   - Properties must be defined
   - Property types must be valid
   - Required properties present

## Best Practices

### 1. Naming Conventions
✅ **Do**:
```json
{
  "name": "userProfileData",
  "description": "Complete user profile information"
}
```

❌ **Don't**:
```json
{
  "name": "data1",
  "description": "some data"
}
```

### 2. Type Selection
- Use simplest type possible
- Consider downstream requirements
- Document format requirements

### 3. Documentation
- Clear descriptions
- Format examples
- Usage notes

## Common Issues and Solutions

### Issue 1: Type Mismatch
**Problem**: Output type doesn't match connected input
**Solution**: Verify type compatibility between connected tasks

### Issue 2: Missing Properties
**Problem**: Required object properties not defined
**Solution**: Define all required properties in object type

### Issue 3: Visibility Issues
**Problem**: Data not visible in UI
**Solution**: Check gaife_internal_is_data_visible setting

## Examples by Use Case

### 1. Data Processing Result
```json
{
  "name": "processing_result",
  "type": "OBJECT",
  "properties": {
    "status": "STRING",
    "processed_items": "INTEGER",
    "errors": "ARRAY"
  }
}
```

### 2. File Generation
```json
{
  "name": "generated_file",
  "type": "PDF",
  "description": "Generated report",
  "gaife_internal_is_data_visible": true
}
```

### 3. API Integration
```json
{
  "name": "api_result",
  "type": "JSON",
  "description": "API response data"
}
```

## Next Steps

1. Learn about [Input Parameters](input-parameters.md)
2. Check [Validation Rules](../guides/validation-rules.md)
3. Review [Best Practices](../guides/best-practices.md)
