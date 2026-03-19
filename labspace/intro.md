# Step 1 - Install OpenShell and NemoClaw

## Install OpenShell

OpenShell is the safe, private runtime for autonomous AI agents. Install it
using the official install script:

```bash
curl -LsSf https://raw.githubusercontent.com/NVIDIA/OpenShell/main/install.sh | sh
```

Reload your shell to pick up the new PATH:

```bash
source ~/.bashrc
```

Verify the installation:

```bash
openshell --version
```

## Install NemoClaw

NemoClaw is NVIDIA's agent plugin for OpenShell that adds Nemotron model
support and the NemoClaw agent harness:

```bash
curl -LsSf https://raw.githubusercontent.com/NVIDIA/NemoClaw/main/install.sh | sh
```

Reload your shell again:

```bash
source ~/.bashrc
```

Verify NemoClaw:

```bash
nemoclaw --version
```

## Verify Docker is Running

OpenShell requires Docker. Confirm it's available:

```bash
docker info | grep -E "Server Version|Operating System"
```

You should see the Docker server version and Linux as the OS.

## Run the NemoClaw Doctor

NemoClaw includes a built-in diagnostic tool:

```bash
nemoclaw doctor
```

This checks that all dependencies are installed and working correctly. Fix any
issues it reports before proceeding.

> **Tip:** If `nemoclaw` is not found after install, run
> `export PATH="$HOME/.local/bin:$PATH"` and try again.
