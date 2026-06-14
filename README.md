# fan-out

One prompt, many models, side by side — then keep the good parts straight into your notes.

A single self-contained HTML file. No build step, no server, no dependencies. Run the same prompt across Claude, GPT, Gemini, Grok, DeepSeek, Qwen, Llama, and local Ollama models at once, compare the answers in parallel columns, and save the ones you like to a markdown notes folder with tags and attribution.

![fan-out — one prompt fanned out across Claude, GPT, and Gemini, with per-column keep and re-run controls](screenshot.svg)

> The image above is a representative mockup of the interface. Replace it with a real screenshot once you've run the tool, or delete the line if you'd rather not include one.

## Quick start

**Hosted (GitHub Pages):** open `https://<username>.github.io/<repo>/`. HTTPS is required for the "connect notes folder" feature to work.

**Local:** just open the file — double-click it (`file://`). In Chromium-based browsers (Chrome, Edge, Brave) this is a secure context, so every feature works, including "connect notes folder." No server required.

If you specifically want an `http://` origin instead, you can optionally serve it, e.g.

```
python -m http.server 8000
```

then open `http://localhost:8000` — but this isn't necessary for the notes feature.

The notes-folder feature depends on the **File System Access API**, which exists only in Chromium-based browsers. Firefox and Safari don't support it in any context (`file://`, localhost, or HTTPS); there, saving falls back to clipboard and file download.

## Setup

1. Open **⚙ keys & models**.
2. Paste an **OpenRouter** key (`sk-or-...`) — this is the simplest path: one key and one balance reach Claude, GPT, Gemini, Grok and the rest, and it works directly from the browser with no CORS proxy. Get one at [openrouter.ai](https://openrouter.ai) → Keys.
3. (Optional) Paste direct provider keys (OpenAI, Anthropic, Google, DeepSeek, Qwen, Grok, Ollama) if you'd rather call a provider directly for a given column. Change a model row's *vendor* to use its direct key instead of OpenRouter.
4. Write a prompt, toggle the models you want, hit **Run** (or ⌘/Ctrl + Enter).

Keys are stored only in your browser's `localStorage` — never in the file, never committed. See *Security* below.

## Usage

A typical session:

1. **Write one prompt** in the box at the top.
2. **Choose models** by toggling the pills beneath it. Each enabled model gets a column.
3. **Run** (button or ⌘/Ctrl + Enter). Columns fill in parallel — live, token by token, if streaming is on; otherwise each appears when its model finishes. Every column shows its response time.
4. **Compare** the answers side by side.
5. **Keep the good parts:**
   - Select any text in a column → a popover lets you keep just that snippet or copy it.
   - Click ⤓ in a column header to keep that whole response.
   - Click **⤓ keep all columns** (top bar) to save every finished column at once, led by the prompt.
   - Each save opens a dialog to add tags and choose where it goes: append to an existing `.md`, create a new one, or copy to the clipboard. Model name, timestamp, and tags are recorded automatically.
6. **Re-run one column** with ↻ if a single answer was weak — the others keep their output and aren't re-billed.

Controls disable themselves while a column is loading: you can't save a half-finished response, re-run a column that's still streaming, or start a new run over columns that are mid-flight. A column becomes saveable the moment it finishes; **keep all** unlocks once every column is done.

## Features

- **Fan-out:** one prompt runs against every enabled model; responses appear in parallel columns with per-model timing.
- **Max reasoning:** requests are sent at maximum reasoning/thinking effort where the provider supports it (OpenRouter's unified `reasoning` field, OpenAI/DeepSeek `reasoning_effort`, Claude extended thinking, Gemini `thinkingLevel: HIGH`).
- **Streaming (optional):** a top-bar toggle (off by default, remembered) fills columns live, token by token, as each model writes.
- **Keep selections:** highlight any text in a column to save just that snippet.
- **Keep a whole column:** the ⤓ button in a column header saves that column's full response.
- **Keep all columns:** the top-bar button saves every finished column at once, led by the prompt.
- **Notes destinations:** append to an existing `.md`, create a new `.md`, or copy to clipboard. Saves carry tags, model attribution, and timestamps.
- **Prompt capture:** keep-all always saves the prompt at the top; single-column saves include it too, but skip it when appending to a file that already contains that prompt.
- **Per-column re-run:** the ↻ button regenerates just one column without re-firing (or re-billing) the rest.
- **Busy lockout:** save and run controls disable while a column is loading, so no partial saves and no overlapping runs.
- **Extensible:** any OpenAI-compatible endpoint (OpenRouter, vLLM, LM Studio, LiteLLM, …) works as a model row — set its vendor to `openai-compatible` and give it a base URL.

## Notes folder

Click **connect notes folder** (Chromium-based browsers: Chrome, Edge, Brave) and grant a folder once per session. Saved picks then write directly into that folder as markdown. This works whether you open the file via `file://` or over a local/remote server. In browsers without the File System Access API (Firefox, Safari), use the clipboard or new-file download paths instead.

## Model IDs

OpenRouter rows use `provider/model` slugs (e.g. `anthropic/claude-opus-4-8`, `google/gemini-3.5-flash`). The defaults are best-guess current slugs — confirm exact strings at [openrouter.ai/models](https://openrouter.ai/models). If a column errors with "model not found," fix that row's model field. Use **↺ reset to latest defaults** in settings to restore the shipped set.

## Security

API keys live only in `localStorage`, scoped to the page's origin. The HTML file itself contains no secrets, so it's safe to publish publicly. Because `localStorage` is per-browser, anyone opening your hosted copy is prompted for their own keys and cannot see or use yours. Never hardcode a key into the file.

## Billing

Each provider bills separately and is not covered by any consumer chat subscription (e.g. ChatGPT Plus or Claude Pro do **not** fund their APIs). Add credits/payment in each provider's console, and set spending caps where offered. Maximum-effort reasoning costs more and runs slower than default.

## License

BSD 3-Clause. See [LICENSE](LICENSE).
