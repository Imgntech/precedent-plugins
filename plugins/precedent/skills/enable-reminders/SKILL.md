---
name: enable-reminders
description: Enable, disable, or repair optional Precedent decision reminders when the user explicitly asks. Requires a local project, a connected Precedent MCP server, and native mcp_tool hook support in Codex or Claude Code.
---

# Precedent project reminders

Only act when the user requests reminder setup, repair, or removal. Installing the plugin or linking a project does not enable reminders.

1. Explain that a reminder sends each submitted prompt to the connected Precedent deployment, searches related decisions, and can wait up to three seconds. It does not capture transcripts or create drafts. Obtain agreement to prompt sharing if the user's request did not already include it.
2. Identify the current client and check its installed version's native `mcp_tool` hook support using its help or official hook reference. Do not invent a command-hook fallback. Without supported hooks or project files, explain that decision-librarian still works when invoked and stop setup.
3. Use decision-librarian to load and validate this project's `.precedent.json` with `setup_project`. Resolve any deployment, workspace, or node mismatch first. Never install a workspace-wide reminder as a fallback for invalid project settings.
4. Inspect configured MCP connections and use the exact name of the connected Precedent server. Plugin names can be scoped: Claude Code uses `plugin:<plugin-name>:<server-name>` (normally `plugin:precedent:precedent`). Codex names must be read from its active configuration or tools, not guessed from Claude's convention. Verify `get_agent_context` succeeds with the saved project and a short sample query before editing hooks. Do not add another server or OAuth grant.
5. Read `.claude/settings.json` for Claude Code or `.codex/hooks.json` for Codex in this project. Invalid existing JSON requires repair with the user; never overwrite it. Preserve all unrelated settings, matchers and hooks. Check project and user hooks for duplicate Precedent reminders. If an account reminder already runs, explain the overlap and resolve it before adding another. Do not edit account-wide hooks without an explicit request.
6. Merge exactly one `UserPromptSubmit` entry with this hook, substituting the actual server name and the entire validated project object:

```json
{
  "type": "mcp_tool",
  "server": "EXACT_CONNECTED_SERVER_NAME",
  "tool": "get_agent_context",
  "input": { "prompt": "${prompt}", "project": {} },
  "timeout": 3
}
```

The containing structure is `{"hooks":{"UserPromptSubmit":[{"hooks":[HOOK]}]}}`. Never write the placeholder name or empty project. Update an existing matching Precedent hook instead of appending a duplicate. Do not add SessionStart, Stop, blocking hooks, tokens, scripts, or transcript capture.
7. Show the resulting project file and connection name. Ask the user to review and trust the hook through their client and start a fresh session. Do not grant hook trust on their behalf or claim the hook ran until a submitted prompt has actually produced a hook result. Disconnected or timed-out hooks supply no context; they do not block work. Empty search results are valid and do not establish compliance.

For removal, delete only this connection's `get_agent_context` reminder, preserving unrelated entries and settings. Remove it before uninstalling the plugin. If project choices change, update the reminder's saved `project` object as part of relinking. If the plugin's connection name changes after an update, repair the reminder using the newly observed name.
