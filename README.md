# Copilot MCP Demo

A demonstration project showcasing the integration of GitHub's Model Context Protocol (MCP) Server with IntelliJ IDEA and GitHub Copilot, enabling seamless AI-assisted development with direct access to repository management capabilities.

## Overview

This project demonstrates how to leverage the GitHub MCP Server within your development environment to enhance productivity through intelligent code assistance and repository management. By integrating GitHub Copilot with MCP, you can interact with issues, pull requests, and repository data directly from the IDE chat interface.

---

## Getting Started

### Prerequisites

Before setting up the GitHub MCP Server integration, ensure you have the following installed:

- **IntelliJ IDEA** (2024.1 or later)
- **GitHub Copilot** extension for IntelliJ
- **Node.js** (v18 or later) for running the MCP server
- **Git** (latest version)

### Step 1: Generate a GitHub Personal Access Token

To enable secure communication between the MCP server and GitHub's API:

1. Navigate to **GitHub Settings** → **Developer Settings** → **Personal Access Tokens (Classic)**
2. Click **Generate new token (classic)**
3. Provide a descriptive name (e.g., "Copilot MCP Server")
4. Select the following scopes:
   - `repo` – Full control of private and public repositories
   - `user` – Read/write access to user profile data
5. Click **Generate token** and **copy the token immediately** (you won't be able to view it again)
6. Store the token securely in a password manager or environment variable

**Security Note:** Never commit your personal access token to version control. Treat it as a sensitive credential.

### Step 2: Configure the MCP Server in IntelliJ IDEA

#### Accessing MCP Configuration

1. Open **Settings** → **Tools** → **GitHub Copilot** → **Model Context Protocol (MCP)**
2. Click the **Configure** button to open the global `mcp.json` configuration file

#### Creating/Updating mcp.json

The MCP configuration file should be located at:
```
~/.config/github-copilot/intellij/mcp.json
```

Add the following configuration for the GitHub MCP Server:

```json
{
    "servers": {
        "github": {
            "type": "stdio",
            "command": "npx",
            "args": [
                "-y",
                "@modelcontextprotocol/server-github"
            ],
            "env": {
                "GITHUB_PERSONAL_ACCESS_TOKEN": "your_personal_access_token_here"
            }
        }
    }
}
```

**Configuration Breakdown:**
- **`type: "stdio"`** – Specifies the server communication method
- **`command: "npx"`** – Uses Node Package Manager to run the server
- **`args`** – Arguments passed to npx to install and run the GitHub MCP server package
- **`env.GITHUB_PERSONAL_ACCESS_TOKEN`** – Your GitHub personal access token for API authentication

**Security Best Practice:** Consider using environment variables instead of hardcoding the token:

```json
{
    "servers": {
        "github": {
            "type": "stdio",
            "command": "npx",
            "args": [
                "-y",
                "@modelcontextprotocol/server-github"
            ],
            "env": {
                "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
            }
        }
    }
}
```

Then set the environment variable:
```bash
export GITHUB_TOKEN="your_personal_access_token_here"
```

### Step 3: Initialize the MCP Server

1. **Restart IntelliJ IDEA** to load the new server configuration
2. IntelliJ will automatically detect and initialize the GitHub MCP server on startup

### Step 4: Enable Agent Mode in GitHub Copilot Chat

1. Open the **GitHub Copilot Chat** panel in IntelliJ (View → GitHub Copilot Chat)
2. In the chat window header, locate the **mode selector** (usually showing "Auto")
3. Switch from **Auto mode** to **Agent mode** to enable tool-calling capabilities
4. A confirmation message should appear indicating agent mode is active

### Step 5: Enable the GitHub Tool

1. In the GitHub Copilot Chat window, click the **Configure Tools** button (wrench icon)
2. In the tools configuration panel, locate the **github** tool
3. Toggle it **ON** to grant GitHub Copilot permission to execute repository-level actions
4. Close the configuration panel

You're now ready to use GitHub Copilot with full MCP capabilities!

---

## Using the GitHub MCP Server

### Interacting with Issues and Pull Requests

Once configured, you can interact with your repository directly from the Copilot Chat in Agent mode:

#### Example Queries:

**View Issues:**
```
@copilot Can you show me all open issues in this repository?
```

**Analyze Pull Requests:**
```
@copilot What are the current pull requests and their statuses?
```

**Create and Manage Issues:**
```
@copilot Create a new issue titled "Implement feature X" with description "...details here..."
```

**Review Code with Context:**
```
@copilot Review the recent changes in PR #123 and suggest improvements
```

### Key Capabilities

With the GitHub MCP Server enabled, Copilot can:

- **List and filter issues** by status, assignee, or labels
- **Manage pull requests** – view, comment on, and suggest changes
- **Access repository metadata** – branches, commits, and history
- **Create and update issues** with templates and descriptions
- **Provide context-aware suggestions** based on your repository's state

---

## Troubleshooting

### MCP Server Not Connecting

**Problem:** The GitHub tool is grayed out or unavailable in Copilot Chat.

**Solutions:**
1. Verify the `mcp.json` file is in the correct location: `~/.config/github-copilot/intellij/mcp.json`
2. Check that your Personal Access Token is valid and hasn't expired
3. Ensure Node.js is installed and accessible: `node --version`
4. Restart IntelliJ IDEA completely
5. Check IDE logs: **Help** → **Show Log in Explorer** for error messages

### Authentication Errors

**Problem:** "Authentication failed" or "Invalid token" errors.

**Solutions:**
1. Verify your Personal Access Token hasn't been revoked in GitHub Settings
2. Confirm the token has the required scopes: `repo` and `user`
3. Check for typos in the token value in `mcp.json`
4. Generate a new token if the old one is compromised or expired

### Agent Mode Not Available

**Problem:** Cannot switch to Agent mode in Copilot Chat.

**Solutions:**
1. Update GitHub Copilot to the latest version: **Help** → **Check for Updates**
2. Ensure your IntelliJ IDEA version is 2024.1 or later
3. Restart the IDE after updating

---

## Project Structure

```
copilot-mcp-demo/
├── README.md                 # This file
├── .git/                     # Git repository metadata
├── .gitignore               # Git ignore rules
├── .idea/                   # IntelliJ IDEA configuration
└── src/                     # Source code directory
```

---

## Best Practices

1. **Token Security:** Use environment variables for sensitive credentials rather than hardcoding them in `mcp.json`
2. **Regular Updates:** Keep Node.js and the GitHub Copilot extension updated for optimal performance
3. **Token Rotation:** Periodically rotate your Personal Access Token for security
4. **Limited Scope:** Generate separate tokens for different tools/services to minimize risk
5. **Audit Logs:** Monitor GitHub's audit log for MCP server access patterns

---

## Additional Resources

- [GitHub MCP Server Documentation](https://github.com/modelcontextprotocol/servers/tree/main/src/github)
- [GitHub Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [IntelliJ IDEA GitHub Copilot Extension](https://plugins.jetbrains.com/plugin/17718-github-copilot)
- [Model Context Protocol Overview](https://modelcontextprotocol.io/)

---

## Contributing

Contributions are welcome! Please feel free to open issues or submit pull requests with improvements and enhancements.

---

## License

This project is licensed under the MIT License. See the LICENSE file for details.

---

## Support

If you encounter any issues or have questions about the GitHub MCP Server setup, please:

1. Check the [Troubleshooting](#troubleshooting) section above
2. Open an issue in this repository with detailed error messages
3. Consult the official GitHub MCP Server documentation

---

**Last Updated:** March 5, 2026

