# Conversations

Conversations represent chat sessions in the system. Each conversation is associated with a workflow and can contain multiple messages.

## Create a Conversation

Create a new conversation instance.

- **Endpoint**: `POST /conversations`
- **Authentication**: API Key Required

### Headers

```bash
Authorization: Bearer {{API_KEY}}
Content-Type: application/json
```

### Conversation Usecases

The `usecase` field determines the conversation's purpose and behavior:

1. **DEFAULT** (`"usecase": "DEFAULT"`)
    - Standard conversation, chat with the GAIFE system
    - Used for general messaging purposes

2. **WORKFLOW_EXECUTION** (`"usecase": "WORKFLOW_EXECUTION"`)
    - Used when executing an existing workflow
    - Automatically sets metadata with `workflow_id` from **config**
    - Enables workflow-specific message processing

!!! info
    Optionally, configure a `callback_url` in `config` to receive a [webhook/callback](./webhooks.md) on every [message of conversation](./message.md).

### Request Body

```json
{
    "channel": "API",
    "usecase": "WORKFLOW_EXECUTION",  // One of: DEFAULT, WORKFLOW_EXECUTION
    "config": {
        "workflow_id": "string", // Required, the ID of the workflow to execute
        "callback_url": "string", // To receive a webhook on every message of conversation
        // Additional configuration
    },
    "metadata": {
        // Optional custom data
    },
    "team_id": "integer"  // Optional
}
```

### Response

```json
{
    "id": "string"  // conversation ID without hyphens
}
```

### Example Usecases Request Body

=== "Workflow Execution"
    ```json
    {
        "channel": "API",
        "usecase": "WORKFLOW_EXECUTION",
        "config": {
            "workflow_id": "123",
            "callback_url": "https://example.com/webhook"
        }
    }
    ```

=== "Default"
    ```json
    {
        "channel": "API",
        "usecase": "DEFAULT",
        "config": {
            "callback_url": "https://example.com/webhook"
        }
    }
    ```

### Notes

- For `WORKFLOW_EXECUTION`, the metadata will automatically include the workflow_id from config
- The `DEFAULT` usecase provides the most flexibility for custom implementations
- All usecases support custom metadata for additional context
- The `channel` field is used to specify the communication channel, currently only `API` is supported
- To receive a webhook on every message of conversation, configure a `callback_url` in `config`

### Example Request

```bash
curl 'https://developer.gaife.com/api/conversations' \
  -H 'Authorization: Bearer {{API_KEY}}' \
  -H 'Content-Type: application/json' \
  --data-raw '{
    "channel":"API",
    "usecase":"WORKFLOW_EXECUTION",
    "config":{
      "workflow_id":"84",
      "callback_url":"https://example.com/webhook"
    }
  }'
```

### Example Response

```json
{
    "id": "550e8400e29b41d4a716446655440000"
}
```

### Status Codes

- `201`: Conversation created successfully
- `400`: Invalid request data
- `401`: Invalid or missing API key
- `500`: Server error

## **Get Conversation Messages**

Retrieve all messages in a conversation.

- **Endpoint**: `GET /conversations/{conversation_id}/messages`
- **Authentication**: Required

### Path Parameters

- `conversation_id`: The ID of the conversation

### Example Request

```bash
curl 'https://developer.gaife.com/api/conversations/550e8400e29b41d4a716446655440000/messages' \
  -H 'Authorization: Bearer {{API_KEY}}' \
  -H 'Content-Type: application/json'
```

### Response

- Messages are returned in chronological order, from newest to oldest.

```json
[
    {
        "id": "string",
        "conversation_id": "string",
        "content": "string",
        "content_type": "TEXT",  // One of: TEXT, IMAGE, VIDEO, AUDIO, etc.
        "filename": "string",    // Optional, present for media messages
        "type": "USER_INPUT",    // Message type
        "channel": "string",     // Communication channel
        "status": "RECEIVED",    // Message status
        "metadata": {
            "key": "value"      // Optional custom metadata
        },
        "created_by": "string",
        "created_at": "datetime",
        "updated_at": "datetime"
    }
]
```

### Status Codes

- `200`: Success
- `404`: Conversation not found
- `500`: Server error

### Notes

1. **Content Types**:
    - `TEXT`: Regular text messages
    - `IMAGE`: Image files
    - `VIDEO`: Video files
    - `AUDIO`: Audio files

2. **Message Types**:
    - `USER_INPUT`: Messages from users
    - Other types as per your MessageType choices

3. **Message Status**:
    - `RECEIVED`: Message has been received
    - Other statuses as per your MessageStatus choices

4. **Response Fields**:
    - `id`: Unique message identifier
    - `content`: Message content/text
    - `filename`: Present only for media messages
    - `metadata`: Optional JSON object for additional data

## Next Steps

- [Handle Messages](./message.md)
- [Webhook/Callback Integration](./webhooks.md)
