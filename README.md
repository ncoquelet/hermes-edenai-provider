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

### Activate the provider

Add your Eden AI API key to `~/.hermes/.env`:

```
EDENAI_API_KEY=your_key_here
```

Declare the provider in `config.yaml`:

```yaml
providers:
  edenai:
    base_url: https://api.edenai.run/v3
```

The `base_url` is optional — it defaults to `https://api.edenai.run/v3`. Override it only if you need a different endpoint.

### Set as default provider + model

```yaml
model:
  provider: edenai
  default: openai/gpt-4o
```

Or switch at runtime with:

```
/model --provider edenai
```

## Fallback models

When the live `/v3/models` endpoint is unreachable, the picker falls back to a static list defined in the plugin. To use different fallbacks, edit `__init__.py` and replace the `fallback_models` tuple:

```python
fallback_models=(
    "qwen/glm-5.2",
    "deepseek/deepseek-v4-pro",
    "openai/gpt-4o-mini",
    "meta/llama-3.1-70b-instruct",
),
```

Pick models that support tool calling — cheaper models like `qwen/glm-5.2` or `deepseek/deepseek-v4-pro` are good defaults that keep costs down.

## License

MIT
