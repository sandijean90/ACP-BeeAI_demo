# 🐝 BeeAI Agent Template

## 🚧 Disclaimer

> The current MCP-based implementation should be treated as temporary and exploratory. Its main purpose is to enable integration of various agents into the platform and to explore underlying protocols, transports, and possible multi-agent patterns.  
> 
> **In Q2 2025, we expect to rebuild the SDK from the ground up.**

---

## 🔧 Setup

1. Navigate to the `demo-agent` folder of this repo and run:

    ```bash
    uv sync && source .venv/bin/activate
    ```

2. Start the BeeAI background service:

    ```bash
    brew services start beeai
    ```

3. Configure the LLM provider in the platform:

    ```bash
    beeai env setup
    ```

---

## 🧠 Agent Template

This is an example template for a Python agent that can be used in the BeeAI platform.

To get started, fork this repository and creat your own .env file with your openai api key.

---

## 📦 Pre-requisites

- Python 3.11 or higher  
- UV package manager: [https://docs.astral.sh/uv/](https://docs.astral.sh/uv/)

---

## 🛠 Implementing an Agent
Set your enviorment variables (openai key) in your CLI for the session:
```bash
export OPENAI_API_KEY= <API key>
   ```
Run the agent locally:
```bash
uv run openai-agent
   ```
Your agents should now be started on http://localhost:8000, you can now list agents using the beeai CLI with a few extra parameters:


```bash
env BEEAI__HOST=http://localhost:8000 \
  BEEAI__MCP_SSE_PATH='/sse' \
  beeai list
   ```
Now you can run the agent locally using :
```bash
env BEEAI__HOST=http://localhost:8000 \
  BEEAI__MCP_SSE_PATH='/sse' \
  beeai run openai-agent
   ```

---

## Adding your Agent to the beeai Platform
There are two ways to add an agent: locally or through a public github link
The agents can also be managed by beeai, or you can decide to manage them locally.

#### Option 1: Locally
The easiest way to add agents to beeai is to use a local provider manifest. It might be useful especially during rapid agent development to keep the agent code locally and iterate quickly. You have two options:

##### Add unmanaged agent
This means that you will have to run the agent server yourself, the platform will not attempt to start, stop or configure it. You can add it in two simple steps:

1. Start the agent server 
```bash
uv run beeai-agents
   ```
2. Add the provider to beeai
```bash
beeai provider add file://beeai-provider-unmanaged.yaml
   ```
Note: You will need to start the agent using uv run beeai-agents everytime you want to interact with the agent through the platform.

##### Add managed agent

This means that the beeai platform will manage the agent server process (you won't have to run uv run beeai-agents, the platform will do it for you). All you need to do is to add the local provider manifest:
```bash
beeai provider add file://beeai-provider-local.yaml
   ```

The platform will register the server and run it automatically from now on.

Note: If you want to update your code in a managed provider, you need to bump package version in pyproject.toml and then re-register the provider

#### Option 2: Add Agents From GitHub
If you want to share your agent with others using the beeai platform, the easiest way is to use a GitHub link to your repository you created with the template:

Modify beeai-provider.yaml manifest with your repository url:
```bash
vim beeai-provider.yaml
   ```

Add provider manifest:
```bash
beeai provider add https://github.com/i-am-bee/beeai-agent-starter.git
   ```

Note: To manage versions properly and prevent automatic updates or breaking changes we recommend creating a tag (for example agents-v0.0.1 which you then use in beeai-provider.yaml).

To release new version - create a new tag and modify beeai-provider.yaml
To update the agent in beeai you'll need to:

```bash
beeai provider remove <ID>
beeai provider add file://beeai-provider-local.yaml
   ```
---
## ⚙️ Running a workflow with 2 agents and traceability:
##### Running traceability:
1. install arize-phoenix
```bash
brew install i-am-bee/beeai/arize-phoenix
   ```
2. start the service:
```bash
start arize-phoenix
   ```
3. open the gui to see traceability http://localhost:6006

##### Running the a sequential workflow
```bash
beeai agent run sequential-workflow '{
      "steps": [
          {"agent": "gpt-researcher", "instruction": "Write an essay about AI agents."},
          {"agent": "aider", "instruction": "Create a plain HTML & simple CSS styled website based on the provided essay."}
      ]
  }'
   ```
---
## Removing a provider that has already been added to beeai
1. find the short id of the provider you wish to remove
```bash
beeai provider list 
   ```
2. remove the provider
```bash
beeai provider remove <ID> 
```

## Other commands
List all available agents
```bash
beeai list 
   ```
 Stopping the platform
 ```bash
brew services stop beeai
   ```
Restarting the platform
```bash
brew services restart beeai
   ```
launching the UI
 ```bash
beeau ui
   ```
