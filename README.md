# Bedrock Bot Dashboard

A small Node.js Minecraft Bedrock client with a password-protected web dashboard, Microsoft device-code authentication, status/log monitoring, and reconnect support.

## Important

Use this only on a server you own or have permission to automate. The bot stays idle; it does not attempt to defeat server-side AFK rules or anti-cheat systems.

## Deploy to Railway

1. Create a **private GitHub repository** and upload the contents of this folder (not the ZIP file itself).
2. In Railway, create a new project and deploy from that GitHub repository.
3. Open the service's **Variables** tab and add:
   - `MC_HOST` — your Bedrock server address
   - `MC_PORT` — Bedrock UDP port (usually `19132`)
   - `MC_USERNAME` — your Minecraft gamertag/profile name
   - `DASHBOARD_PASSWORD` — a long, unique password for the dashboard
   - `DATA_DIR` — `/data`
   - `AUTO_RECONNECT` — `true`
4. Add a Railway **Volume** to the service and set its mount path to `/data`. This keeps Microsoft authentication cache files across deployments.
5. In Railway service settings, generate a public domain. Open it; the browser will ask for the dashboard username/password. The username can be anything; use the value of `DASHBOARD_PASSWORD` as the password.
6. Press **Start bot**. If Microsoft device login is requested, the dashboard displays the code and sign-in link. Complete the sign-in using the Microsoft account that owns Minecraft Bedrock.
7. Keep the GitHub repository private. Never commit `.env`, authentication cache files, or account credentials.

## Run locally

1. Install Node.js 20 or newer.
2. Copy `.env.example` to `.env` and fill in the server address, gamertag, and a strong dashboard password.
3. In this folder, run:

   ```bash
   npm install
   npm start
   ```

4. Open `http://localhost:3000` and enter the dashboard password when prompted.

## Configuration

| Variable | Default | Purpose |
|---|---|---|
| `MC_HOST` | `localhost` | Bedrock server hostname |
| `MC_PORT` | `19132` | Bedrock UDP port |
| `MC_USERNAME` | `BedrockBot` | Profile name used by the auth library |
| `DASHBOARD_PASSWORD` | none | Required dashboard password |
| `DATA_DIR` | `./data` | Stores cached Microsoft authentication |
| `AUTO_RECONNECT` | `true` | Reconnect after a disconnect |
| `RECONNECT_MIN_MS` | `5000` | First retry delay |
| `RECONNECT_MAX_MS` | `60000` | Maximum retry delay |
| `BOT_AUTOSTART` | `false` | Start automatically when service boots |
| `PORT` | `3000` | Dashboard HTTP port |

## Notes

- Bedrock servers use UDP for Minecraft traffic. The hosting provider must permit the required outbound UDP traffic. Confirm this with your hosting provider before relying on a cloud deployment.
- A Railway Volume mounted at `/data` is recommended for Microsoft sign-in cache persistence. Without persistent storage, the account may need to authenticate again after a redeploy.
- Railway services can restart or sleep depending on plan and usage. A restart policy does not guarantee uninterrupted uptime.
- The dashboard uses HTTP Basic Authentication. Use a strong password and do not share the dashboard URL and password together.
- The health endpoint at `/health` is intentionally public for Railway's health check; all dashboard and bot-control routes require authentication.
