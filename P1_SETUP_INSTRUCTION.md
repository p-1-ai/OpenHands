## Openhands set up instructions with uv
- If not found, install uv:
```
curl -LsSf https://astral.sh/uv/install.sh | sh
````
- Create `python3.12` environment with uv and activate:
```
uv venv .venv -p 3.12
source .venv/bin/activate
```

- Install nodejs (based on your system, pick the right script from https://nodejs.org/en/download), e.g. for linux:
```
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

- Build the project: `make build`

- Create a `config.toml` at the repo's root folder:
```
[mcp]
mcp_servers=["http://localhost:3003/sse", "http://localhost:8888/sse"]
```
  - You can add more MCP https servers by appending to the list `mcp_servers`

- Launch openhands server by either:
  + `make run`, or
  + `make start-frontend` then `make start-backend` -> easy for debugging with the following config:
    ```
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

**NOTES**
- For safari users: may need to clear cache for openhands to work

