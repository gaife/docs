<!-- # Assignment Rules Guide

Assignment rules define how tasks are assigned to users or teams in your workflow. Understanding and configuring these rules correctly is crucial for efficient workflow execution.

## Overview

Assignment rules determine:
- Who can handle tasks
- How tasks are distributed
- What happens during exceptions
- When human feedback is needed

## Rule Types

### 1. Exception Rules
Used for handling exceptional cases in workflow execution.

```json
{
  "rule_type": "Exception",
  "assignment_type": "User",
  "assignee_id": "user_id"
}
```

### 2. Human Feedback Rules
Used when tasks require human intervention or approval.

```json
{
  "rule_type": "Human Feedback",
  "assignment_type": "Team",
  "assignee_id": "team_id",
  "assignment_logic": "Round Robin"
}
```

## Assignment Types

### User Assignment
Direct assignment to a specific user.

```json
{
  "assignment_type": "User",
  "assignee_id": "user_id"
}
```

**Use cases:**
- Specific expertise required
- Individual responsibility needed
- Direct accountability

### Team Assignment
Assignment to a team of users.

```json
{
  "assignment_type": "Team",
  "assignee_id": "team_id",
  "assignment_logic": "Round Robin"
}
```

**Use cases:**
- Shared responsibility
- Load balancing
- Backup coverage

## Assignment Logic

### Round Robin (Team Only)
Distributes tasks evenly among team members.

```json
{
  "assignment_type": "Team",
  "assignment_logic": "Round Robin",
  "team_id": "team_id"
}
```

## Validation Requirements

1. **Minimum Requirements**
   - At least one assignment rule must exist
   - Valid assignee (user/team) must be specified
   - Proper rule type must be selected

2. **Team Assignment Requirements**
   - Assignment logic required for team assignments
   - Valid team ID must be provided
   - Team must have active members

3. **User Assignment Requirements**
   - Valid user ID must be provided
   - User must have appropriate permissions

## Configuration Examples

### Basic Exception Rule
```json
{
  "rule_type": "Exception",
  "assignment_type": "User",
  "assignee_id": "user_123",
  "assignment_logic": null
}
```

### Team Review Rule
```json
{
  "rule_type": "Human Feedback",
  "assignment_type": "Team",
  "assignee_id": "team_456",
  "assignment_logic": "Round Robin"
}
```

### Multiple Rules Example
```json
[
  {
    "rule_type": "Exception",
    "assignment_type": "User",
    "assignee_id": "user_123"
  },
  {
    "rule_type": "Human Feedback",
    "assignment_type": "Team",
    "assignee_id": "team_456",
    "assignment_logic": "Round Robin"
  }
]
```

## Best Practices

1. **Rule Organization**
   - Keep rules simple and clear
   - Use descriptive assignee names
   - Document special requirements

2. **Assignment Strategy**
   - Use team assignments for shared tasks
   - Assign exceptions to experts
   - Consider backup assignments

3. **Validation**
   - Verify assignee existence
   - Check team member availability
   - Test assignment logic

## Common Issues

### Issue 1: Missing Assignment Rule
**Problem**: Workflow validation fails due to no assignment rules.
**Solution**: Add at least one valid assignment rule.

### Issue 2: Invalid Assignee
**Problem**: Specified user or team doesn't exist.
**Solution**: Verify assignee IDs and permissions.

### Issue 3: Team Logic Missing
**Problem**: Team assignment without assignment logic.
**Solution**: Add Round Robin logic for team assignments.

## Examples by Use Case

### Document Approval Flow
```json
{
  "rule_type": "Human Feedback",
  "assignment_type": "Team",
  "assignee_id": "reviewers_team",
  "assignment_logic": "Round Robin"
}
```

### Error Handling
```json
{
  "rule_type": "Exception",
  "assignment_type": "User",
  "assignee_id": "support_engineer"
}
```

## Integration with Tasks

### Human Task Assignment
```json
{
  "type": "HUMAN_TASK",
  "block": {
    "name": "Document Review",
    "instructions": "Review and approve document"
  }
}
```

The task will be assigned based on the configured assignment rules.

## Next Steps

1. Review [Task Types](../tasks/overview.md)
2. Learn about [Validation Rules](validation-rules.md)
3. Check [Best Practices](best-practices.md)

## Need Help?

- Consult the [Documentation](../index.md)
- Contact Support
- Visit Community Forum -->

# Assignment Rules Guide

