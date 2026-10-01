---
title: MCP Server
hidden: false
x-privacy:
  view: public
---
The OwlPay Harbor Model Context Protocol (MCP) server enables AI-powered code editors like Cursor and Windsurf, plus general-purpose tools like Claude Desktop, to interact directly with your OwlPay Harbor API and documentation.

### What is MCP?

Model Context Protocol (MCP) is an open standard that allows AI applications to securely access external data sources and tools. The OwlPay Harbor MCP server provides AI agents with:

* **Direct API access** to OwlPay Harbor functionality
* **Documentation search** capabilities
* **Real-time data** from your OwlPay Harbor account
* **Code generation** assistance for OwlPay Harbor integrations

### OwlPay Harbor MCP Server Setup

OwlPay Harbor hosts a remote MCP server at `https://harbor-developers.owlpay.com/mcp`. Configure your AI development tools to connect to this server. If your APIs require authentication, you can pass in headers via query parameters or however headers are configured in your MCP client.

<Tabs>
  <Tab title="Cursor">
    **Add to `~/.cursor/mcp.json`:**

    ```json
    {
      "mcpServers": {
        "owlpay-harbor": {
          "url": "https://harbor-developers.owlpay.com/mcp"
        }
      }
    }
    ```

    </Tab>
  <Tab title="Windsurf">
    **Add to `~/.codeium/windsurf/mcp_config.json`:**

    ```json
    {
      "mcpServers": {
        "owlpay-harbor": {
          "url": "https://harbor-developers.owlpay.com/mcp"
        }
      }
    }
    ```

  </Tab>
  <Tab title="Claude Desktop">
    **Add to `claude_desktop_config.json`:**

    ```json
    {
      "mcpServers": {
        "owlpay-harbor": {
          "url": "https://harbor-developers.owlpay.com/mcp"
        }
      }
    }
    ```

  </Tab>
</Tabs>

### Testing Your MCP Setup

Once configured, you can test your MCP server connection:

1. **Open your AI editor** (Cursor, Windsurf, etc.)
2. **Start a new chat** with the AI assistant
3. **Ask about OwlPay Harbor** - try questions like:
   * "How do I [common use case]?"
   * "Show me an example of [API functionality]"
   * "Create a [integration type] using OwlPay Harbor"

The AI should now have access to your OwlPay Harbor account data and documentation through the MCP server.
