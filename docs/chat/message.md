# Messages

Messages are individual communications within a conversation. Each message can contain text or media content and
maintains metadata about its creation and status.

## Send Message

Send a new message in a conversation.

-   **Endpoint**: `POST /messages`
-   **Authentication**: API Key Required

### Headers

```bash
Authorization: Bearer {{API_KEY}}
Content-Type: application/json
```

### Message Types

The `content_type` field determines the message format and required fields:

1. **TEXT** (`"content_type": "text"`)

    - Basic text messages
    - Requires `content` field
    - No file handling needed

2. **MEDIA** (`audio/*, image/*, application/pdf, etc.`)
    - Used for file attachments
    - Requires `filename` field
    - Must obtain pre-signed URL before upload
    - Supports various formats (audio, image, PDF, XLSX, CSV)
    - More detail below [Media Handling](#media-handling)

### Request Body

| Field        | Type   | Required    | Description                               |
| ------------ | ------ | ----------- | ----------------------------------------- |
| conversation | string | Yes         | ID of the conversation                    |
| content      | string | Conditional | Message text (required for text messages) |
| content_type | string | Yes         | Message format (see Supported Types)      |
| filename     | string | Conditional | File path/name (required for media)       |
| metadata     | object | No          | Custom message data                       |
| channel      | string | No          | Defaults to "api"                         |

### Example: Text Message

```json
{
    "conversation": "550e8400e29b41d4a716446655440000",
    "content": "Hello world!",
    "content_type": "text",
    "metadata": {
        // Optional metadata
    }
}
```

### Example: Media Message

```json
{
    "conversation": "550e8400e29b41d4a716446655440000",
    "filename": "20240321_120000_report.pdf",
    "content_type": "application/pdf",
    "metadata": {
        // Optional metadata
    }
}
```

### Response

```json
{
    "id": "550e8400e29b41d4a716446655440000",
    "conversation": "550e8400e29b41d4a716446655440000",
    "content": "Hello world!",
    "content_type": "text",
    "channel": "api",
    "type": "USER_INPUT",
    "status": "received",
    "metadata": {
        // Additional metadata
    },
    "created_at": "2024-03-21T10:00:00Z",
    "updated_at": "2024-03-21T10:00:00Z"
}
```

### Supported Content Types

| Type  | Content Type     | Required Fields | Max Size | Description                 |
| ----- | ---------------- | --------------- | -------- | --------------------------- |
| Text  | text             | content         | 4KB      | Plain text messages         |
| Audio | audio/\*         | filename        | 10MB     | Voice messages, audio files |
| Image | image/\*         | filename        | 5MB      | Photos and images           |
| PDF   | application/pdf  | filename        | 10MB     | PDF documents               |
| Excel | application/xlsx | filename        | 10MB     | Excel spreadsheets          |
| CSV   | text/csv         | filename        | 5MB      | CSV data files              |

### Status Codes

-   `200`: Message sent successfully
-   `400`: Validation error
-   `401`: Invalid or missing API key
-   `422`: Missing organization ID
-   `500`: Server error

### Example Request

```bash
curl 'https://beta-api.gaife.com/developer/api/messages' \
  -H 'Authorization: Bearer {{API_KEY}}' \
  -H 'Content-Type: application/json' \
  --data-raw '{"conversation":"550e8400e29b41d4a716446655440000","content":"Hello world!","content_type":"text"}'
```

### Notes

1. **File Uploads**:

    - Obtain pre-signed URL before uploading media
    - Use appropriate content type headers
    - Respect file size limits

2. **Message Processing**:

    - Messages are processed asynchronously
    - Check message status for delivery confirmation
    - All timestamps are in UTC

3. **Metadata Usage**:

    - Use metadata for custom tracking
    - Metadata must be valid JSON
    - Maximum metadata size: 4KB

4. **Rate Limits**:
    - 100 messages per minute per organization
    - Larger limits available for enterprise plans

## Media Handling

For sending media messages (images, audio, PDFs, etc.), you'll need to follow a three-step process:

### Step 1: Get Pre-signed URL

**Endpoint**: `POST /media/presigned-url`

**Headers**:

```bash
Authorization: Bearer {{API_KEY}}
Content-Type: application/json
```

**Request Body**:

```json
{
    "file_name": "report.pdf",
    "file_type": "application/pdf"
}
```

**Response**:

```json
{
    // The URL to upload the file
    "presigned_url": "https://s3.amazonaws.com/bucket/...",
    // The file name will be used to send the message
    "file_name": "20240321_120000_report.pdf"
}
```

**Curl Example**:

```bash
curl -X POST 'https://beta-api.gaife.com/developer/api/media/presigned-url' \
  -H 'Authorization: Bearer {{API_KEY}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "file_name": "report.pdf",
    "file_type": "application/pdf"
}'
```

### Step 2: Upload File

Use the pre-signed URL to upload your file:

```bash
curl --location --request PUT "${PRESIGNED_URL}" \ # The URL received from Step 1
    --header "Content-Type: application/octet-stream" \
    --data-binary "@/path/to/your/file"
```

Note: Replace `${PRESIGNED_URL}` with the URL received from Step 1, and adjust the file path to your local file
location.

### Step 3: Send Message

After successful upload, send the message:

```json
{
    "conversation": "550e8400e29b41d4a716446655440000",
    // Use the file name received from Step 1
    "filename": "20240321_120000_report.pdf",
    "content_type": "application/pdf",
    "metadata": {
        // Optional metadata
    }
}
```

**Curl Example**:

```bash
curl --location 'https://beta-api.gaife.com/developer/api/messages' \
    --header 'Authorization: Bearer {{API_KEY}}' \
    --header 'Content-Type: application/json' \
    --data '{
        "conversation": "550e8400e29b41d4a716446655440000",
        "content_type": "image/jpeg",
        "filename": "20240321_120000_image.jpg",
    }'
```

### Supported File Types

| File Type | Content Type     | Storage Folder |
| --------- | ---------------- | -------------- |
| Audio     | audio/\*         | audio/         |
| Image     | image/\*         | images/        |
| PDF       | application/pdf  | pdfs/          |
| Excel     | application/xlsx | xlsx/          |
| CSV       | text/csv         | csv/           |

### Complete Example

```python
import requests

# 1. Get pre-signed URL
presigned_response = requests.post(
    'https://api.example.com/media/presigned-url',
    headers={
        'Authorization': 'Bearer your_api_key',
        'Content-Type': 'application/json'
    },
    json={
        'file_name': 'report.pdf',
        'file_type': 'application/pdf'
    }
)
presigned_data = presigned_response.json()

# 2. Upload file using pre-signed URL
with open('report.pdf', 'rb') as f:
    requests.put(
        presigned_data['presigned_url'],
        data=f,
        headers={'Content-Type': 'application/pdf'}
    )

# 3. Send message with file reference
message_response = requests.post(
    'https://api.example.com/messages',
    headers={
        'Authorization': 'Bearer your_api_key',
        'Content-Type': 'application/json'
    },
    json={
        'conversation': '550e8400e29b41d4a716446655440000',
        'filename': presigned_data['file_name'],
        'content_type': 'application/pdf'
    }
)
```

### Media Upload Notes

1. **File Naming**:

    - Uploaded files are automatically prefixed with timestamp
    - Format: `YYYYMMDD_HHMMSS_original_filename`

2. **Storage Organization**:

    - Files are stored in type-specific folders
    - Each organization has its own directory
    - Maintains clean separation of different file types

3. **Limitations**:

    - Text files are not supported for pre-signed URLs
    - Pre-signed URLs expire after 15 minutes
    - File type must match the declared content type

4. **Error Cases**:
    - `422`: Missing organization ID
    - `400`: Invalid file type or unsupported operation
    - `500`: Server error during URL generation

## Message Metadata

The `metadata` field in messages serves several critical functions:

1.  **File Storage Information**

    -   Tracks file storage details for system generated files
    -   Used when system generates a file response
    -   System automatically processes the file:
        -   Extracts bucket and path information
        -   Transfers file to conversation media bucket
        -   Updates message with standardized S3 URI
    -   Supports both HTTPS and S3 protocol URLs
    -   File access requires proper authentication
    -   Example message metadata:
        ```json
        // Original metadata from AI Engine
        {
            "metadata": {
                "from_s3": true,
                "output": {
                    "s3_url": "https://bucket.s3.amazonaws.com/file.pdf",
                    "output_type": "application/pdf"
                }
            }
        }
        ```

2.  **Workflow Instance Status**

    ```json
    {
        "metadata": {
            "workflow_instance_id": "789",
            "workflow_instance_status": "RUNNING"
        }
    }
    ```

    The metadata tracks workflow execution state through the `workflow_instance_status` field:

    **Available Status Values**

    | Status       | Description                                       |
    | ------------ | ------------------------------------------------- |
    | `RUNNING`    | Workflow is actively executing tasks              |
    | `PAUSED`     | Workflow execution has been temporarily suspended |
    | `COMPLETED`  | All tasks have successfully finished              |
    | `FAILED`     | Workflow stopped due to an error                  |
    | `CANCELLED`  | Workflow was manually cancelled by user           |
    | `TERMINATED` | Workflow stopped due to system intervention       |

    **Example Status Updates**

    === "Initial workflow start"

        ```json
        {
            "metadata": {
                "workflow_instance_id": "789",
                "workflow_instance_status": "RUNNING"
            }
        }
        ```

    === "Workflow paused for user input"

        ```json
        {
            "metadata": {
                "workflow_instance_id": "789",
                "workflow_instance_status": "PAUSED"
            }
        }
        ```

    === "Workflow completion"

        ```json
        {
            "metadata": {
                "workflow_instance_id": "789",
                "workflow_instance_status": "COMPLETED"
            }
        }
        ```

    **Status Transitions**

    -   `RUNNING` → Any status
    -   `PAUSED` → `RUNNING`, `CANCELLED`, `TERMINATED`
    -   `COMPLETED` (Terminal state)
    -   `FAILED` (Terminal state)
    -   `CANCELLED` (Terminal state)
    -   `TERMINATED` (Terminal state)

    **Important Notes**

    -   Status updates are real-time
    -   Terminal states cannot transition to other states
    -   Each status update includes timestamp information
    -   Status changes may trigger notifications or webhook events
    -   Historical status changes are preserved in workflow logs

3.  **Ticket Integration**

    ```json
    {
        "ticket_id": "123",
        "task_instance_status": "EXECUTED"
    }
    ```

    The metadata contains ticket information that automatically manages support ticket lifecycles:

    **Status Management**

    -   Ticket status is automatically updated based on task instance status:
        ```bash
        ERRORED, CANCELLED, REJECTED → CLOSED
        EXECUTED → RESOLVED
        ```
    -   Updates are handled atomically to prevent race conditions
    -   Status changes are tracked in ticket history

    **Ticket Fields** Important ticket information tracked:

    ```json
    {
        "workflow_instance_id": "789",
        "failed_task_id": "456",
        "exception_message": "Error details",
        "workflow_name": "Document Processing",
        "task_name": "PDF Generation",
        "workflow_progress": 75.5,
        "conversation_id": "conv_123"
    }
    ```

    **Example Metadata Usage**

    ```json
    {
        "metadata": {
            "ticket_id": "123",
            "task_instance_status": "EXECUTED",
            "workflow_instance_id": "789",
            "task_name": "Document Processing",
            "workflow_progress": 85.5
        }
    }
    ```

    **Notification System** When tickets are created or updated:

    -   Assignees receive email notifications
    -   Notifications include:
        -   Ticket ID
        -   Task name and description
        -   Workflow name
        -   Exception details (if any)
        -   Assignment details

    **Important Notes**

    -   Ticket updates are processed atomically using database transactions
    -   Missing ticket IDs are safely ignored
    -   Status updates are immediate and automatic
    -   All ticket activities are logged for audit purposes

### Example Message with Complex Metadata

```json
{
    "conversation": "550e8400e29b41d4a716446655440000",
    "content": "Processing your document...",
    "content_type": "text",
    "metadata": {
        "from_s3": true,
        "output": {
            "s3_url": "s3://bucket/org123/documents/20240321_120000_report.pdf",
            "output_type": "application/pdf"
        },
        "workflow_instance_id": "789",
        "workflow_instance_status": "RUNNING",
        "ticket_id": "244",
        "task_instance_status": "EXECUTED"
    }
}
```
