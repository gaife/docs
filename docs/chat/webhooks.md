# Webhook Integration Guide

## Overview

Our webhook system enables real-time notifications for message events in your conversations. When a message is created
or processed in your conversation, our system sends HTTP POST requests to your configured webhook URL with detailed
message information.

## Quick Start

### 1. Configure Webhook

Add a webhook URL to your conversation configuration:

```json
{
    "channel": "API",
    "usecase": "WORKFLOW_EXECUTION",
    "config": {
        "workflow_id": "123",
        "callback_url": "https://your-domain.com/webhook" // Required
    }
}
```

### 2. Implement Webhook Handler

=== "Django"

    ```python
    from django.http import HttpResponse
    from django.views.decorators.csrf import csrf_exempt
    import json

    @csrf_exempt
    def webhook_handler(request):
        try:
            payload = json.loads(request.body)
            # Process payload asynchronously
            process_webhook.delay(payload)
            return HttpResponse(status=200)
        except Exception as e:
            logger.error(f"Webhook processing error: {str(e)}")
            return HttpResponse(status=200)  # Always acknowledge receipt
    ```

=== "FastAPI"

    ```python
    from fastapi import FastAPI, Request
    from fastapi.responses import JSONResponse

    app = FastAPI()

    @app.post("/webhook")
    async def webhook_handler(request: Request):
        try:
            payload = await request.json()
            # Process payload asynchronously
            process_webhook.delay(payload)
            return JSONResponse(status_code=200)
        except Exception as e:
            logger.error(f"Webhook processing error: {str(e)}")
            return JSONResponse(status_code=200)
    ```

=== "Flask"

    ```python
    from flask import Flask, request

    app = Flask(__name__)

    @app.route('/webhook', methods=['POST'])
    def webhook_handler():
        try:
            payload = request.get_json()
            # Process payload asynchronously
            process_webhook.delay(payload)
            return '', 200
        except Exception as e:
            logger.error(f"Webhook processing error: {str(e)}")
            return '', 200
    ```

=== "Express.js"

    ```javascript
    const express = require('express');
    const app = express();

    app.post('/webhook', express.json(), async (req, res) => {
        try {
            const payload = req.body;
            // Process payload asynchronously
            await processWebhook(payload);
            res.sendStatus(200);
        } catch (err) {
            console.error(`Webhook processing error: ${err.message}`);
            res.sendStatus(200); // Always acknowledge receipt
        }
    });
    ```

=== "Spring Boot"

    ```java
    @RestController
    public class WebhookController {
        @PostMapping("/webhook")
        public ResponseEntity<Void> handleWebhook(@RequestBody String payload) {
            try {
                // Process payload asynchronously
                webhookService.processWebhook(payload);
                return ResponseEntity.ok().build();
            } catch (Exception e) {
                logger.error("Webhook processing error: " + e.getMessage());
                return ResponseEntity.ok().build(); // Always acknowledge receipt
            }
        }
    }
    ```

=== "PHP"

    ```php
    <?php

    // Receive POST data
    $rawPayload = file_get_contents('php://input');

    try {
        // Parse JSON payload
        $payload = json_decode($rawPayload, true);
        if (json_last_error() !== JSON_ERROR_NONE) {
            throw new Exception('Invalid JSON payload');
        }

        // Log webhook receipt
        error_log('Webhook received: ' . $rawPayload);

        // Process payload asynchronously (using a job queue system like Laravel Queue or Gearman)
        processWebhookAsync($payload);

        // Always return 200 response
        http_response_code(200);
        echo json_encode(['status' => 'success']);

    } catch (Exception $e) {
        error_log('Webhook processing error: ' . $e->getMessage());

        // Still return 200 to acknowledge receipt
        http_response_code(200);
        echo json_encode(['status' => 'received']);
    }

    /**
        * Example async processing function
        * In practice, this should use a proper job queue
        */
    function processWebhookAsync($payload) {
        // Add to job queue or process directly
        // Example using Laravel Queue:
        // ProcessWebhook::dispatch($payload);
    }
    ```

## Technical Specifications

### Delivery System

-   **Protocol**: HTTPS only (for security)
-   **Method**: POST
-   **Content-Type**: application/json
-   **Timeout**: 30 seconds per request
-   **Retry Logic**: 3 attempts with exponential backoff

### Retry Configuration

-   **Maximum Attempts**: 3
-   **Backoff Schedule**:
    -   1st retry: 4 seconds wait
    -   2nd retry: 8 seconds wait
    -   3rd retry: 10 seconds wait
-   **Retry Conditions**:
    -   Network errors
    -   Timeouts
    -   Non-2xx responses

### Security Requirements

-   HTTPS protocol mandatory
-   Valid SSL certificate
-   Public endpoint accessibility
-   2xx status code response expected
-   IP whitelist recommended
-   Request signing (optional but recommended)

## Webhook Payload

### Structure

```json
{
    "conversation_id": "uuid-string",
    "data": {
        "id": "message-uuid",
        "content": "Message content",
        "content_type": "TEXT",
        "filename": "document.pdf", // Only present for media messages
        "type": "USER_INPUT",
        "channel": "API",
        "status": "RECEIVED",
        "metadata": {
            // Optional custom metadata
        },
        "created_by": "user123",
        "updated_by": "user123",
        "created_at": "2024-03-21T10:00:00Z",
        "updated_at": "2024-03-21T10:00:00Z"
    },
    "timestamp": "2024-03-21T10:00:00.000Z"
}
```

### Field Reference

