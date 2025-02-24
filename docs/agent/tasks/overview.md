# Task Types Overview

## Introduction

Tasks are the building blocks of your workflow agent. Each task type serves a specific purpose and has its own configuration requirements.

## Available Task Types

### AI Task
![AI Task](../assets/ai-task-icon.svg)

**Purpose**: Execute AI-driven operations for intelligent processing.

**Use Cases**:
- Text analysis
- Content generation
- Data classification
- Pattern recognition

[Learn more about AI Tasks →](ai-task.md)

### Human Task
![Human Task](../assets/human-task-icon.svg)

**Purpose**: Enable human intervention in workflows.

**Use Cases**:
- Document approval
- Content review
- Decision making
- Quality validation

[Learn more about Human Tasks →](human-task.md)

### App Task
![App Task](../assets/app-task-icon.svg)

**Purpose**: Integrate with external tools and applications.

**Use Cases**:
- Data processing
- File operations
- API integrations
- Tool automation

[Learn more about App Tasks →](app-task.md)

### Coder Task
![Coder Task](../assets/coder-task-icon.svg)

**Purpose**: Execute custom code artifacts.

**Use Cases**:
- Custom calculations
- Data transformation
- Complex logic implementation
- Script execution

[Learn more about Coder Tasks →](coder-task.md)

## Task Configuration Basics

All tasks share some common configuration elements:

```json
{
  "name": "Task Name",
  "description": "Task description",
  "type": "TASK_TYPE",
  "error_policy": "RAISE"
}
```

### Common Elements

| Element | Description | Required |
|---------|-------------|----------|
| name | Unique task identifier | Yes |
| description | Task purpose and functionality | Yes |
| type | Task type identifier | Yes |
| error_policy | Error handling strategy | No |

## Next Steps

- Learn about [Input Parameters](../parameters/input-parameters.md)
- Understand [Assignment Rules](../guides/assignment-rules.md)
- Check [Best Practices](../guides/best-practices.md)