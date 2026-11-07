# Safety Model and Controlled Autonomy

Setup Desk is designed to execute routine developer setup tasks autonomously while strictly enforcing safety boundaries. Autonomous agent workflows must never risk the host operating system, expose secrets, or modify files outside their designated domain.

---

## The Principle of Controlled Autonomy

Setup Desk adheres to the concept of **controlled autonomy**:
Agents are granted permission to explore, provision, build, test, and repair software within an **assigned, isolated project workspace**. Beyond that perimeter, autonomy ceases, and operations require explicit human intervention or are halted immediately.

```
+---------------------------------------------------------------+
|                      HOST OPERATING SYSTEM                    |
|  [Global Filesystem] [System Registries] [Credentials / Keys] |
|                                                               |
|   +-------------------------------------------------------+   |
|   |         ASSIGNED PROJECT WORKSPACE (SANDBOX)          |   |
|   |                                                       |   |
|   |   - Repository clone                                  |   |
|   |   - Project-local virtual environments (.venv, etc.)  |   |
|   |   - Local package manifests & config files            |   |
|   |   - Verified SETUP.md generation                      |   |
|   |                                                       |   |
|   |   [Scout: Read-Only]  [Setup: Exec]  [Repair: Fix]    |   |
|   +-------------------------------------------------------+   |
|                                                               |
|        ANY OPERATION ATTEMPTING TO CROSS THIS BOUNDARY        |
|               IS IMMEDIATELY HALTED / REJECTED                |
+---------------------------------------------------------------+
```

---

## Permission Classification

### 1. Allowed Actions (Autonomous Scope)
When supported by the connected tools' actual permissions and sandboxing policies, agents may perform the following without requesting explicit per-command user confirmation:

- **Reading Target Files**: Inspecting documentation, source code, build scripts, and manifests.
- **Dependency Inspection**: Scanning dependencies and analyzing package lockfiles.
- **Repository Cloning**: Cloning public git repositories into the designated workspace folder.
- **Environment Isolation**: Creating project-local virtual environments (e.g., Python `venv`, local `node_modules`).
- **Dependency Installation**: Running standard package management commands within the local environment (e.g., `pip install`, `npm install`).
- **Compilation & Testing**: Executing project build commands and running automated test suites.
- **Application Smoke Testing**: Booting the application or invoking CLI commands to verify runtime stability.
- **Targeted Workspace Edits**: Modifying configuration files or dependency versions strictly within the workspace directory to repair setup failures.
- **Documentation Authoring**: Creating and updating `SETUP.md` in the project root.

### 2. Strictly Prohibited / Human Escalation Actions
Any operation that falls into the following categories **must immediately stop** and either report that the action is blocked or escalate to Slack for human authorization:

- **Elevated Privileges**: Invoking `sudo`, `su`, Windows UAC elevation, or administrative prompts.
- **Global System Configuration**: Altering host OS registries, system libraries (`/usr/lib`, `C:\Windows\System32`), or system package managers (`apt install`, `brew install`, `choco install`).
- **Global Environment Alterations**: Writing to global shell configuration files (`~/.bashrc`, `~/.zshrc`, system-level environment variables).
- **Filesystem Traversal**: Reading, creating, or modifying files outside the assigned project directory (`../`, `/etc/`, `C:\Users\`).
- **Security & Network Configuration**: Modifying firewall rules, disabling antivirus/security policies, or creating persistent background tunnels.
- **Credentials & Secrets Access**: Reading SSH keys (`~/.ssh`), API tokens, cloud credential files (`~/.aws`), or environment files containing production secrets.
- **Destructive Operations**: Executing recursive deletion on unverified paths (`rm -rf *`), wiping disk volumes, or killing system processes.
- **Untrusted Script Execution**: Running arbitrary remote shell scripts piped into bash (`curl ... | bash`).

---

## Authority of Platform Sandbox Restrictions

> [!IMPORTANT]
> **Prompt Instructions Are Not a Security Guarantee**
> 
> System instructions and agent prompt boundaries provide operational intent, but **agent prompts alone do not enforce security**. 
> 
> True system safety depends strictly on:
> 1. Native sandbox policies configured within the [Switch platform](https://github.com/sandbox-quantum/switch).
> 2. Hard file-system isolation and user-permission constraints enforced by the connected agent backends (**Codex** and **OpenCode**).
> 3. Host-level OS sandboxing, container virtualization (Docker/Podman), or non-privileged user accounts.

This document describes the intended safety model and design principles of Setup Desk, not the result of a formal cryptographic or third-party security audit.

