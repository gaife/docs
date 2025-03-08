# 💻 Coder Artifact Documentation

## Overview

The Coder Artifact in Gaife allows you to create custom code functions that process inputs and generate structured outputs. These artifacts are used in knowledge agents to handle the computational logic of your automation workflows.

## Interface Layout

The Coder Artifact interface consists of:

- **Header Section**: Displays the artifact name with edit option
- **Action Buttons**: Update and Publish buttons
- **Code Generation Assistant**: AI-powered assistant to help generate code
- **Generated Code**: Python editor with syntax highlighting

## Creating a Coder Artifact

### Getting Started

When creating a new Coder Artifact, you'll see the following interface elements:

1. **Code Generation Assistant**:
    - Powered by GAIFE AI
    - Provides a conversational interface to describe what you want to build
    - Options to define input parameters, expected output, and add rules

2. **Supporting Files Section**:
    - Rules (.txt, .json, .yaml)
    - Sample data (.csv, .json)

3. **Input & Output Configuration**:
    - Define the structure and format of your input parameters
    - Specify the expected output format

## Writing Coder Functions

### Function Structure

Every Coder Artifact must follow this structure:

1. **Main Function**:
    - Must accept an `arguments` parameter that contains all input data
    - Process the input according to business logic
    - Return structured output data

2. **Required Boilerplate Code**:
   ```python
    # Calculate and store the result
    result = {}
    # Call the function for execution here
    output = [your_function_name](arguments)
    # Store the output in result dictionary
    result['output'] = output
   ```

### Example Implementation

Here's an example of a complete Coder Artifact for invoice calculation:

```python
from datetime import datetime

def calculate_amount(arguments):
    try:
        # Extract items_to_be_billed from the arguments dictionary
        items = arguments.get('items_to_be_billed', [])

        # If items is a string, attempt to evaluate it into a list
        if isinstance(items, str):
            import ast
            items = ast.literal_eval(items)

        # Ensure items is a list
        if not isinstance(items, list):
            items = [items]

        # Initialize totals
        total_price = 0.0

        # Process each item in the list
        for item in items:
            # Extract item attributes, handle default values and type conversion
            item_price = float(item.get('item_price', '0'))
            item_quantity = int(item.get('item_quantity', '1'))  # Default to 1 if not provided

            # Calculate item subtotal
            item_subtotal = item_price * item_quantity
            total_price += item_subtotal

        # Calculate taxes and total amounts
        total_state_tax = total_price * 0.06  # 6% state GST
        total_central_tax = total_price * 0.06  # 6% central GST
        total_tax = total_state_tax + total_central_tax
        total_amount = total_price
        total_amount_with_gst = total_price + total_tax  # 12% overall GST

        # Get the current date
        current_date = datetime.now().strftime('%d-%m-%Y')

        # Construct the output dictionary according to expected output format
        return {
            'calculation_amount': {
                'total_tax': f'{total_tax:.2f}',
                'current_date': current_date,
                'total_amount': f'{total_amount:.2f}',
                'total_state_tax': f'{total_state_tax:.2f}',
                'total_central_tax': f'{total_central_tax:.2f}',
                'total_amount_with_gst': f'{total_amount_with_gst:.2f}',
            }
        }

    except Exception as e:
        raise e


# Calculate and store the result
result = {}
# Call the function for execution here
output = calculate_amount(arguments)
# Store the output in result dictionary
result['output'] = output
```

## Key Components

### Function Parameters

Every Coder Artifact function must accept a single parameter called `arguments`, which is a dictionary containing all input data passed to the function.

### Input Handling

Proper input handling is essential:
- Extract values using `arguments.get('key', default_value)`
- Include type conversion for input values (e.g., `float()`, `int()`)
- Handle potential missing or malformed data
- Implement error handling with try/except blocks

### Processing Logic

Implement your business logic in the middle section of your function:
- Perform calculations
- Apply business rules
- Format data as needed

### Output Structure

Return a structured dictionary that matches your expected output format:
- Nest data in appropriate hierarchies
- Format values consistently
- Include all required fields

### Boilerplate Code

Always include the required boilerplate code at the end to properly store and return results:

```python
# Calculate and store the result
result = {}
# Call the function for execution here
output = [your_function_name](arguments)
# Store the output in result dictionary
result['output'] = output
```

## 🏷️ Naming Conventions

Following consistent naming conventions ensures readability, maintainability, and consistency across all Gaife automation workflows.

### Arguments Dictionary Naming Conventions

#### General Rules

1. **Parameter Name**: Always use `arguments` (lowercase) as the parameter name for your main function
2. **Key Formatting**:
    - Use snake_case (lowercase with underscores)
    - Use descriptive names that clearly indicate purpose
    - Avoid abbreviations unless widely recognized
    - Keep names concise yet meaningful

#### Examples of Well-Formatted Argument Keys

