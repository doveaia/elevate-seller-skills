# Elevate Seller — skills for AI agents

This repository is a **Claude Code plugin marketplace** and the **`elevate-seller` plugin** itself. It ships:

- the **product analysis skill** (`skills/elevate-seller-product-analysis/`): the method an AI agent follows to analyse an Amazon product with the seller's Elevate Seller grid (twelve criteria), decide, and record its opinion in the application;
- the connection to the **Elevate Seller MCP server** (`https://api.elevateseller.com/mcp`), authenticated by an API key of your organisation.

The skill contains no data of any organisation and no threshold value: the agent reads the seller's grid with the `get_analysis_grid` tool.

## Install

You need an API key, created in Elevate Seller under **Settings › API keys**. Choose **Read and write** if the agent should record its opinions.

### Claude Code

```
/plugin marketplace add doveaia/elevate-seller-skills
/plugin install elevate-seller@elevate-seller
```

Claude Code asks for the API key when the plugin is enabled and stores it as a sensitive value. The skill is then available as `/elevate-seller:elevate-seller-product-analysis`, and the `elevate-seller` MCP server is connected.

### claude.ai and Claude Desktop

**Customize › Plugins › Add marketplace**, then enter `doveaia/elevate-seller-skills`. The skill is installed from the marketplace; connect the Elevate Seller MCP server as a connector, as described in Elevate Seller under **Settings › AI agent**.

### Other agents (OpenCode, Hermes, …)

```
git clone https://github.com/doveaia/elevate-seller-skills
```

Copy the folder `skills/elevate-seller-product-analysis/` into the folder where your agent reads its skills (OpenCode: `~/.config/opencode/skills/`, Hermes: `~/.hermes/skills/`), then add the MCP server with your API key as shown in Elevate Seller under **Settings › AI agent**.

## Updates

The plugin version is in `.claude-plugin/plugin.json`. In Claude Code, run `/plugin marketplace update elevate-seller` to get a new version.

---

# Elevate Seller — skills pour agents IA

Ce dépôt est une **marketplace de plugins Claude Code** et le **plugin `elevate-seller`** lui-même : le **skill d'analyse de produit** (la méthode que suit un agent pour analyser un produit Amazon avec la grille Elevate Seller du vendeur, décider et déposer son avis) et la connexion au **serveur MCP d'Elevate Seller**, avec une clé API de ton organisation.

- **Claude Code** : `/plugin marketplace add doveaia/elevate-seller-skills` puis `/plugin install elevate-seller@elevate-seller` ; la clé API est demandée à l'activation.
- **claude.ai et Claude Desktop** : Personnaliser › Plugins › Ajouter une marketplace › `doveaia/elevate-seller-skills`.
- **Autres agents** : `git clone https://github.com/doveaia/elevate-seller-skills`, puis copie `skills/elevate-seller-product-analysis/` dans le dossier des skills de ton agent.

Crée la clé dans Elevate Seller, **Paramètres › Clés API** (« Lecture et écriture » pour que l'agent dépose ses avis).

Privacy: https://dash.elevateseller.com/privacy
