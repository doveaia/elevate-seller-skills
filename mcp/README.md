# Elevate Seller MCP server: configuration files per AI agent

The Elevate Seller MCP server is a remote server (streamable HTTP) at `https://api.elevateseller.com/mcp`. It authenticates with an API key of your organisation, sent as the header `Authorization: Bearer <API key>`. It does not use OAuth.

Each file in this folder declares the server under the name `elevate-seller` and reads the key from the environment variable **`ELEVATE_SELLER_API_KEY`**, so no file contains a key. Copy the file for your agent to the place shown below. If that file already exists, merge the `elevate-seller` entry into it instead of replacing the file.

Claude Code users who install the `elevate-seller` plugin do not need this folder: the plugin connects the server itself and asks for the key when it is enabled.

## 1. Set `ELEVATE_SELLER_API_KEY`

Create the key in Elevate Seller under **Settings › API keys**. Choose **Read and write** if the agent should record its opinions; a read-only key lets it read data only.

- **macOS / Linux**: add this line to your shell profile (`~/.zshrc`, `~/.bashrc`, …), then open a new terminal:

  ```sh
  export ELEVATE_SELLER_API_KEY="<your API key>"
  ```

- **Windows** (PowerShell or cmd), then open a new terminal:

  ```bat
  setx ELEVATE_SELLER_API_KEY "<your API key>"
  ```

Desktop apps started from the Dock, the Finder or a launcher on macOS do not read your shell profile, so they do not see the variable. Start them from a terminal where the variable is set (for example `code .` or `cursor .`), or set it for the macOS session with `launchctl setenv ELEVATE_SELLER_API_KEY "<your API key>"` and restart the app.

## 2. Copy the file for your agent

| Agent | File in this folder | Copy it to | Key syntax in the file | Status | Documentation (read 2026-11-27) |
| --- | --- | --- | --- | --- | --- |
| Claude Code (without the plugin) | [`claude-code/.mcp.json`](claude-code/.mcp.json) | `.mcp.json` at the root of your project | `${ELEVATE_SELLER_API_KEY}` | Verified locally 2026-11-27, Claude Code 2.1.326 | <https://code.claude.com/docs/en/mcp> |
| OpenCode | [`opencode/opencode.json`](opencode/opencode.json) | `opencode.json` at the root of your project, or `~/.config/opencode/opencode.json` for all projects | `{env:ELEVATE_SELLER_API_KEY}` | Verified locally 2026-11-27, OpenCode 2.3.0 | <https://opencode.ai/docs/mcp-servers/>, schema <https://opencode.ai/config.json> |
| Hermes Agent | [`hermes/config.yaml`](hermes/config.yaml) | merge into `~/.hermes/config.yaml` | `${ELEVATE_SELLER_API_KEY}` | Verified locally 2026-11-27, Hermes Agent 0.23.4 | <https://hermes-agent.nousresearch.com/docs/reference/mcp-config-reference> |
| OpenAI Codex CLI | [`codex/config.toml`](codex/config.toml) | merge into `~/.codex/config.toml`, or `.codex/config.toml` in a trusted project | `bearer_token_env_var = "ELEVATE_SELLER_API_KEY"` | Verified locally 2026-11-27, Codex 0.135.0 | <https://learn.chatgpt.com/docs/extend/mcp?surface=cli> |
| Gemini CLI | [`gemini-cli/settings.json`](gemini-cli/settings.json) | merge into `.gemini/settings.json` in your project, or `~/.gemini/settings.json` | `${ELEVATE_SELLER_API_KEY}` | Verified locally 2026-11-27, Gemini CLI 0.6.1 | <https://geminicli.com/docs/tools/mcp-server/>, <https://geminicli.com/docs/reference/configuration/> |
| VS Code with GitHub Copilot | [`vscode/.mcp.json`](vscode/.mcp.json) | `.mcp.json` at the root of your workspace, or `~/.copilot/mcp-config.json` for all workspaces | `${ELEVATE_SELLER_API_KEY}` | From the docs only, not tried | <https://code.visualstudio.com/docs/agents/reference/mcp-configuration> |
| Cursor | [`cursor/mcp.json`](cursor/mcp.json) | `.cursor/mcp.json` in your project, or `~/.cursor/mcp.json` | `${env:ELEVATE_SELLER_API_KEY}` | From the docs only, not tried | <https://cursor.com/docs/context/mcp> |
| Windsurf | [`windsurf/mcp_config.json`](windsurf/mcp_config.json) | merge into Windsurf's `mcp_config.json`: use **Open MCP config file** in Cascade's MCP menu (`~/.config/devin/mcp_config.json` on macOS and Linux, `%AppData%/devin/mcp_config.json` on Windows) | `${env:ELEVATE_SELLER_API_KEY}` | From the docs only, not tried | <https://docs.devin.ai/windsurf/plugins/cascade/mcp> (the Windsurf documentation now redirects there) |
| Claude Desktop, claude.ai, Claude mobile | [`claude-desktop.md`](claude-desktop.md) (steps, no file) | custom connector in Claude's settings, with a request header | key typed in Claude's settings | Official method, from the docs only: the connection is made from Anthropic's cloud | <https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp>, <https://claude.com/docs/connectors/building/authentication> |

## 3. Check the connection

