# Ask STEP chat bot

The "Ask STEP" chat box on the resource hub talks to this small Cloudflare Worker. The Worker reads the live hub page (events, deadlines, contacts, links, and Cohort Cup standings) and answers questions with a free Workers AI model (`@cf/google/gemma-4-26b-a4b-it`). Visitors don't need an account.

## Cost: $0

- Runs on Cloudflare's **Workers Free plan**, which includes 10,000 Workers AI "neurons" a day at no charge. That's roughly 250 questions a day.
- On the Free plan there is no payment method, so nothing can be charged. When the daily allowance runs out, the bot replies "try again tomorrow" until it resets at 8 pm Eastern (midnight UTC).
- **To keep it free:** never add a payment method and never upgrade to "Workers Paid." No step below asks for either.

## One-time setup

1. **Create a free Cloudflare account** at https://dash.cloudflare.com/sign-up. Don't add a card.
2. **Log in from Terminal** (opens a browser window to approve):

   ```bash
   cd ~/Documents/GitHub/stepresourcehub/chatbot
   npx wrangler login
   ```

3. **Deploy the bot:**

   ```bash
   npx wrangler deploy
   ```

   If it asks you to pick a `workers.dev` subdomain, choose any name (for example `step-hub`). When it finishes, it prints an address like `https://step-hub-bot.step-hub.workers.dev`.
4. **Connect the hub:** paste that address into `chatbotUrl` near the bottom of `index.html` (in `CONFIG`), commit, and push. The "Ask STEP" button appears once the site updates.

## Updating

- **Hub content:** nothing to do. The bot re-reads the live hub every 5 minutes.
- **Bot rules or wording** (`src/index.js`) or **settings** (`wrangler.jsonc`): edit, then run `npx wrangler deploy` again from this folder.

## Safeguards

- Only pages on `whartonstep.github.io` (and `localhost` for testing) can use the bot. Change `ALLOWED_ORIGINS` in `wrangler.jsonc` if the hub moves.
- At most 8 questions a minute per person.
- It only sees what's on the public hub page. Don't put private student information on the hub.
- Usage: Cloudflare dashboard → AI → Workers AI.
