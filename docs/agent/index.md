# Agent Builder Documentation

Welcome to the Agent Builder documentation. This comprehensive guide will help you understand and effectively use the Agent Builder to create powerful workflow agents.

## Getting Started 🚀

- **[Introduction](getting-started/introduction.md)** - Learn about Agent Builder and its capabilities
- **[Core Concepts](getting-started/concepts.md)** - Understand the fundamental concepts
- **[Quick Start](getting-started/quickstart.md)** - Create your first agent in minutes

## Core Components 🛠️

### Task Types

Build your workflow using four powerful task types:

| Task Type | Description | Use Case |
|-----------|-------------|----------|
| [AI Task](tasks/ai-task.md) | Intelligent processing using AI | Text analysis, content generation |
| [Human Task](tasks/human-task.md) | Human intervention and approvals | Document review, decision making |
| [App Task](tasks/app-task.md) | Integration with external tools | Data processing, API interactions |
| [Coder Task](tasks/coder-task.md) | Custom code execution | Complex calculations, transformations |

### Configuration

Master the configuration of your agents:

- **[Input Parameters](parameters/input-parameters.md)** - Configure task inputs
- **[Output Parameters](parameters/output-parameters.md)** - Define expected outputs
- **[Assignment Rules](guides/assignment-rules.md)** - Manage task assignments

## Essential Guides 📚

Learn best practices and validation rules:

1. **[Assignment Rules Guide](guides/assignment-rules.md)**
   - User assignments
   - Team assignments
   - Assignment logic

2. **[Validation Rules](guides/validation-rules.md)**
   - Input validation
   - Output validation
   - Common validation patterns

3. **[Best Practices](guides/best-practices.md)**
   - Task design
   - Error handling
   - Performance optimization

## Examples and Tutorials 💡

Get hands-on experience:

- **[Task Examples](examples/task-examples.md)** - Real-world task configurations

## Quick References 📋

### Common Task Configurations

```json
{
  "name": "Task Name",
  "type": "TASK_TYPE",
  "instructions": "Task instructions",
  "input_parameters": [],
  "expected_output": [],
  "error_policy": "RAISE"
}
```

### Assignment Rule Example

```json
{
  "rule_type": "Exception",
  "assignment_type": "User",
  "assignee_id": "user_id"
}
```

## Support and Resources 🆘

- **Documentation Updates** - Check regularly for the latest documentation
- **Community Support** - Join our community forum
- **Technical Support** - Contact our support team

## Next Steps

1. Start with the [Introduction](getting-started/introduction.md)
2. Learn the [Core Concepts](getting-started/concepts.md)
3. Follow the [Quick Start Guide](getting-started/quickstart.md)
4. Explore [Task Types](tasks/overview.md)
