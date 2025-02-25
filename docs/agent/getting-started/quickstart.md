# Quick Start Guide

This guide will help you create your first workflow agent using Agent Builder. We'll create a simple document review workflow that includes AI analysis and human approval.

## Prerequisites
- Access to Agent Builder platform
- Necessary permissions to create and edit workflows
- Basic understanding of workflow concepts

## Step 1: Create a New Agent

1. Navigate to the Agent Builder dashboard
2. Click "Create New Agent"
3. Fill in the basic details:
   ```json
   {
     "name": "Document Review Agent",
     "description": "Reviews documents using AI and human approval"
   }
   ```

## Step 2: Configure Assignment Rules

Before adding tasks, set up at least one assignment rule:

1. Click "Assignment Rules"
2. Add a new rule:
   ```json
   {
     "rule_type": "Human Feedback",
     "assignment_type": "User",
     "assignee_id": "your_user_id"
   }
   ```

## Step 3: Add an AI Task

Add the first task for AI analysis:

1. Click "Add Task"
2. Choose "AI Task"
3. Configure the task:
   ```json
   {
     "name": "Analyze Document",
     "type": "TASK",
     "instructions": "Analyze the document content and extract key points",
     "input_parameters": [
       {
         "name": "document_text",
         "type": "string",
         "description": "Document content to analyze",
         "required": true,
         "source": "task_config"
       }
     ],
     "expected_output": [
       {
         "name": "analysis_result",
         "type": "object",
         "description": "Analysis results",
         "properties": {
           "key_points": {
             "type": "array",
             "items": {
               "type": "string"
             }
           },
           "summary": {
             "type": "string"
           }
         }
       }
     ]
   }
   ```

## Step 4: Add a Human Task

Add a human approval task:

1. Click "Add Task"
2. Choose "Human Task"
3. Configure the task:
   ```json
   {
     "name": "Review Analysis",
     "type": "HUMAN_TASK",
     "instructions": "Review the AI analysis and approve or reject",
     "dependencies": ["Analyze Document"]
   }
   ```

## Step 5: Test Your Agent

1. **Save Your Agent**
   - Click "Save" to store your configuration

2. **Run a Test**
   - Click "Test Agent"
   - Input test document content
   - Monitor the execution

3. **Review Results**
   - Check AI analysis output
   - Complete human review task
   - Verify workflow completion

## Common Issues and Solutions

### Issue 1: Task Creation Errors
- **Problem**: Cannot create task
- **Solution**: Check all required fields are filled

### Issue 2: Assignment Rule Errors
- **Problem**: Assignment rule validation fails
- **Solution**: Ensure at least one rule is properly configured

### Issue 3: Dependency Errors
- **Problem**: Tasks not properly connected
- **Solution**: Verify task dependencies are correctly set

## Next Steps

After completing this guide, you can:

1. **Explore More Task Types**
   - Learn about [App Tasks](../tasks/app-task.md)
   - Try [Coder Tasks](../tasks/coder-task.md)

2. **Advanced Configuration**
   - Configure [Input Parameters](../parameters/input-parameters.md)
   - Set up [Output Parameters](../parameters/output-parameters.md)

3. **Best Practices**
   - Review [Validation Rules](../guides/validation-rules.md)
   - Learn [Best Practices](../guides/best-practices.md)

## Tips for Success

1. **Start Simple**
   - Begin with basic tasks
   - Add complexity gradually

2. **Test Thoroughly**
   - Test each task individually
   - Verify full workflow execution

3. **Document Your Work**
   - Add clear descriptions
   - Document any special requirements

## Need Help?

- Check our [Documentation](../index.md)
