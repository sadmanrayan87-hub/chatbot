# Companion Chat (18+)

A small, zero-dependency web app for adult roleplay chat with a self-hosted open model.

## Run

1. Start a local model server with an OpenAI-compatible API, for example Ollama:
   ```bash
   ollama pull llama3.1
   ollama serve
   ```
2. Start the app:
   ```bash
   cd companion-chat
   LLM_MODEL=llama3.1 node server.js
   ```
3. Open http://127.0.0.1:3000

Environment variables: `PORT`, `LLM_URL` (default Ollama's `/v1/chat/completions`), `LLM_MODEL`, `COOKIE_SECRET`.

## Bengali

The server prompt tells the model to understand Bengali script, romanized Bengali and mixed Bengali-English, and to reply in the user's script. The UI uses a Bengali-capable font stack. How well it works depends on the model, and many small English-centric models are weak in Bengali. Try one of these and compare before choosing:

- `qwen2.5:14b` or larger
- `gemma2:9b` or `gemma3`
- `aya-expanse:8b` (Cohere's multilingual model, check its licence for your use)

```bash
ollama pull gemma2:9b
LLM_MODEL=gemma2:9b node server.js
```

## Safeguards

- **18+ age gate.** The server issues a signed cookie after the user confirms, and `/api/chat` refuses requests without it. This is a self-declaration only. If you deploy publicly, add real age verification, which is legally required in many places.
- **Hard limits in the system prompt.** The server always prepends rules covering fictional adults only, no minors, no non-consent and no real people. The user-editable persona cannot override them. These are model instructions, so they are not a guarantee. For a public deployment, add an input/output classifier as a second layer.
- **Rate limit** of 20 requests per minute per IP, and bounded message and history sizes.

## Notes

- The server binds to `127.0.0.1`. Put it behind HTTPS and authentication before exposing it.
- Chat history is stored only in the user's browser (`localStorage`). The server keeps no logs of messages.
- Use an open model whose licence permits your use case, and check the laws of where you host and who your users are.
- This directory is self-contained, so you can move it into its own repo.
