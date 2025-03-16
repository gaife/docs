# Task Types Overview

## Introduction

Tasks are the building blocks of your workflow agent. Each task type serves a specific purpose and has its own
configuration requirements.

## Available Task Types

### AI Task

**Purpose**: Execute AI-driven operations for intelligent processing.

**Use Cases**:

-   Text analysis
-   Content generation
-   Data classification
-   Pattern recognition

!!! Learn

    [More about AI Tasks →](ai-task.md)

### Human Task

**Purpose**: Enable human intervention in workflows.

**Use Cases**:

-   Document approval
-   Content review
-   Decision making
-   Quality validation

!!! Learn

    [More about Human Tasks →](human-task.md)

### App Task

**Purpose**: Integrate with external tools and applications.

**Use Cases**:

-   Data processing
-   File operations
-   API integrations
-   Tool automation

!!! Learn

    [More about App Tasks →](app-task.md)

### Coder Task

**Purpose**: Execute custom code artifacts.

**Use Cases**:

-   Custom calculations
-   Data transformation
-   Complex logic implementation
-   Script execution

!!! Learn

    [More about Coder Tasks →](coder-task.md)

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

| Element      | Description                    | Required |
| ------------ | ------------------------------ | -------- |
| name         | Unique task identifier         | Yes      |
| description  | Task purpose and functionality | Yes      |
| type         | Task type identifier           | Yes      |
| error_policy | Error handling strategy        | No       |
