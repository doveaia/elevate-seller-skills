# Elevate Seller — skills for AI agents

This repository is a **Claude Code plugin marketplace** and the **`elevate-seller` plugin** itself. It ships:

- the **product analysis skill** (`skills/elevate-seller-product-analysis/`): the method an AI agent follows to analyse an Amazon product with the seller's Elevate Seller grid (twelve criteria), decide, and record its opinion in the application;
- the connection to the **Elevate Seller MCP server** (`https://api.elevateseller.com/mcp`), authenticated by an API key of your organisation: Claude Code gets it with the plugin (`mcpServers` in `.claude-plugin/plugin.json`), and the folder [`mcp/`](mcp/README.md) holds ready-to-copy configuration files for other agents.

The skill contains no data of any organisation and no threshold value: the agent reads the seller's grid with the `get_analysis_grid` tool.

## Install

You need an API key, created in Elevate Seller under **Settings › API keys**. Choose **Read and write** if the agent should record its opinions.

### Claude Code

```
/plugin marketplace add doveaia/elevate-seller-skills
/plugin install elevate-seller@elevate-seller
```

Claude Code asks for the API key when the plugin is enabled and stores it as a sensitive value. The skill is then available as `/elevate-seller:elevate-seller-product-analysis`, and the `elevate-seller` MCP server is connected. You do not need the files in `mcp/`.

### claude.ai and Claude Desktop

**Customize › Plugins › Add marketplace**, then enter `doveaia/elevate-seller-skills`. The skill is installed from the marketplace. Connect the Elevate Seller MCP server as a custom connector with a request header, as described in [`mcp/claude-desktop.md`](mcp/claude-desktop.md).

### Other agents (OpenCode, Hermes, Codex, Gemini CLI, VS Code, Cursor, Windsurf, …)

```
git clone https://github.com/doveaia/elevate-seller-skills
```

1. Copy the folder `skills/elevate-seller-product-analysis/` into the folder where your agent reads its skills (OpenCode: `~/.config/opencode/skills/`, Hermes: `~/.hermes/skills/`).
2. Copy your agent's MCP configuration file from [`mcp/`](mcp/README.md). The table in `mcp/README.md` gives the file, where to copy it, and how each file was checked. If the target file already exists, merge the `elevate-seller` entry into it.
3. Set the environment variable `ELEVATE_SELLER_API_KEY` to your API key. The files read the key from it and never contain it.
   - macOS / Linux: add `export ELEVATE_SELLER_API_KEY="<your API key>"` to your shell profile (`~/.zshrc`, `~/.bashrc`, …) and open a new terminal.
   - Windows: run `setx ELEVATE_SELLER_API_KEY "<your API key>"` and open a new terminal.

A **Read and write** key is needed for the agent to record its opinions.

## Updates

The plugin version is in `.claude-plugin/plugin.json`. In Claude Code, run `/plugin marketplace update elevate-seller` to get a new version.

---

# Elevate Seller — skills pour agents IA

Ce dépôt est une **marketplace de plugins Claude Code** et le **plugin `elevate-seller`** lui-même : le **skill d'analyse de produit** (la méthode que suit un agent pour analyser un produit Amazon avec la grille Elevate Seller du vendeur, décider et déposer son avis) et la connexion au **serveur MCP d'Elevate Seller**, avec une clé API de ton organisation.

- **Claude Code** : `/plugin marketplace add doveaia/elevate-seller-skills` puis `/plugin install elevate-seller@elevate-seller` ; la clé API est demandée à l'activation et le serveur MCP est connecté par le plugin.
- **claude.ai et Claude Desktop** : Personnaliser › Plugins › Ajouter une marketplace › `doveaia/elevate-seller-skills` ; le serveur MCP s'ajoute comme connecteur personnalisé avec un en-tête de requête, voir [`mcp/claude-desktop.md`](mcp/claude-desktop.md).
- **Autres agents** (OpenCode, Hermes, Codex, Gemini CLI, VS Code, Cursor, Windsurf…) : `git clone https://github.com/doveaia/elevate-seller-skills`, copie `skills/elevate-seller-product-analysis/` dans le dossier des skills de ton agent, puis le fichier de configuration MCP de ton agent depuis [`mcp/`](mcp/README.md) (à fusionner si le fichier existe déjà).
- **Clé API** : les fichiers de `mcp/` lisent la clé dans la variable d'environnement `ELEVATE_SELLER_API_KEY`. macOS / Linux : ajoute `export ELEVATE_SELLER_API_KEY="<ta clé API>"` à ton profil shell (`~/.zshrc`, `~/.bashrc`…). Windows : `setx ELEVATE_SELLER_API_KEY "<ta clé API>"`. Ouvre ensuite un nouveau terminal.

Crée la clé dans Elevate Seller, **Paramètres › Clés API** (« Lecture et écriture » pour que l'agent dépose ses avis).

Privacy: https://dash.elevateseller.com/privacy
