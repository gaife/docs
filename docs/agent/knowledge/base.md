# 🧠 Knowledge Agent Documentation

## Knowledge Agents and Workflows

Knowledge Agents serve as repositories of specialized knowledge that **workflow agents** can leverage to complete tasks.
The workflow agent can access and utilize the `Coder` and `Rule` artifacts to:

-   Execute calculations and transformations (Coder artifacts)
-   Apply business rules and validations (Rule artifacts)
-   Make decisions based on predefined criteria
-   Ensure consistency across automated processes

This integration allows for more sophisticated automation while maintaining separation between process flow and domain
knowledge.

## Overview

The Knowledge Agent feature in Gaife allows you to create specialized knowledge bases for various automation tasks. This
documentation covers how to access, configure, and use Knowledge Agents in the Gaife platform.

## Creating a Knowledge Base

1. Navigate to `https://beta.gaife.com/organizations/[your-org-id]/knowledge/bases`
2. Click the `+ Build New Knowledge` button in the upper right corner
3. Fill in the required information:
    - **Knowledge Name**: A descriptive name for your knowledge base
    - **Description**: Brief explanation of the knowledge base's purpose
4. Click `Create` to generate your new knowledge base
5. Your new knowledge base will appear in the knowledge bases list

## Accessing Knowledge Agents

1. Navigate to `https://beta.gaife.com/organizations/[your-org-id]/knowledge/bases`
2. From the Knowledge Base listing, locate and click on the desired knowledge agent (e.g., "Invoice Generation")
3. This will take you to the detailed view at `https://beta.gaife.com/organizations/[your-org-id]/knowledge/bases/[id]`

## Interface Layout

The Knowledge Agent interface consists of:

-   **Header Section**: Displays the title of the knowledge agent with its description
-   **Navigation Tabs**: Code, Rules, Prompts, and Data Relations
-   **Search Bar**: Allows searching for specific artifacts within the knowledge base
-   **Filter Status**: Option to filter items by their status
-   **Create Button**: Used to add new artifacts to the knowledge base

## Artifacts Table

The main table displays artifacts with the following columns:

|    Column     | Description                                           |
| :-----------: | ----------------------------------------------------- |
|     Type      | The category of the artifact (e.g., Coder)            |
| Artifact Name | Name of the artifact with an identifier               |
|    Details    | View details of the artifact (eye icon)               |
|    Status     | Current status of the artifact (e.g., DRAFT, PUBLISH) |
|  Created On   | Date when the artifact was created                    |
| Last Updated  | Date when the artifact was last modified              |

## Working with Artifacts

### Viewing Existing Artifacts

The knowledge base will display all artifacts associated with it in the main table. Each artifact includes its type,
name, status, and relevant dates.

### Creating New Artifacts

To create a new artifact:

1. Click the `+ Create` button in the upper right corner
2. A "Create Artifact" popup will appear
3. Enter the artifact name in the "Artifact Name" field
4. Select one of the available artifact types:
    - **Coder**: Create and manage code-related knowledge
    - **Rule**: Define business rules and logic
    - **Prompt**: Design and store prompts (Coming Soon)
    - **Data Relation**: Define data schemas and relationships (Coming Soon)
5. Click the `Create Artifact` button at the bottom of the popup to finish

> **Note:** Some artifact types may be marked as "Coming Soon" and not yet available for use.

## Available Tabs

### 💻 Code Tab

The Code tab displays coding artifacts that handle calculations and logic for your automation. These components handle
the computational aspects of your process. Create "Coder" type artifacts to add code-related knowledge to your knowledge
base.

### 📏 Rules Tab

The Rules tab contains business rules and constraints that govern your process. These rules ensure compliance with
business requirements and validation standards. Add "Rule" type artifacts to define the business logic for your
knowledge agent.

### 💬 Prompts Tab (Coming Soon)

The Prompts tab includes templates and AI prompts used to guide your automated processes. These prompts help structure
AI interactions for consistent results. This feature will allow you to design and store prompts for AI interactions.

### 🔄 Data Relations Tab (Coming Soon)

The Data Relations tab shows connections between different data sources and how they interact with your process. This
includes mappings to databases or external systems. This feature will allow you to define data schemas and
relationships.

## Managing Artifacts

### Publishing an Artifact

To publish an artifact:

1. Select the artifact from the list
2. Click the `PUBLISH` button in the Status column
3. Confirm the publishing action

### Viewing Artifact Details

To view details of an artifact:

1. Click the eye icon in the Details column
2. A detailed view will display the artifact's configuration and content

## Best Practices

1. **Regular Updates**: Keep artifacts updated to reflect current business requirements
2. **Clear Naming**: Use descriptive names for artifacts to ensure easy identification
3. **Status Management**: Properly manage the status of artifacts to control what is active
4. **Documentation**: Add detailed descriptions to artifacts for team knowledge sharing

## Troubleshooting

If you encounter issues with your Knowledge Agent:

1. Check the Status column to ensure artifacts are published
2. Review the Last Updated date to verify recency
3. Ensure proper connections in the Data Relations tab
4. Consult the development team if issues persist

## Example: Invoice Generation Knowledge Agent

As an example, an "Invoice Generation" knowledge agent might include:

-   **Code artifacts**: Invoice calculation formulas
-   **Rules**: Validation rules for invoice fields
-   **Prompts**: Templates for generating invoice descriptions
-   **Data Relations**: Connections to customer and product databases

This knowledge agent would automate the creation of invoices according to established business rules while maintaining
consistency and accuracy.

## Managing Knowledge Bases

### Editing a Knowledge Base

To edit an existing knowledge base:

1. From the main knowledge bases page, locate the knowledge base you wish to edit
2. Click the pencil icon in the Actions column
3. Update the Name and/or Description as needed
4. Save your changes

### Deleting a Knowledge Base

To delete a knowledge base:

1. From the main knowledge bases page, locate the knowledge base you wish to delete
2. Click the delete (trash) icon in the Actions column
3. Confirm the deletion when prompted

> **Warning:** Deletion is permanent and will remove all associated artifacts

## 📚 Related Resources

-   [Coder Artifact](coder-artifact.md)
