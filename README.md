# Kling AI MCP Server

Two ways to use this — pick whichever fits you:

| Mode | File | Best for |
|------|------|----------|
| **Local** (Claude Desktop) | `index.js` | Running on your own computer |
| **URL** (Claude.ai / any client) | `server.js` | Deploying online so any device can use it |

---

## Option A — Local Setup (Claude Desktop)

### 1. Get Your Kling API Keys
1. Go to **https://klingai.com** and sign in
2. Go to **Developer** section or **https://klingai.com/dev**
3. Create an API key — save your **Access Key ID** and **Secret Key**

### 2. Install dependencies
```
npm install
```

### 3. Add to Claude Desktop config

Open the config file:
- **Mac:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "kling-ai": {
      "command": "node",
      "args": ["/full/path/to/kling-mcp/index.js"],
      "env": {
        "KLING_ACCESS_KEY": "your_access_key_here",
        "KLING_SECRET_KEY": "your_secret_key_here"
      }
    }
  }
}
```

### 4. Restart Claude Desktop — done.

---

## Option B — Deploy as a URL (Claude.ai Connector)

Deploy `server.js` to any cloud platform and connect via URL.

### Deploy to Railway (easiest — free tier available)

1. Go to **https://railway.app** and create a free account
2. Click **New Project → Deploy from GitHub repo**
   - Or use **Deploy from template** → select Node.js
3. Upload or push this folder to a GitHub repo
4. Set these environment variables in Railway dashboard:
   ```
   KLING_ACCESS_KEY = your_access_key_here
   KLING_SECRET_KEY = your_secret_key_here
   PORT = 3000
   ```
5. Railway gives you a public URL like: `https://kling-mcp-production.up.railway.app`

### Deploy to Render (also free)

1. Go to **https://render.com** → New Web Service
2. Connect your GitHub repo containing this folder
3. Set:
   - **Build Command:** `npm install`
   - **Start Command:** `node server.js`
4. Add the same environment variables
5. You'll get a URL like: `https://kling-mcp.onrender.com`

### Deploy to Heroku

```bash
heroku create your-kling-mcp
heroku config:set KLING_ACCESS_KEY=xxx KLING_SECRET_KEY=yyy
git push heroku main
```

### Run locally and expose with ngrok (for testing)

```bash
# Terminal 1 — start the server
KLING_ACCESS_KEY=xxx KLING_SECRET_KEY=yyy node server.js

# Terminal 2 — expose it publicly
npx ngrok http 3000
# Ngrok gives you: https://abc123.ngrok.io
```

---

## Connect URL to Claude.ai

Once deployed, add it as a connector in Claude.ai:

1. Go to **Claude.ai → Settings → Connectors** (or the MCP section)
2. Click **Add Connector**
3. Enter your URL: `https://your-domain.com/mcp`
4. Save — the Kling AI tools will appear automatically

---

## Test your deployment

Visit your URL in a browser:
```
https://your-domain.com/
```
You should see:
```json
{
  "name": "Kling AI MCP Server",
  "status": "running",
  "endpoint": "/mcp",
  "tools": ["image_to_video", "text_to_video", "check_task_status", "list_recent_videos"]
}
```

---

## How to Use (in Claude)

Once connected, just ask naturally:

- *"Generate a video from this product image: [URL] with cinematic dark lighting"*
- *"Make a 9:16 video of the nail cross chain with slow rotation"*
- *"Create a text-to-video of a luxury faith jewelry ad"*
- *"Check the status of Kling task [task_id]"*
- *"List my recent Kling videos"*

---

## Troubleshooting

**401 Unauthorized** → Keys are wrong or expired — get new ones from klingai.com/dev

**"Missing KLING_ACCESS_KEY"** → Environment variables aren't set on your host

**Connection refused** → Server isn't running, or wrong port

**Task times out** → Videos take 2–5 min to generate — use `check_task_status` with the task ID
