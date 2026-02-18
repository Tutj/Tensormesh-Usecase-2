# vLLM + LMCache Response Contract

The tutorial expects an OpenAI-compatible response from vLLM, enriched with performance metrics.

## Request
Standard `POST /v1/chat/completions`

## Response (Expected Structure)
```json
{
  "id": "chatcmpl-...",
  "object": "chat.completion",
  "created": 123456789,
  "model": "...",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "..."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 15000,
    "completion_tokens": 100,
    "total_tokens": 15100
  },
  "performance": {
    "ttft": 0.05,
    "total_latency": 1.2,
    "kv_cache_hit_rate": 0.98
  }
}
```

*Note: The `performance` block may be provided in `extra_body` or parsed from logs if the vLLM version used doesn't include it in the standard response.*
