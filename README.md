# Book Check

A small web app for screening books for a young reader. Enter a title and author. Claude searches the web live (Common Sense Media, the publisher, Amazon, Barnes & Noble, Goodreads, and review journals), reads the pages, and returns:

- a verdict for the reader's age (Good to go / OK with a talk / Wait a year or two / Not recommended)
- the age range each source lists, linked to the page it came from
- 1–5 ratings for sex & romance, language, violence, mature themes, and drinking/drugs
- a spoiler-light plot summary, strengths, and questions to talk through
- every page Claude looked at

## What you need

1. An Anthropic API key from https://console.anthropic.com (Settings → API Keys). Add a payment method and, ideally, a monthly spend limit.
2. Somewhere to run it. The steps below use Render, which hosts small Node apps for a few dollars a month.

## Run it on your own computer

Install Node.js 20 or newer, then in this folder:

```bash
npm install
ANTHROPIC_API_KEY=sk-ant-... npm start
```

Open http://localhost:3000.

## Put it online (Render)

1. Put this folder in a GitHub repository.
2. In Render, choose **New → Blueprint** and pick the repository. Render reads `render.yaml`.
3. When asked, set:
   - `ANTHROPIC_API_KEY`: your API key
   - `APP_PASSWORD`: a password for the site. Your browser will ask for it once; type anything as the username.
4. Deploy. Render gives you a URL like `https://book-check.onrender.com`.

Always set `APP_PASSWORD` on a public deployment. Without it, anyone who finds the URL can run checks on your API key.

## Settings

| Variable | Default | What it does |
|---|---|---|
| `ANTHROPIC_API_KEY` | required | Your Anthropic API key |
| `APP_PASSWORD` | none | Password for the whole site |
| `PORT` | 3000 | Port to listen on (hosts usually set this) |
| `CLAUDE_MODEL` | `claude-opus-5-5` | Model that does the research |
| `CLAUDE_EFFORT` | `medium` | `low`, `medium`, or `high`. Higher is more thorough, slower, and costs more |

## Cost and speed

Each check runs up to 8 web searches and reads up to 6 pages, and takes about one to three minutes. Expect very roughly $0.25–$0.75 per book with the defaults; your Anthropic console shows the exact amount. Checking the same book again within 24 hours reuses the saved result at no cost (until the server restarts). The server runs at most two checks at a time.

## Files

- `server.js`: web server and the Claude research call
- `public/index.html`: the page
- `render.yaml`: Render deployment settings
