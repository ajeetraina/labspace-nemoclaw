# Step 1 - Install NemoClaw

NemoClaw is NVIDIA's agent plugin for OpenShell. Before installing, configure
npm to install packages without root permissions:
```bash
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
export PATH="$HOME/.npm-global/bin:$PATH"
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.bashrc
```

Now install NemoClaw:
```bash
curl -LsSf https://raw.githubusercontent.com/NVIDIA/NemoClaw/main/install.sh | bash
```

Reload your shell:
```bash
source ~/.bashrc
```

Verify NemoClaw is installed:
```bash
nemoclaw help
```

You should see the NemoClaw command list including `onboard`, `list`, `connect` and more.

> **Note:** `nemoclaw --version` is not a valid command. Use `nemoclaw help` to see all available commands.

## Run the Onboarding Wizard

NemoClaw includes an interactive setup wizard that handles everything in one go:
```bash
nemoclaw onboard
```

The wizard runs through 7 steps:

1. **Preflight checks** — verifies Docker, openshell, and port availability
2. **Start OpenShell gateway** — deploys K3s inside Docker (~60-90 seconds)
3. **Create sandbox** — enter a name like `collabnix`
4. **Configure inference** — choose NVIDIA Cloud API (option 1)
5. **Set up inference provider** — enter your `nvapi-` key
6. **Set up OpenClaw inside sandbox** — installs the agent
7. **Apply policy presets** — select `pypi` and `npm` (suggested defaults)

> ⚠️ **When prompted for a sandbox name**, type only the name (e.g. `collabnix`).
> Do not paste other commands — the prompt is waiting for input only.

> ⚠️ **If you see "Port 8080 is not available"**, a previous gateway is still
> running. Fix it with:
> ```bash
> openshell gateway stop
> nemoclaw onboard
> ```

> 💡 Have your `nvapi-` key from [build.nvidia.com](https://build.nvidia.com)
> ready before starting — you'll need it at step 4.

Once onboarding completes you'll see:

```
=== Installation complete ===
Sandbox      collabnix (Landlock + seccomp + netns)
Model        nvidia/nemotron-3-super-120b-a12b (NVIDIA Cloud API)
```

Connect to your sandbox:
```bash
nemoclaw collabnix connect
```

Testing the inference:

```bash
curl https://inference.local/v1/chat/completions \
   -H "Content-Type: application/json" \
   -d '{"model":"nvidia/nemotron-3-super-120b-a12b","messages":[{"role":"user","content":"hello, who are you?"}]}'
```
