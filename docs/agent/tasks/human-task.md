# Human Task

## Overview

Human Tasks are designed for workflows that require human intervention, such as approvals, reviews, or decision-making. These tasks pause the workflow execution until human input is received.

## Visual Example

![Human Task Example](./images/human-task.png)

## Configuration Structure

```json
{
  "type": "HUMAN_TASK",
  "block": {
    "name": "Human Task Name",
    "type": "HUMAN_TASK",
    "instructions": "Based on the given message, categorize the message into APPROVED or REJECTED or UNDECIDED",
    "dependencies": [],
    "error_policy": "RAISE"
  }
}
```

## Required Fields

| Field | Type | Description | Required |
|-------|------|-------------|----------|
| name | string | Task identifier | Yes |
| instructions | string | Task instructions | Yes |
| dependencies | array | Task dependencies | No |
| error_policy | string | Error handling strategy | No |

## Task Instructions

Human tasks require clear instructions for the assigned user or team. Instructions should:

- Be specific and clear
- Define expected actions
- Provide decision criteria
- Include any relevant context

Example:
```json
{
  "instructions": "Review the generated content for accuracy and alignment with brand guidelines. Choose APPROVE if content meets all criteria, REJECT if changes are needed, or UNDECIDED if further review is required."
}
```

## Assignment Rules

### User Assignment
```json
{
  "rule_type": "Human Feedback",
  "assignment_type": "User",
  "assignee_id": "user_id"
}
```

### Team Assignment
```json
{
  "rule_type": "Human Feedback",
  "assignment_type": "Team",
  "assignee_id": "team_id",
  "assignment_logic": "Round Robin"
}
```

## Common Use Cases

### 1. Document Approval
```json
{
  "name": "Review Document",
  "instructions": "Review the attached document and approve or reject based on compliance guidelines",
  "dependencies": ["Generate Document"]
}
```

### 2. Content Review
```json
{
  "name": "Review Content",
  "instructions": "Review the AI-generated content for accuracy and brand voice. Provide feedback if changes are needed.",
  "dependencies": ["Generate Content"]
}
```

### 3. Decision Making
```json
{
  "name": "Process Decision",
  "instructions": "Review the analysis results and decide on the next steps. Choose APPROVE to proceed or REJECT to revise.",
  "dependencies": ["Analyze Data"]
}
```

## Task Flow

```mermaid
graph TD
    A[Previous Task] --> B[Human Task]
    B -- Approved --> C[Next Task]
    B -- Rejected --> D[Revision Task]
    B -- Undecided --> E[Additional Review]
```

## Response Types

Human tasks can have three possible responses:

1. **APPROVED**: Task approved, workflow continues
2. **REJECTED**: Task rejected, requires revision
3. **UNDECIDED**: Additional review needed

## Best Practices

### 1. Instructions

✅ **Do**:

- Write clear, concise instructions
- Specify decision criteria
- Include relevant context
- Define expected outcomes

❌ **Don't**:

- Use technical jargon
- Write vague instructions
- Omit important details
- Assume knowledge

### 2. Assignment Rules

✅ **Do**:

- Set appropriate assignees
- Configure backup assignments
- Use team assignments for shared tasks
- Set reasonable deadlines

❌ **Don't**:

- Assign to inactive users
- Skip backup assignments
- Ignore time zones
- Use overly complex rules

### 3. Dependencies

✅ **Do**:

- Clearly define prerequisites
- Verify data availability
- Set proper task order
- Handle all outcomes

❌ **Don't**:

- Create circular dependencies
- Skip validation
- Ignore error cases

## Error Handling

### Configuration

```json
{
  "error_policy": "RAISE",
  "notification_channels": ["email", "slack"],
  "escalation_after": 24  // hours
}
```

### Common Scenarios

1. **Timeout**
    - Task not completed within deadline
    - Automatic escalation

2. **Assignee Unavailable**
    - Automatic reassignment
    - Backup assignee notification

3. **Invalid Response**
    - Validation error
    - Request for correction

## Integration Examples

### With AI Task
```json
{
  "name": "Review AI Output",
  "instructions": "Review the AI-generated content for accuracy",
  "dependencies": ["Generate AI Content"]
}
```

### With App Task
```json
{
  "name": "Approve Document",
  "instructions": "Review and approve the generated document",
  "dependencies": ["Generate PDF"]
}
```

## Notifications

### Assignment Notification
```json
{
  "channel": "email",
  "template": "task_assignment",
  "data": {
    "task_name": "Document Review",
    "instructions": "...",
    "deadline": "2024-02-24T15:00:00Z"
  }
}
```

### Reminder Notification
```json
{
  "channel": "slack",
  "template": "task_reminder",
  "data": {
    "task_name": "Content Review",
    "time_remaining": "2 hours"
  }
}
```

## Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| Missed Deadlines | Set up reminders and escalations |
| Unclear Instructions | Review and clarify task requirements |
| Assignment Conflicts | Configure backup assignments |
| Response Delays | Implement notification system |

## Next Steps

1. Learn about [AI Tasks](ai-task.md)
2. Review [Assignment Rules](../guides/assignment-rules.md)
3. Check [Best Practices](../guides/best-practices.md)

## Related Documentation

- [Assignment Rules](../guides/assignment-rules.md)
