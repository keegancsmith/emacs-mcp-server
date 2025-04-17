# Emacs MCP Server

This is a simple MCP (Model Context Protocol) server that allows language models to interact with Emacs through `emacsclient --eval` commands and view the current state of Emacs.

## Features

The server provides three tools:

- `emacs_eval`: Evaluates an Emacs Lisp expression and returns the result
- `emacs_get_visible_text`: Gets the text currently visible in the Emacs window
- `emacs_get_context`: Gets contextual information about the current Emacs state (buffer, mode, point position, etc.)

## Requirements

- Node.js
- Emacs with the server running (`M-x server-start` in Emacs)

## Installation

No installation is needed if you use npx. Just make sure you have Node.js and npm installed.

## Usage with Claude for Desktop

To use this MCP server with Claude for Desktop:

1. Make sure Emacs is running with the server started (`M-x server-start`)
2. Add the server to your Claude for Desktop configuration at:

   - MacOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Linux: `~/.config/Claude/claude_desktop_config.json`
   - Windows: `%AppData%\Claude\claude_desktop_config.json`

Example configuration:

```json
{
  "mcpServers": {
    "emacs-mcp": {
      "command": "npx",
      "args": [
        "-y", "@keegancsmith/emacs-mcp-server"
      ]
    }
  }
}
```

3. Restart Claude for Desktop

## How It Works

The server uses `emacsclient --eval` to send Lisp expressions to a running Emacs instance. All operations are performed in the context of the currently visible buffer in the selected window. This allows language models to:

- Query information from Emacs (buffers, modes, variables, etc.)
- Execute commands in Emacs
- Manipulate text and buffers
- Access Emacs functionality
- See what's currently visible in Emacs
- Get cursor position and other buffer context

## Example Usage

Once the server is set up, you can use it in Claude to interact with Emacs:

```
Can you show me the list of buffers in my Emacs?
```

Claude will use the `emacs_eval` tool with an expression like `(buffer-list)` to retrieve and display your buffers.

```
What's currently visible in my Emacs window?
```

Claude can use `emacs_get_visible_text` to see the text content that's currently displayed.

```
What file am I currently editing in Emacs?
```

Claude will use `emacs_get_context` to provide information about the current buffer.

## Security Considerations

This server allows executing arbitrary Emacs Lisp code, which has full access to your Emacs environment and potentially your system. Use with trusted LLM providers and be cautious about the expressions that might be executed.

## License

MIT