| Field             | Type     | Required | Description                                           |
| ----------------- | -------- | -------- | ----------------------------------------------------- |
| conversation_id   | UUID     | Yes      | Unique identifier for the conversation                |
| data.id           | UUID     | Yes      | Unique identifier for the message                     |
| data.content      | String   | Yes      | The actual message content                            |
| data.content_type | Enum     | Yes      | Type of content (TEXT, IMAGE, VIDEO, DOCUMENT, AUDIO) |
| data.filename     | String   | No       | Present only for media messages                       |
| data.type         | Enum     | Yes      | Message type (USER_INPUT, SYSTEM, BOT)                |
| data.channel      | Enum     | Yes      | Communication channel (API, WHATSAPP, SMS)            |
| data.status       | Enum     | Yes      | Message status (RECEIVED, PROCESSED, FAILED)          |
| data.metadata     | Object   | No       | Custom metadata for the message                       |
| data.created_by   | String   | Yes      | ID or name of the creator                             |
| data.updated_by   | String   | Yes      | ID or name of the last modifier                       |
| data.created_at   | ISO 8601 | Yes      | Creation timestamp                                    |
| data.updated_at   | ISO 8601 | Yes      | Last update timestamp                                 |
| timestamp         | ISO 8601 | Yes      | Webhook event timestamp                               |

## Best Practices

### Performance

1. **Quick Acknowledgment**

    - Return 2xx status immediately
    - Process webhook data asynchronously
    - Keep handler logic minimal
    - Use background workers for processing

2. **Idempotency Handling**

    ```python
    def handle_webhook(payload):
        message_id = payload['data']['id']
        if is_already_processed(message_id):
            logger.info(f"Skipping duplicate message: {message_id}")
            return

        try:
            process_message(payload)
            mark_as_processed(message_id)
        except Exception as e:
            logger.error(f"Processing failed for {message_id}: {str(e)}")
            raise
    ```

### Error Handling

1. **Graceful Recovery**

    ```python
    def webhook_handler(request):
        try:
            payload = json.loads(request.body)
            validate_payload(payload)  # Validate required fields

            # Store raw webhook for backup
            store_raw_webhook(payload)

            # Process asynchronously
            process_webhook.delay(payload)

        except json.JSONDecodeError:
            logger.error("Invalid JSON payload")
        except ValidationError as e:
            logger.error(f"Validation failed: {str(e)}")
        except Exception as e:
            logger.exception("Webhook processing failed")
            store_failed_webhook(payload)

        # Always acknowledge receipt
        return HttpResponse(status=200)
    ```

2. **Logging Best Practices**
    ```python
    def log_webhook(payload):
        logger.info("Webhook received", extra={
            'conversation_id': payload['conversation_id'],
            'message_id': payload['data']['id'],
            'content_type': payload['data']['content_type'],
            'channel': payload['data']['channel'],
            'timestamp': payload['timestamp']
        })
    ```

## Testing & Development

### Local Testing

1. **Using ngrok**

    ```bash
    # Start ngrok
    ngrok http 8000

    # Update webhook URL
    {
        "config": {
            "callback_url": "https://your-ngrok-url/webhook"
        }
    }
    ```

2. **Manual Testing**
    ```python
    # Test payload generator
    def generate_test_payload():
        return {
            "conversation_id": str(uuid.uuid4()),
            "data": {
                "id": str(uuid.uuid4()),
                "content": "Test message",
                "content_type": "TEXT",
                "type": "USER_INPUT",
                "channel": "API",
                "status": "RECEIVED",
                "created_at": datetime.now().isoformat(),
                "updated_at": datetime.now().isoformat()
            },
            "timestamp": datetime.now().isoformat()
        }
    ```

### Webhook Testing Tools

1. **webhook.site**

    - Quick webhook inspection
    - Real-time request monitoring
    - Headers and payload validation
    - Response simulation

2. **Custom Test Suite**

    ```python
    class WebhookTests(TestCase):
        def test_webhook_handler(self):
            payload = generate_test_payload()
            response = client.post(
                '/webhook',
                json=payload,
                headers={'Content-Type': 'application/json'}
            )
            self.assertEqual(response.status_code, 200)

        def test_invalid_payload(self):
            response = client.post(
                '/webhook',
                data="invalid json",
                headers={'Content-Type': 'application/json'}
            )
            self.assertEqual(response.status_code, 200)
    ```

## Monitoring & Troubleshooting

### Health Checks

1. **Metrics to Monitor**

    - Webhook delivery success rate
    - Average response time
    - Retry attempt counts
    - Error rates by type
    - Processing time distribution

2. **Alerting Rules**
    - High error rate threshold
    - Response time degradation
    - Retry count spikes
    - Failed delivery clusters

### Common Issues & Solutions

| Issue      | Possible Causes      | Solution                    |
| ---------- | -------------------- | --------------------------- |
| Timeouts   | Slow processing      | Implement async processing  |
|            | Heavy payload        | Optimize payload size       |
|            | Network latency      | Reduce processing time      |
| SSL Errors | Invalid certificate  | Update SSL certificate      |
|            | Expired certificate  | Set up auto-renewal         |
|            | Certificate mismatch | Verify domain matching      |
| 404 Errors | Invalid URL          | Check webhook configuration |
|            | URL changed          | Update webhook URL          |
|            | Service unavailable  | Verify service status       |

### Debug Checklist

-   Verify webhook URL configuration
-   Check SSL certificate validity
-   Monitor server logs for errors
-   Validate payload format
-   Test endpoint accessibility
-   Verify network connectivity
-   Check rate limiting
-   Monitor system resources

## Support

For webhook-related assistance, please provide:

-   Conversation ID
-   Message ID
-   Timestamp of issue
-   Server logs
-   Webhook configuration
-   Error messages
-   Retry attempt logs

## Rate Limiting

-   Maximum 10 concurrent webhook deliveries
-   Maximum 100 webhooks per minute per endpoint
-   Automatic back-pressure handling
-   Rate limit headers included in responses
