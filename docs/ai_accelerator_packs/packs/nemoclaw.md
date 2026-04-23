# NemoClaw Autonomous Agent Sandbox

> **Experimental** — This pack is still in experiment mode and not meant for production use cases.

The **NemoClaw Starter Pack** is an AI Accelerator Pack that deploys a secure, sandboxed execution environment for [NVIDIA OpenClaw](https://github.com/NVIDIA/OpenClaw) autonomous agents on Oracle Cloud Infrastructure (OCI). It uses [NemoClaw](https://github.com/NVIDIA/NemoClaw) with [OpenShell](https://docs.nvidia.com/nemoclaw/latest/) runtime protection to isolate agent actions inside a Landlock + seccomp + network-namespaced sandbox.

## What You Get

- **Hardware:** A BM.GPU4.8 bare metal instance (8x A100 80GB) for self-hosted inference, or CPU-only infrastructure when using a cloud API provider. Runs on Oracle Kubernetes Engine (OKE).
- **Software:** NemoClaw + OpenShell sandbox with:
  - **OpenClaw dashboard**: Browser-based chat UI for interacting with autonomous agents
  - **Web terminal**: Optional ttyd-based terminal for direct sandbox access (`nemoclaw connect`, `openclaw tui`)
  - **Provider selection**: Self-hosted NVIDIA NIM, OpenAI API, or Anthropic API for inference
  - **Security tiers**: Restricted, balanced, or open policy tiers controlling sandbox permissions

## Use Cases

- **Autonomous coding agents**: Run AI agents that can write, execute, and iterate on code in a sandboxed environment without risk to host infrastructure.
- **Research and web tasks**: Agents can search the web, download data, and produce reports — all within policy-controlled network boundaries.
- **Tool-use evaluation**: Test and evaluate agent capabilities (code execution, file I/O, web access) with configurable security policies.

## Security

OpenShell enforces security at the proxy level — all network traffic from the sandbox passes through a policy-aware proxy that filters requests by host, port, HTTP method, and URL path. Key protections:

- **Network isolation**: Only explicitly allowed endpoints are reachable. Random internet access is blocked.
- **Method-level control**: Policies can allow GET but block POST/DELETE on specific APIs.
- **IMDS blocked**: The OCI Instance Metadata Service (169.254.169.254) is not accessible from inside the sandbox, preventing instance principal credential theft.
- **Filesystem isolation**: Landlock restricts file access to designated read-only and read-write paths.

**Note on DinD:** The workspace container runs in privileged mode (required for Docker-in-Docker). This is a possible security risk — agent isolation is enforced by OpenShell inside the DinD environment, not by Kubernetes pod-level security. The workspace container itself has elevated privileges on the node.

## Deployment and Access

You can deploy the NemoClaw Starter Pack from the **OCI Console**. Under **AI Accelerator Packs**, select NemoClaw, choose your inference provider and security tier, add the portal credentials, and click Create.

After deployment you get:

- **OpenClaw Dashboard**: The tokenized dashboard URL is in the stack outputs. This is the main interface for interacting with agents.
- **Web Terminal**: A browser-based terminal for running CLI commands inside the sandbox.
- **OCI AI Blueprints Portal**: The Blueprints portal URL for managing deployments.
