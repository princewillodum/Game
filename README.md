# Match & Make Words — Online multiplayer prototype

## Included
- Shareable room URLs and room codes.
- Supabase Realtime Broadcast/Presence for live room state.
- Interest, subcategory, favourite and turn-based word rounds.
- Netlify Function for OpenAI-generated hints, with built-in fallback hints.
- Responsive neon + educational UI.

## 1. Supabase setup
1. Create a project at https://supabase.com.
2. Copy the Project URL and publishable key (or legacy anon key) from Project Settings / API.
3. Do not use the `service_role` key in browser code.
4. This prototype uses Supabase Realtime Broadcast/Presence and no database table.
5. On the deployed app, click Create or Join; enter the Project URL and publishable/anon key when prompted. It stores these in localStorage in that browser.

## 2. Deploy to Netlify
1. Create a GitHub repository and upload all files, preserving `netlify.toml` and `netlify/functions/ai-clue.mjs`.
2. In https://app.netlify.com select Add new site / Import an existing project.
3. Connect GitHub and select the repository.
4. Leave the build command empty. Publish directory is `.`; the configuration also sets the Functions directory.
5. Deploy and open the generated `https://...netlify.app` URL.
6. Netlify Functions require a function-capable deploy. Use Git-based deployment or Netlify CLI; a static drag-and-drop upload alone may not deploy the function.

## 3. Enable OpenAI hints
1. Create an API key in your OpenAI API account.
2. In Netlify site settings, find Environment variables.
3. Add `OPENAI_API_KEY` with the secret value. Optionally add `OPENAI_MODEL` set to `gpt-5-mini`.
4. Ensure the variable is available to Functions, and trigger a new deploy.
5. In a game, the Word Arena's “Ask AI for a hint” button calls `/.netlify/functions/ai-clue`. Without the key, it uses built-in demo hints.
6. Never put the OpenAI API key in `index.html` or commit it to GitHub. API use may incur charges.

## 4. Test the room
1. Open the live site in browser/device A; enter a nickname and create a room.
2. Copy the invite URL and open it on device B.
3. Enter a different nickname, enter the same Supabase URL/key when prompted, and join.
4. Confirm both players appear in the lobby and the host can start.

## Important limitations
This is a demo/competition prototype, not a secure production game backend. It uses public Realtime topics and host-broadcast state; messages may be inspectable, actions/scores can be spoofed by malicious clients, and game state is not durably stored. A room can be lost if the host disconnects. Do not use for prizes or sensitive information. Before public production, implement authenticated private room membership, server-authoritative state and scoring, Postgres persistence/RLS, input validation, rate limits, reconnect support and room cleanup. Keep private interest choices off shared state until reveal.
