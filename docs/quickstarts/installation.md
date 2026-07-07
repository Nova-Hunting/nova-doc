---
hide:
  - quickstart
icon: material/cog-outline
title: Installation
---

# Installing Nova

Nova is a prompt pattern matching framework designed to detect potentially harmful or problematic prompts for Large Language Models (LLMs). This guide explains how to install and configure Nova.

## Installation

Nova is available as a Python package via pip. It requires Python 3.10 or newer.

### Quick Installation

```bash
pip install nova-hunting
```

This installs the core engine — keyword matching, regex matching, LLM evaluation, and the `novarun` command-line tool, which is automatically added to your path.

### Semantic Matching (Optional)

Semantic similarity (`semantics:` rules) uses a heavier ML dependency stack (`sentence-transformers`). Install it explicitly when you need it:

```bash
pip install "nova-hunting[semantic]"
```

### Installation from Source
For development or to get the latest version, install from GitHub in editable mode:

```bash
git clone https://github.com/Nova-Hunting/nova-framework
cd nova-framework
pip install -e ".[dev]"
```

# Configuration
Nova requires API keys for the LLM providers you want to use. You can set these keys as environment variables:

## Setting API Keys
### OpenAI (Default)

```bash
# For OpenAI models (GPT-4o, etc.)
export OPENAI_API_KEY="your_openai_api_key_here"
```
### Anthropic

```bash
# For Anthropic models (Claude, etc.)
export ANTHROPIC_API_KEY="your_anthropic_api_key_here"
```

### Azure OpenAI
```bash
# For Azure OpenAI Service
export AZURE_OPENAI_API_KEY="your_azure_api_key_here"
export AZURE_OPENAI_ENDPOINT="your_azure_endpoint_here"
```

### Groq
```bash
# For Groq models (Llama-3, etc.)
export GROQ_API_KEY="your_groq_api_key_here"
```

### OpenRouter
```bash
# For OpenRouter's OpenAI-compatible API (access to many models)
export OPENROUTER_API_KEY="your_openrouter_api_key_here"

# Optional app attribution headers sent with OpenRouter requests
export OPENROUTER_HTTP_REFERER="https://your-app.example"
export OPENROUTER_APP_TITLE="Your App Name"
```

### Ollama (Local Models)
For Ollama, no API key is needed as it runs locally, but you need to have Ollama installed and running:
```bash
# Optional: If Ollama is running on a different host or port
export OLLAMA_HOST="http://localhost:11434"
```

## Model Overrides

If you don't pass a model explicitly, Nova checks provider-specific environment variables such as `OPENAI_LLM_MODEL`, `ANTHROPIC_LLM_MODEL`, `AZURE_OPENAI_LLM_MODEL`, `GROQ_LLM_MODEL`, `OLLAMA_LLM_MODEL`, and `OPENROUTER_LLM_MODEL`. Shorter aliases (`OPENAI_MODEL`, `OPENROUTER_MODEL`, `AZURE_OPENAI_DEPLOYMENT`, ...) are also accepted, with `NOVA_LLM_MODEL` as a shared fallback before the provider default.

## Configuration File
Nova also supports loading provider defaults and credentials from an INI file. Create a file named `nova.ini`:

```ini
[llm]
provider = openrouter
model = openai/gpt-5.2

[api_keys]
openrouter = sk-or-...
```

To use this configuration file with the Nova runner:
```bash
novarun -r rules.nov -p "prompt" -c nova.ini
```

Explicit `--llm` and `--model` flags override config file values, and environment variables override file credentials and model settings. When `-c/--config` is passed explicitly, Nova fails fast if the file is missing or malformed. Without `-c`, Nova also looks for `~/.nova/config.ini` and `./nova.ini`.

# Troubleshooting
## Common Installation Issues

1. Missing command-line tool: If the `novarun` command is not found, ensure that your Python binary directory is in your PATH. You can also run `python -m nova.novarun` as an alternative.
2. Dependency conflicts: If you encounter dependency conflicts, consider using a virtual environment:
```bash
python -m venv nova-env
source nova-env/bin/activate  # On Windows: nova-env\\Scripts\\activate
pip install nova-hunting
```
3. Semantic rules report degraded coverage: install the optional extra with `pip install "nova-hunting[semantic]"`. The first semantic scan downloads the embedding model and needs network access.

## LLM Connection Issues

1. API key errors: Ensure your API keys are correctly set in your environment variables or configuration file. Nova intentionally does not fall back to another provider when the requested one is missing credentials.
2. Ollama connection errors: If using Ollama, make sure the Ollama service is running and accessible.
3. Network issues: Check your internet connection and firewall settings if you're having trouble connecting to external LLM providers.

# Uninstallation
To remove Nova:
```bash
pip uninstall nova-hunting
```

# Next Steps
Once you have Nova installed, you can proceed to:

- Creating Rules: Learn how to write detection rules
- Running Nova: Start checking prompts against your rules
