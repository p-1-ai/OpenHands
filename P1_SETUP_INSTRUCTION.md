# Openhands set up instructions with uv

## Preliminaries

<details>
  <summary>Install node/npm</summary>
  
#### For linux/macOS:

```shell
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 22

# Verify the Node.js version:
node -v # Should print "v22.15.0".
nvm current # Should print "v22.15.0".

# Verify npm version:
npm -v # Should print "10.9.2".
```

</details>

<details>
<summary>[optional] Install poetry </summary>
  
```shell
curl -sSL https://install.python-poetry.org | python3.12 -
```

</details>
<details>
<summary>Install uv</summary>
  
```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
```

</details>

## Repo/environment setup

- In the OpenHands repo root, create a python 3.12 environment with uv and activate:

```shell
uv venv .venv -p 3.12
source .venv/bin/activate
```

- Install dependencies and build:

```bash
make build
```

## Configuration

> [!NOTE]
> Configuration changes generally require a hard restart of the backend server to take effect.

### MCP

- If you want to use MCP tools, create a `config.toml` at the repo's root folder; you can comment out servers to disable them:

```toml
[[mcp.sse_servers]]
url = "http://localhost:8123"
```

- You can add more MCP https servers by appending to the list `mcp_servers`

### LLM and agents

You can setup several named LLM configs in `config.toml`:

```toml
[llm.gpt-4o]
model="gpt-4o"
api_key="XXXX"

[llm.claude-3-7]
model="claude-3-7-sonnet-20250219"
api_key="YYYY"

[llm.llama3]
model="hosted_vllm/llama3"
base_url="http://123.123.123.123:8888/v1"
native_tool_calling = 'true'
```

Note that for self-hosted vllm models, you should specify `native_tool_calling = 'true'`.

You can then configure specific agents to use particular LLM config:

```toml
[agent.CodeActAgent]
llm_config="claude-3-7"

[agent.ArchieAgent]
llm_config="llama3"
```

## Running

The simplest way to run is just with `make run`.

If you're developing or debugging, you can run the frontend and backend separately by running `make start-frontend` and `make start-backend` in separate terminals.

To setup debugging, start only the frontend, and then use this VSCode configuration:

  ```json
  {
      "name": "Python Debugger: Debug Openhands Backend",
      "type": "debugpy",
      "request": "launch",
      "module": "uvicorn",
      "args": [
          "openhands.server.listen:app",
          "--host",
          "0.0.0.0",
          "--port",
          "3000"
      ],
      "cwd": "${workspaceFolder}",
      "python": "${workspaceFolder}/.venv/bin/python",
      "justMyCode": true
  }
  ```

> [!NOTE]
> For safari users: may need to clear cache for openhands to work