✅ **Recommended**:

- `customer_id`
- `invoice_date`
- `items_to_be_billed`
- `tax_rate`
- `shipping_address`
- `payment_method`

❌ **Avoid**:

- `CustomerID` (not snake_case)
- `inv_dt` (unclear abbreviation)
- `i` (not descriptive)
- `calculation amount` (spaces instead of underscores)

#### Nested Arguments

For nested dictionaries within arguments:

```python
# Accessing nested arguments example
customer = arguments.get('customer', {})
address = customer.get('shipping_address', {})
postal_code = address.get('postal_code', '')
```

### Output Dictionary Naming Conventions

#### General Rules

1. **Result Structure**: Always use the following structure:
    ```python
    result = {}
    output = your_function_name(arguments)
    result['output'] = output
    ```

2. **Primary Output Keys**:
    - Use snake_case for all keys (lowercase with underscores)
    - Format values consistently based on their type
    - For categories or sections, use descriptive names

3. **Formatting Values**:
    - Format numeric values consistently (e.g., `f'{value:.2f}'` for currency)
    - Use ISO formats for dates when possible
    - Convert all values to appropriate string formats for API compatibility

#### Examples of Well-Formatted Output Keys

✅ **Recommended Output Structure**:
```python
return {
    'calculation_results': {
        'total_tax': f'{total_tax:.2f}',
        'current_date': current_date,
        'total_amount': f'{total_amount:.2f}',
        'total_state_tax': f'{total_state_tax:.2f}',
        'total_central_tax': f'{total_central_tax:.2f}',
        'total_amount_with_gst': f'{total_amount_with_gst:.2f}'
    }
}
```

❌ **Avoid**:
```python
return {
    'calculation amount': {  # spaces instead of underscores
        'TotalTax': total_tax,  # not snake_case
        'date': current_date,  # inconsistent naming (prefer current_date)
        'amt': total_amount,  # unclear abbreviation
        'Total_amount_with_gst': f'{total_amount_with_gst:.2f}'  # mixed case
    }
}
```

### Data Type Conventions

#### Numeric Values

- Format currency values with two decimal places: `f'{value:.2f}'`
- Use integers for counts: `item_count = int(value)`
- Format percentages consistently: `f'{value:.1f}%'` or as decimal values

#### Date and Time Values

- Use ISO format (YYYY-MM-DD) for date storage: `2023-04-15`
- For display formatting, use consistent patterns: `'%d-%m-%Y'`
- Include timezone information for time-sensitive operations

#### Boolean Values

- Use clear boolean names: `is_taxable`, `has_discount`, `requires_shipping`

## 🚀 Testing and Publishing

### Testing Your Code

1. Use the "Dry Run" button to test your code with sample inputs
2. Check the output to ensure it matches the expected format
3. Debug any issues before publishing

### Publishing

1. Click the "Publish" button to make your artifact available for use
2. Published artifacts can be used by other components in your knowledge agent

## 💡 Best Practices

1. **Error Handling**: Always implement robust error handling with try/except blocks
2. **Input Validation**: Validate all input data before processing
3. **Documentation**: Add detailed comments to explain the logic and data flow
4. **Type Conversion**: Convert input data to appropriate types (string, int, float)
5. **Default Values**: Provide sensible default values for optional parameters
6. **Code Organization**: Structure your code logically with clear sections
7. **Naming Consistency**: Use the same name for the same concept throughout your code
8. **Clarity**: Choose names that reveal intent
9. **Simplicity**: Keep names simple but descriptive
10. **Hierarchy**: Structure output dictionaries logically with clear parent-child relationships

## 🔄 Integration with Workflow Agent

The Coder Artifact is a key component in the Workflow Agent ecosystem:

1. **Workflow Integration**: Your published Coder Artifact will be available as a processing step within workflow agents
2. **Data Flow**:
    - Workflow agents pass input parameters to your Coder function
    - The function processes these parameters according to your logic
    - Output parameters are returned to the workflow for further processing or final delivery
3. **Complex Processing**: Workflow agents can chain multiple Coder Artifacts together to handle complex calculations and data transformations
4. **Automation Pipeline**: Acts as the computational engine for automation workflows, handling everything from simple calculations to complex business logic

### Use Cases in Workflows

- **Financial Calculations**: Process invoice amounts, taxes, discounts as shown in the example
- **Data Transformation**: Convert data between formats or structures
- **Decision Logic**: Implement complex business rules that determine workflow paths
- **API Integration**: Pre-process data before sending to external systems
- **Analytics**: Perform statistical analysis or data aggregation within workflows

## 📚 Related Resources

- 📖 Python Coding Best Practices
- 🧮 Business Logic Implementation Guide
- 🔌 Data Integration Patterns
- 🧑‍💻 Learn about [Coder Task](../tasks/coder-task.md)
