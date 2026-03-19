# Step 5 - Test Nemotron Inference

## Connect to the Sandbox

```bash
openshell sandbox connect <your-sandbox-name>
```

## Test Basic Inference

Send a request to `inference.local` — the Privacy Router will intercept it,
inject your NVIDIA NIM credentials, and forward to Nemotron-3-Nano:

```bash
curl http://inference.local/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3-nano-30b-a3b",
    "messages": [
      {"role": "user", "content": "Hello! Who are you and what can you do?"}
    ]
  }'
```

You should get a JSON response with Nemotron's reply including a `thinking` block!

## Test with Streaming

```bash
curl http://inference.local/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3-nano-30b-a3b",
    "stream": true,
    "messages": [
      {"role": "user", "content": "Explain what OpenShell does in 3 sentences."}
    ]
  }'
```

## Test Tool Use

Nemotron supports function calling. Try this:

```bash
curl http://inference.local/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3-nano-30b-a3b",
    "messages": [
      {"role": "user", "content": "What is 42 * 137?"}
    ],
    "tools": [
      {
        "type": "function",
        "function": {
          "name": "calculator",
          "description": "Perform arithmetic",
          "parameters": {
            "type": "object",
            "properties": {
              "expression": {"type": "string"}
            }
          }
        }
      }
    ]
  }'
```

## List Available Models

```bash
curl http://inference.local/v1/models
```

## Verify Privacy Router is Working

Notice that your request to `inference.local` never included your NVIDIA API key
— the Privacy Router injected it automatically. Try confirming this:

```bash
# This should work (Privacy Router adds the key)
curl http://inference.local/v1/models

# This would fail if you called NVIDIA directly without a key
# curl https://integrate.api.nvidia.com/v1/models  ← would return 401
```

> **What just happened?** Your request went:
> Sandbox → Egress Proxy → Policy Engine (allow) → Privacy Router (inject key) → NVIDIA NIM → Nemotron-3-Nano → back to you
>
> Your API key never touched the sandbox filesystem. 🔐

Exit the sandbox when done:

```bash
exit
```
