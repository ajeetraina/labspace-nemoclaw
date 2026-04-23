## Install OpenShell 0.0.32
 
OpenShell is distributed as a statically linked tarball. Detect your
architecture and download the matching release:
 
```bash
# Detect architecture (aarch64 for Apple Silicon / ARM Linux, x86_64 for Intel/AMD)
ARCH=$(uname -m)
OPENSHELL_VERSION="0.0.32"
 
# Create your user-local bin directory
mkdir -p ~/.local/bin
 
# Download and extract the openshell binary directly into ~/.local/bin
curl -fsSL \
  "https://github.com/NVIDIA/OpenShell/releases/download/v${OPENSHELL_VERSION}/openshell-${ARCH}-unknown-linux-musl.tar.gz" \
  | tar -xz -C ~/.local/bin openshell
 
chmod +x ~/.local/bin/openshell
```
 
Add `~/.local/bin` to your PATH:
 
```bash
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```
 
Verify the installation:
 
```bash
openshell --version
```
 
You should see:
 
```
openshell 0.0.32
```
 
## Install NemoClaw
 
First, configure npm to install global packages without root:
 
```bash
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
export PATH="$HOME/.npm-global/bin:$PATH"
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.bashrc
```
 
Run the NemoClaw installer:
 
```bash
curl -LsSf https://raw.githubusercontent.com/NVIDIA/NemoClaw/main/install.sh | bash
```
 
Because you already have a compatible OpenShell installed, the installer's
"Installing OpenShell CLI" step should detect version 0.0.32 and pass its
compatibility check instead of trying to install a newer version.
 
Reload your shell:
 
```bash
source ~/.bashrc
```
 
Verify NemoClaw is installed:
 
```bash
nemoclaw help
```
 
You should see the NemoClaw command list including `onboard`, `list`, `connect`,
and more.
 
> **Note:** Use `nemoclaw help` to list commands. `nemoclaw --version` prints the
> version but does not list subcommands.
 
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
