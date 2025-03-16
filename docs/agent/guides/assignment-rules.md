# Assignment Rules Guide

Assignment rules define how tasks are assigned to users or teams in your workflow. Understanding and configuring these
rules correctly is crucial for efficient workflow execution.

## Overview

Assignment rules determine:

-   Who can handle tasks
-   How tasks are distributed
-   What happens during exceptions
-   When human feedback is needed

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
