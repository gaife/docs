# [GAIFE](https://www.gaife.com) Documentation

Welcome to the official GAIFE documentation for both our products and APIs.

## Product Documentation

- [Agent](./agent/index.md) - Guides for the GAIFE Agent product
    - [Getting Started](./agent/getting-started/introduction.md)
    - [Tasks Overview](./agent/tasks/overview.md)
    - [Knowledge Base](./agent/knowledge/base.md)

## API Documentation

- [Chat API](./chat/index.md) - Developer integration with GAIFE Chat
    - [Conversations](./chat/conversations.md)
    - [Messages](./chat/message.md)
    - [Webhooks](./chat/webhooks.md)

## API Access & Authentication

To manage your API keys:

1. Visit the API Access page at:
   `https://beta.gaife.com/organizations/{your_org}/api-access`

2. Here you can:
    - Create new API keys
    - View existing keys
    - Revoke or rotate keys as needed
    - Set key permissions and expiration dates

!!! warning "Important"
    Keep your API keys secure and never share them in public repositories or client-side code.

## Authentication

- All API endpoints require authentication using an API key in the Bearer token
format in the Authorization header:

    ```bash
    Authorization: Bearer your_api_key_here
    ```

## Next Steps

- [Chat API](./chat/index.md)
