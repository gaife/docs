# [GAIFE](https://www.gaife.com) API Documentation

This is the documentation for the GAIFE developer APIs.

## Base URL

- The base URL for all API requests is:

```bash
https://developer.gaife.com/api
```

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
