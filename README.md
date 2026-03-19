# Running NVIDIA NemoClaw on OpenShell with Nemotron Inference

## Welcome!

In this lab, you will set up **NVIDIA NemoClaw** — a safe, private runtime for
autonomous AI agents — on a Linux instance and connect it to **Nemotron-3-Nano**
inference via NVIDIA NIM.

## What You'll Build

```
AI Agent (Codex)
    ↓
OpenShell Sandbox (policy-enforced egress)
    ↓ inference.local
Privacy Router (credential stripping)
    ↓
NVIDIA NIM Cloud API
    ↓
Nemotron-3-Nano 30B (thinking model)
```

## What You'll Learn

- Install OpenShell and NemoClaw on Linux
- Create a gateway and sandbox
- Configure NVIDIA NIM as an inference provider
- Apply declarative YAML network policies
- Test Nemotron inference from inside a sandboxed environment
- Run Codex agent inside a secure sandbox

## Prerequisites

- A free NVIDIA API key from [build.nvidia.com](https://build.nvidia.com)
- An OpenAI API key (for Codex agent)
- Basic familiarity with the command line

> **Note:** This lab runs on Linux where all OpenShell features work correctly.
> If you're on macOS/Apple Silicon, some features (local Ollama inference,
> Docker Model Runner bridge) are not yet supported — see
> [NemoClaw issue #260](https://github.com/NVIDIA/NemoClaw/issues/260).

## Start the local development mode:

 ```bash
    # On Mac/Linux
    CONTENT_PATH=$PWD docker compose up --watch

    # On Windows with PowerShell
    $Env:CONTENT_PATH = (Get-Location).Path; docker compose up --watch
  ```

## Time to Complete

~30 minutes

Let's get started! 🚀
