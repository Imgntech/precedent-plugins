# Precedent plugins

Version 1.2.0. Connects to https://precedent.biz/mcp. Contains skills and remote MCP configuration only. No Python, npm package, local server, credentials, or application source.

## Install

Add https://github.com/gordonbyron/precedent-plugins as a marketplace in your client's plugin settings, then install Precedent. In Claude use Customize → Plugins → Personal → Add marketplace. In Codex use the plugin marketplace controls.

CLI equivalents:

```sh
codex plugin marketplace add gordonbyron/precedent-plugins
codex plugin add precedent@precedent
```

Inside Claude Code:

```text
/plugin marketplace add gordonbyron/precedent-plugins
/plugin install precedent@precedent
```

Authorize the plugin's MCP connection in your client, choose your workspace, and start a fresh conversation. Ask: “Set up Precedent for this project. Ask where to record decisions and which area to check for guidance.” The assistant validates your choices and saves a credential-free .precedent.json where project files are available. Without files, choices last for the conversation. Project linking requires a deployment exposing setup_project and get_node_context. If those tools are unavailable, the skill reports that the server needs an update instead of claiming the project is linked.

## Optional reminders

Ask to use the enable-reminders skill in a trusted local project. It explains prompt sharing, checks native MCP hook support, and configures only an explicitly requested project reminder. No hooks run just because you installed the plugin. Chat-only clients use the decision-librarian skill without local hooks.

## Updates and removal

Use your client's plugin manager to refresh this marketplace and update or uninstall Precedent. Start a fresh session afterwards. Remove any project reminder before uninstalling; it lives in project settings. Uninstalling does not revoke your OAuth grant: revoke unused connections in Precedent Settings → Agents. Keep .precedent.json if you plan to reconnect.

## Moving from a script or manual setup

Use one Precedent connection and one copy of the skill per client. Uninstall the old managed setup with its original client, scope and node options, or remove the manual connection and standalone skill in your client. Remove old Precedent hooks while preserving unrelated entries, then install this plugin and authorize it. Do not delete .precedent.json. Revoke old grants only after checking that no other installation uses them.

Self-hosted users should use their deployment's MCP URL and skill download instead of this package. Full instructions: https://precedent.biz/docs/agents

## Release contents

Generated from the canonical skills and integration manifests maintained by Precedent. Releases include no executable hooks. File changes and version history are available through this repository's Git history.