Assignment rules define how tasks are assigned to users or teams in your workflow. Understanding and configuring these rules correctly is crucial for efficient workflow execution.

## Overview

Assignment rules determine:
- Who can handle tasks
- How tasks are distributed
- What happens during exceptions
- When human feedback is needed

## Rule Types

=== "User Guide"
    ### Exception Rules
    Exception rules are used when you need someone to handle error cases or special situations in your workflow.
    
    **How to Configure:**
    1. Select "Exception" as the rule type
    2. Choose either a specific user or team
    3. For teams, select "Round Robin" distribution method
    
    **When to Use:**
    - Error handling
    - Special case processing
    - Technical support tasks

    ### Human Feedback Rules
    These rules are for tasks that need human review or approval.
    
    **How to Configure:**
    1. Select "Human Feedback" as the rule type
    2. Choose assignee (user or team)
    3. Set distribution method for teams
    
    **When to Use:**
    - Document approvals
    - Content review
    - Quality checks

=== "Developer Reference"
    ### Exception Rules
    ```json
    {
      "rule_type": "Exception",
      "assignment_type": "User",
      "assignee_id": "user_id"
    }
    ```

    ### Human Feedback Rules
    ```json
    {
      "rule_type": "Human Feedback",
      "assignment_type": "Team",
      "assignee_id": "team_id",
      "assignment_logic": "Round Robin"
    }
    ```

## Assignment Types

=== "User Guide"
    ### Assigning to a User
    When you need a specific person to handle the task:
    
    1. Select "User" assignment
    2. Choose the user from the dropdown
    3. Save the assignment
    
    **Best for:**
    - Tasks needing specific expertise
    - Individual responsibility
    - Direct accountability

    ### Assigning to a Team
    When you want a group to handle tasks:
    
    1. Select "Team" assignment
    2. Choose the team
    3. Select "Round Robin" for fair distribution
    4. Save the assignment
    
    **Best for:**
    - Shared workload
    - Backup coverage
    - Balanced distribution

=== "Developer Reference"
    ### User Assignment
    ```json
    {
      "assignment_type": "User",
      "assignee_id": "user_id"
    }
    ```

    ### Team Assignment
    ```json
    {
      "assignment_type": "Team",
      "assignee_id": "team_id",
      "assignment_logic": "Round Robin"
    }
    ```

## Configuration Requirements

=== "User Guide"
    ### Basic Requirements
    1. At least one assignment rule is needed
    2. Select a valid user or team
    3. Choose appropriate rule type
    
    ### For Team Assignments
    - Select distribution method
    - Ensure team has active members
    - Set backup assignments if needed
    
    ### For User Assignments
    - Verify user has correct permissions
    - Consider setting backup assignees
    - Check user availability

=== "Developer Reference"
    ### Validation Schema
    ```json
    {
      "required": ["rule_type", "assignment_type", "assignee_id"],
      "properties": {
        "rule_type": {
          "enum": ["Exception", "Human Feedback"]
        },
        "assignment_type": {
          "enum": ["User", "Team"]
        },
        "assignment_logic": {
          "enum": ["Round Robin", null]
        }
      }
    }
    ```

## Common Use Cases

=== "User Guide"
    ### Document Review Process
    1. Create Human Feedback rule
    2. Assign to review team
    3. Enable Round Robin distribution
    
    ### Error Handling
    1. Create Exception rule
    2. Assign to support team or expert
    3. Set notification preferences

=== "Developer Reference"
    ### Document Review Configuration
    ```json
    {
      "rule_type": "Human Feedback",
      "assignment_type": "Team",
      "assignee_id": "reviewers_team",
      "assignment_logic": "Round Robin"
    }
    ```

    ### Error Handling Configuration
    ```json
    {
      "rule_type": "Exception",
      "assignment_type": "User",
      "assignee_id": "support_engineer"
    }
    ```

## Best Practices

=== "User Guide"
    1. **Keep It Simple**
       - Use clear assignments
       - Set logical rules
       - Document special cases
    
    2. **Plan for Backup**
       - Set alternate assignees
       - Consider team availability
       - Plan for holidays/time off

=== "Developer Reference"
    1. **Code Organization**
       ```json
       {
         "rules": [
           {
             "primary": {...},
             "backup": {...}
           }
         ]
       }
       ```
    
    2. **Validation**
       - Verify IDs before saving
       - Check permissions
       - Test rule combinations

## Need Help?

- Review the [Task Types](../tasks/overview.md) guide
- Contact support team
- Visit developer forum