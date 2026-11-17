# ai-toolkit

Daily usage and limits for free AI models, in the terminal and the macOS menu bar.

## Restore on a new Mac

```bash
git clone https://github.com/Appentium/ai-toolkit ~/ai-toolkit
mkdir -p ~/.local/bin ~/.swiftbar
ln -s ~/ai-toolkit/bin/llm-usage ~/.local/bin/llm-usage
ln -s ~/ai-toolkit/swiftbar/llm-usage.15m.sh ~/.swiftbar/llm-usage.15m.sh
brew install --cask swiftbar
defaults write com.ameba.SwiftBar PluginDirectory -string "$HOME/.swiftbar"
open -a SwiftBar
```

## Keys

Keys stay out of this repo. Put these in `~/.zshrc`:

| Variable | Where to get it |
|---|---|
| `OPENROUTER_API_KEY` | openrouter.ai → Keys |
| `CEREBRAS_API_KEY` | cloud.cerebras.ai → API Keys |
| `GEMINI_API_KEY` | aistudio.google.com → Get API key |
| `GOOGLE_GENERATIVE_AI_API_KEY` | set to `$GEMINI_API_KEY` (opencode reads this name) |
| `ZHIPU_API_KEY` | z.ai → API Keys |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare dashboard sidebar |
| `CLOUDFLARE_API_KEY` | Cloudflare → API Tokens, Workers AI template + Account Analytics: Read |

## Needs

- opencode (Gemini, Z.ai and Cloudflare counts come from its local history)
- `~/.local/bin/agy-quota` and a running Antigravity.app for the Antigravity section