| Agent | Command |
| --- | --- |
| Claude Code | `claude mcp list` (the first time, run `claude` in the project and approve the server from `.mcp.json`) |
| OpenCode | start a session (`opencode`); the server's tools are available in it |
| Hermes Agent | `hermes mcp test elevate-seller` |
| Codex CLI | `codex mcp list` shows the configured server; its tools are available in a Codex session |
| Gemini CLI | `gemini mcp list` |
| VS Code, Cursor, Windsurf | the MCP panel of the app |

With a valid key the server lists its tools (twelve at the time of writing). Without the variable, or with a wrong key, the server answers `401` and the agent reports the server as failed or disconnected.

## Notes per agent

- **Claude Code**: the project `.mcp.json` uses `${ELEVATE_SELLER_API_KEY}` from your environment. The plugin's own server, declared in `.claude-plugin/plugin.json`, uses `${user_config.api_key}` instead: that value is the key Claude Code asks for when the plugin is enabled, and it only works inside the plugin. Claude Code asks you to approve the servers of a project `.mcp.json` before it connects to them.
- **OpenCode**: the file uses the `mcp.servers` form of the OpenCode 2.x schema (the older form, with servers directly under `mcp`, is deprecated), with `"oauth": false` because the server takes an API key header. With OpenCode 2.3.0, `opencode mcp list` printed "No MCP servers configured" even for a server added with `opencode mcp add`; the server was connected in sessions all the same.
- **Hermes Agent**: you can also put `ELEVATE_SELLER_API_KEY=<your API key>` in `~/.hermes/.env`. If the variable is not set, Hermes refuses to connect and names the variable.
- **Codex CLI**: `bearer_token_env_var` makes Codex send `Authorization: Bearer <value of the variable>`. A project `.codex/config.toml` is read only in trusted projects.
- **Gemini CLI**: the Gemini CLI documentation says it was replaced by Antigravity CLI on 2026-06-18. Use `httpUrl` (streamable HTTP), not `url` (SSE).
- **VS Code / GitHub Copilot**: the file uses the portable `mcpServers` format of the Copilot harness, where variables are read in header values. VS Code's older `.vscode/mcp.json` format (Local harness, deprecated, to be removed in February 2027) documents only input prompts (`${input:…}`) for headers, so no file is provided for it.
- **Claude Code and VS Code in the same project**: both read `.mcp.json` at the project root, but Claude Code skips an entry that has `"tools": ["*"]` (checked with Claude Code 2.1.326). In such a project, put the Claude Code file at the root and the VS Code entry in `~/.copilot/mcp-config.json`.
- **Cursor**: Cursor reads `${env:NAME}` in `url` and `headers`. Start Cursor from a terminal if it does not see the variable.
- **Windsurf**: the documentation lists `serverUrl` (or `url`) and `${env:NAME}` in `headers`.

## Agents without a file

- **Zed**: its documentation shows a remote server with an `Authorization` header in `context_servers`, but documents no way to read an environment variable there. The only documented option is to write the key in your own Zed settings, which this repository does not ship. See <https://zed.dev/docs/ai/mcp>.
- **Goose**: substitution of environment variables in the headers of a `streamable_http` extension (`env_keys` plus `${VAR}` in `headers`) was added to Goose's source code in November 2025 (pull request block/goose#5439) but is not in its documentation (<https://goose-docs.ai/docs/guides/config-files>), and the Goose version on the test machine was too old to try it. No file is shipped until it is verified.
- **Claude Desktop configuration file**: `claude_desktop_config.json` takes only local servers. Use the custom connector described in [`claude-desktop.md`](claude-desktop.md).

---

# Serveur MCP Elevate Seller : fichiers de configuration par agent IA

Le serveur MCP d'Elevate Seller est distant (HTTP streamable), à l'adresse `https://api.elevateseller.com/mcp`. Il s'authentifie avec une clé API de ton organisation, envoyée dans l'en-tête `Authorization: Bearer <clé API>` (pas d'OAuth).

Chaque fichier de ce dossier déclare le serveur sous le nom `elevate-seller` et lit la clé dans la variable d'environnement **`ELEVATE_SELLER_API_KEY`** : aucun fichier ne contient de clé. Copie le fichier de ton agent à l'endroit indiqué dans le tableau ci-dessus ; si ce fichier existe déjà, fusionne l'entrée `elevate-seller` au lieu de le remplacer.

Avec Claude Code, le plugin `elevate-seller` connecte déjà le serveur et demande la clé à l'activation : ce dossier est inutile.

Définir la variable (clé créée dans Elevate Seller, **Paramètres › Clés API** ; « Lecture et écriture » pour que l'agent dépose ses avis) :

- **macOS / Linux** : ajoute `export ELEVATE_SELLER_API_KEY="<ta clé API>"` à ton profil shell (`~/.zshrc`, `~/.bashrc`…), puis ouvre un nouveau terminal.
- **Windows** : `setx ELEVATE_SELLER_API_KEY "<ta clé API>"`, puis ouvre un nouveau terminal.
- Sur macOS, une application lancée depuis le Dock ou le Finder ne lit pas le profil shell : lance-la depuis un terminal (`code .`, `cursor .`) ou utilise `launchctl setenv ELEVATE_SELLER_API_KEY "<ta clé API>"` puis redémarre l'application.

Claude Desktop, claude.ai et Claude mobile : pas de fichier, un connecteur personnalisé avec un en-tête de requête, voir [`claude-desktop.md`](claude-desktop.md).
