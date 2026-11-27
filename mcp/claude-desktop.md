# Claude Desktop, claude.ai and Claude mobile

Claude Desktop reads only local (command-line) MCP servers from `claude_desktop_config.json`. A remote server such as Elevate Seller is added as a **custom connector**, which is set up in your Claude account. The same connector then works in claude.ai on the web, in Claude Desktop and in the Claude mobile apps. There is no file to copy.

1. Open **Settings › Connectors**, then **Add custom connector**.
2. URL: `https://api.elevateseller.com/mcp`
3. Authentication: **No sign-in**.
4. Under **Request headers**, add one header:
   - name: `Authorization`
   - value: `Bearer ` followed by your API key (Claude sends the value exactly as you type it and adds no prefix).
5. Save. Claude stores the header value securely.

Notes:

- Claude connects to the server from Anthropic's cloud, not from your computer, so only the public URL works.
- Request headers are a beta feature, open to a limited set of organisations. Only an organisation Owner adds a connector with request headers, and every member of the Claude organisation then uses the same key. If you do not see **Request headers**, the connector cannot be used with an API key yet.
- A **Read and write** key is needed for the agent to record opinions.

Sources (read 2026-11-27): <https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp>, <https://claude.com/docs/connectors/custom/remote-mcp>, <https://claude.com/docs/connectors/building/authentication>.

---

# Claude Desktop, claude.ai et Claude mobile

Claude Desktop ne lit dans `claude_desktop_config.json` que des serveurs MCP locaux (lancés par une commande). Un serveur distant comme Elevate Seller s'ajoute comme **connecteur personnalisé**, configuré dans ton compte Claude. Ce connecteur sert ensuite sur claude.ai, dans Claude Desktop et dans les applications mobiles. Il n'y a aucun fichier à copier.

1. Ouvre **Paramètres › Connecteurs**, puis **Ajouter un connecteur personnalisé**.
2. URL : `https://api.elevateseller.com/mcp`
3. Authentification : **Aucune connexion**.
4. Sous **En-têtes de requête**, ajoute un en-tête :
   - nom : `Authorization`
   - valeur : `Bearer ` suivi de ta clé API (Claude envoie la valeur telle que tu la saisis, sans ajouter de préfixe).
5. Enregistre. Claude conserve la valeur de l'en-tête de façon sécurisée.

À savoir :

- Les libellés de l'interface peuvent varier selon la langue choisie dans Claude.
- Claude se connecte au serveur depuis le cloud d'Anthropic, pas depuis ton ordinateur : seule l'URL publique fonctionne.
- Les en-têtes de requête sont une fonction bêta, ouverte à un nombre limité d'organisations. Seul un Owner de l'organisation Claude ajoute un connecteur avec en-têtes de requête, et tous ses membres utilisent alors la même clé. Si **En-têtes de requête** n'apparaît pas, le connecteur ne peut pas encore utiliser de clé API.
- Une clé **Lecture et écriture** est nécessaire pour que l'agent dépose ses avis.
