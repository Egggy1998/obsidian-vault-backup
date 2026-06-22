# Secrets & API Keys — [MASKED]

> ⚠️ File này chỉ ghi MAPPING tên biến → tên service. KHÔNG có giá trị thật.
> Giá trị thật chỉ tồn tại trong `~/.hermes/secrets/` trên server.

---

## GitHub
- File: `~/.hermes/secrets/github.env`
- Key: `GITHUB_ACCESS_TOKEN`
- Type: Classic PAT (`ghp_***`), full admin scope
- Token đã bị GitHub Secret Scanning revoke sau khi lỡ push lên quang-obsidian-vault
- **Cần tạo PAT mới từ https://github.com/settings/tokens**

## Hugging Face
- File: `~/.hermes/secrets/huggingface.env`
- Key: `HF_TOKEN`
- Type: User access token
- Used by: `huggingface-cli`, `transformers`, `diffusers`

## Supabase
- File: `~/.hermes/secrets/supabase.env`
- Key: `SUPABASE_ACCESS_TOKEN`
- Type: Personal access token
- Used by: Supabase CLI, direct REST API

## MiniMax /abab
- File: `~/.hermes/secrets/minimax.env`
- Key: `MINIMAX_API_KEY`
- Type: API key
- Used by: MiniMax API calls

## OpenAI
- File: `~/.hermes/secrets/openai.env`
- Key: `OPENAI_API_KEY`
- Type: API key (`sk-***`)
- Used by: OpenAI API, Codex, GPT models

## Anthropic / Claude
- File: `~/.hermes/secrets/anthropic.env`
- Key: `ANTHROPIC_API_KEY`
- Type: API key
- Used by: Claude models

## Cloudflare
- File: `~/.hermes/secrets/cloudflare.env`
- Key: `CLOUDFLARE_ACCOUNT_API_TOKEN`
- Type: Account API token
- Used by: Cloudflare Workers, R2, Pages, Vectorize

## AtomicMail
- File: `~/.hermes/secrets/atomicmail.env`
- Key: `ATOMICMAIL_API_KEY`
- Type: API key
- Used by: Email sending via AtomicMail JMAP

## PlenXAI
- File: `~/.hermes/secrets/plenxai.env`
- Keys: `PLENXAI_ACCESS_TOKEN`, `PLENXAI_REFRESH_TOKEN`
- Type: OAuth tokens
- Used by: Image/video generation via PlenXAI web
- Token refresh: auto-handled by PlenXAI web MCP tools

## Tavily
- File: `~/.hermes/secrets/tavily.env`
- Key: `TAVILY_API_KEY`
- Type: API key
- Used by: Web research, Tavily search skill

## Supermemory
- File: `~/.hermes/secrets/.sm_key`
- Key: (raw API key)
- Used by: Supermemory backend — primary agent memory

## RunningHub
- File: `~/.hermes/secrets/runninghub.env`
- Key: `RUNNINGHUB_API_KEY`
- Type: API key
- Used by: AI video generation models

## Bitwarden CLI
- File: `~/.hermes/secrets/bitwarden.env`
- Keys: `BW_CLIENT_ID`, `BW_CLIENT_SECRET`
- Type: Secrets Manager client credentials
- Used by: `bw` CLI for vault access
- Auth: `bw login --apikey`

## Composio
- File: `~/.hermes/secrets/composio.env`
- Key: `COMPOSIO_API_KEY`
- Type: API key
- Used by: Composio tools/MCP for app integrations

---

## Actions needed

1. **GitHub PAT** — bị revoke → tạo PAT mới tại https://github.com/settings/tokens (classic, tick `repo` scope), gửi cho em
2. Sau khi có PAT mới → em update `~/.hermes/secrets/github.env` và push lại toàn bộ vault lên `quang-obsidian-vault`
