# quota-bar

**See every free AI model quota you have, in one place, right in your Mac menu bar.**

Free tiers are generous when you combine them (OpenRouter, Cerebras, Gemini, Z.ai, OpenCode Zen, Cloudflare Workers AI), but every provider counts differently and hides its limits somewhere else. `llm-usage` pulls them together so you know which model still has room before your coding agent hits a 429.

![Menu bar](docs/menu-bar.png)

## Features

- 🟢🟡🔴 One dot in the menu bar for your overall usage; one colored row per model inside
- Live numbers straight from providers that expose them (OpenRouter, Cerebras, Cloudflare)
- Local counts from [opencode](https://opencode.ai) history for providers that don't (Gemini, Z.ai, OpenCode Zen)
- Reset times shown in your own time zone
- Click any provider to open its dashboard
- Same view in the terminal: `llm-usage`
- Everything the terminal does is in the menu too: **Best models now** (click to copy), and **Tools** with Copy best models list, Copy all data as JSON and Edit known limits
- Single Python file, standard library only, works with the Python that ships with macOS
- Finds providers by itself: any of the 300+ providers in the [models.dev](https://models.dev) catalog shows up once you use it through opencode (last 7 days) or log in to it there, and disappears when you stop. Providers disabled in opencode stay hidden
- Flags models the catalog marks deprecated or no longer offers
- Shows Ollama and LM Studio models when they are running on your Mac
- Estimates when a limit runs out at your current pace ("Runs out around 15:20")
- Mac notification when a limit is nearly or fully used up, and when it is usable again
- `llm-usage --pick` lists your recently used opencode models that still have room, most room first, as `provider/model`; `llm-usage --json` prints everything for scripts
- Hide any provider you don't use from the menu (**Hide**), bring it back with **Show hidden**, or run `llm-usage --hide zai` / `llm-usage --show zai`. Hidden providers make no API calls.

## Supported providers

| Provider | Source | What you see |
|---|---|---|
| OpenRouter | live API | free-model requests used of daily limit, credit spent |
| Cerebras | live rate-limit headers | requests and tokens used today, or blocked |
| Google Gemini | opencode history + `providers.json` | requests per model vs the free-tier daily limit |
| Any other provider (Groq, Z.ai, OpenCode Zen, Mistral, NVIDIA…) | opencode history + live rate-limit headers when the API sends them | requests and tokens per model, live limit, blocked |
| Ollama, LM Studio | local server | installed models |
| Cloudflare Workers AI | live GraphQL + probe | neurons used in the last 24 h of 10,000, per model, blocked status and when it frees up |
| Antigravity | local Antigravity.app hub (must be running) | % left in every 5-hour and weekly pool, reset times |

## Install

```bash
git clone https://github.com/Appentium/quota-bar ~/quota-bar
mkdir -p ~/.local/bin ~/.swiftbar
ln -s ~/quota-bar/bin/llm-usage ~/.local/bin/llm-usage
ln -s ~/quota-bar/swiftbar/llm-usage.15m.sh ~/.swiftbar/llm-usage.15m.sh
brew install --cask swiftbar
defaults write com.ameba.SwiftBar PluginDirectory -string "$HOME/.swiftbar"
open -a SwiftBar
```

The menu refreshes every 15 minutes. Rename the plugin to `llm-usage.30m.sh` to change that.

## Known limits

`providers.json` holds what the catalog doesn't: free-tier daily limits, the reset time zone, dashboard links and API addresses for providers whose address isn't in the catalog. Add a provider there and its section picks it up.

## Keys

Add whichever you have to `~/.zshrc`. Each key is only sent to its own provider.

| Variable | Where to get it |
|---|---|
| `OPENROUTER_API_KEY` | openrouter.ai → Keys |
| `CEREBRAS_API_KEY` | cloud.cerebras.ai → API Keys |
| `GEMINI_API_KEY` | aistudio.google.com → Get API key |
| `GOOGLE_GENERATIVE_AI_API_KEY` | set to `$GEMINI_API_KEY` (the name opencode reads) |
| `GROQ_API_KEY` | console.groq.com → API Keys |
| `ZHIPU_API_KEY` | z.ai → API Keys |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare dashboard sidebar |
| `CLOUDFLARE_API_KEY` | Cloudflare → API Tokens, Workers AI template plus **Account Analytics: Read** |

## Notes

- The Cerebras check sends one 1-token request per refresh (about 96 a day at the default interval). Providers with live limits (Groq and others) send one per model you used today, and Cloudflare one only when it has usage in the last 24 h.
- Cloudflare enforces its 10,000 free neurons over the last 24 hours, not per UTC day, so the row counts that window and shows when enough usage ages out.
- Gemini, Z.ai and OpenCode Zen have no usage API, so only requests made through opencode are counted. Your AI Studio dashboard stays the source of truth.
- Status colors: green under 70% used, yellow 70–90%, red 90% and above.

## Contributing

Issues and pull requests for more providers (Mistral, GitHub Models, NVIDIA NIM…) are welcome. Each provider is one small function that returns a `Section`.

## License

[MIT](LICENSE)
