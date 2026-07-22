# hermes-edenai-provider

Eden AI model-provider plugin for [Hermes Agent](https://github.com/NousResearch/hermes-agent).

Eden AI exposes an OpenAI-compatible `/v3` endpoint that fronts multiple upstream providers (OpenAI, Anthropic, Google, Meta, Mistral, etc.) behind a single API key. Model IDs follow the `<upstream>/<model>` form, e.g. `openai/gpt-4o`, `anthropic/claude-3-5-sonnet`.

## Prerequisites

- A Hermes Agent installation (v2.x+)
- An Eden AI API key — get one at [app.edenai.run](https://app.edenai.run/)

## Installation

### Option A — Drop-in (no pip)

```bash
mkdir -p ~/.hermes/plugins/model-providers/edenai
cd ~/.hermes/plugins/model-providers/edenai
curl -sL https://raw.githubusercontent.com/ncoquelet/hermes-edenai-provider/main/__init__.py -o __init__.py
curl -sL https://raw.githubusercontent.com/ncoquelet/hermes-edenai-provider/main/plugin.yaml -o plugin.yaml
```

### Option B — Clone

```bash
git clone https://github.com/ncoquelet/hermes-edenai-provider.git ~/.hermes/plugins/model-providers/edenai
```

## Configuration

Add your Eden AI API key to `~/.hermes/.env`:

```
EDENAI_API_KEY=your_key_here
```

To use Eden AI as your provider, set it in `config.yaml` under the `model` section:

```yaml
model:
  provider: edenai
  base_url: https://api.edenai.run/v3
```

Or switch at runtime with:

```
/model --provider edenai
```

### Model picker grouping

Eden AI returns hundreds of models. To group them by upstream sub-provider in the picker, add to `config.yaml`:

```yaml
model_picker:
  group_by_subprovider:
    - edenai
```

This gives you a two-step picker: pick the upstream (openai, anthropic, google, ...), then the model.

## Fallback models

When the live `/v3/models` endpoint is unreachable, the picker falls back to:

- `openai/gpt-4o`
- `openai/gpt-4o-mini`
- `anthropic/claude-3-5-sonnet`
- `google/gemini-1.5-pro`
- `meta/llama-3.1-70b-instruct`

## License

MIT
