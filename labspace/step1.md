# Step 2 - Create Gateway and Sandbox

## What is the Gateway?

The **Gateway** is OpenShell's control plane. It runs as a K3s Kubernetes
cluster inside a single Docker container and manages:

- Sandbox lifecycle
- Credential providers
- Policy distribution
- The Privacy Router for inference

## Start the Gateway

```bash
openshell gateway start
```

This pulls the gateway image and initializes the K3s cluster. It takes about
60-90 seconds on first run.

When complete you'll see:

```
✓ Gateway ready
  Name: openshell
  Endpoint: https://127.0.0.1:8080
```

## Verify the Gateway is Healthy

```bash
openshell gateway info
```

## Set Up Your NVIDIA NIM Provider

Before creating a sandbox, configure your NVIDIA API key as a credential
provider. Get your free key at [build.nvidia.com](https://build.nvidia.com):

```bash
openshell provider create \
  --name nvidia-nim \
  --type nvidia \
  --credential NGC_API_KEY=nvapi-YOUR-KEY-HERE
```

Verify it was created:

```bash
openshell provider list
```

## Create Your Sandbox

Create a sandbox with Codex as the agent:

```bash
openshell provider create \
  --name codex \
  --type codex \
  --credential OPENAI_API_KEY=sk-YOUR-KEY-HERE

openshell sandbox create --provider codex -- codex
```

Wait for the sandbox image to pull and start:

```
✓ Sandbox allocated
✓ Image pulled
✓ Sandbox ready
```

## List Your Sandboxes

```bash
openshell sandbox list
```

Note the sandbox name — you'll use it in the next steps. It will be something
like `happy-dolphin` or `swift-falcon`.

> **What just happened?** OpenShell created an isolated container with
> policy-enforced egress routing. The Codex agent is running inside it,
> but ALL outbound network traffic is blocked by default until you apply a policy.
