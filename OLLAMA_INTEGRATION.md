# Ollama Cloud Integration for 📟 PocketAgent

## What is Ollama Cloud?

Ollama Cloud provides access to powerful open-source models (Kimi K2.5, MiniMax M2.5, GLM-5, etc.) through a simple API - no GPU infrastructure required. Models run on Ollama's cloud infrastructure while you use the same familiar Ollama interface.

**Key Benefits:**
- ✅ No GPU required on your server
- ✅ Access to frontier open models
- ✅ Pay-per-use (no idle costs)
- ✅ Simple API key authentication
- ✅ OpenAI-compatible API
- ✅ Works with OpenClaw out of the box

## Available Cloud Models

**All Available Models (32 total):**

| Model | Size | Best For | Notes |
|-------|------|----------|-------|
| `kimi-k2.5` | 1.1TB | General assistant, multimodal | ⭐ Vision + language, agentic capabilities |
| `minimax-m2.5` | 230GB | Coding & productivity | ⭐ State-of-the-art for real-world tasks |
| `minimax-m2.1` | 230GB | Coding & productivity | Latest MiniMax version |
| `minimax-m2` | 230GB | Agent workflows | High-efficiency coding |
| `glm-5` | 756GB | Complex reasoning | ⭐ 744B total params (40B active) |
| `glm-4.7` | 696GB | Advanced reasoning | Latest GLM-4 version |
| `glm-4.6` | 696GB | Advanced reasoning | GLM-4 series |
| `deepseek-v3.2` | 689GB | Advanced reasoning | ⭐ Latest DeepSeek |
| `deepseek-v3.1:671b` | 689GB | Advanced reasoning | DeepSeek v3.1 |
| `qwen3-coder-next` | 82GB | Multilingual coding | ⭐ Optimized for development |
| `qwen3-coder:480b` | 510GB | Large-scale coding | Massive coding model |
| `qwen3.5:397b` | 397GB | Multimodal with tools | Vision + tools |
| `qwen3-next:80b` | 82GB | General purpose | Qwen3 next generation |
| `qwen3-vl:235b` | 470GB | Vision + language | Multimodal capabilities |
| `qwen3-vl:235b-instruct` | 470GB | Vision + language (instruct) | Instruction-tuned vision model |
| `kimi-k2:1t` | 1.1TB | General assistant | Kimi K2 1T version |
| `kimi-k2-thinking` | 1.1TB | Reasoning & thinking | Specialized reasoning model |
| `mistral-large-3:675b` | 682GB | Large tasks | Mistral's largest model |
| `cogito-2.1:671b` | 689GB | Complex reasoning | Advanced reasoning model |
| `devstral-2:123b` | 128GB | Development | Mistral for developers |
| `devstral-small-2:24b` | 52GB | Development (efficient) | Smaller dev model |
| `gpt-oss:120b` | 65GB | Large tasks | 120B parameters |
| `gpt-oss:20b` | 14GB | General purpose | 20B parameters |
| `ministral-3:14b` | 16GB | General purpose | Mistral small model |
| `ministral-3:8b` | 10GB | Efficient tasks | Fast inference |
| `ministral-3:3b` | 4.7GB | Lightweight tasks | Very efficient |
| `nemotron-3-nano:30b` | 33GB | Efficient reasoning | NVIDIA Nemotron |
| `rnj-1:8b` | 16GB | General purpose | RNJ model |
| `gemma3:27b` | 55GB | General purpose | Google Gemma large |
| `gemma3:12b` | 24GB | General purpose | Google Gemma medium |
| `gemma3:4b` | 8.6GB | Lightweight tasks | Google Gemma small |
| `gemini-3-flash-preview` | N/A | Fast inference | Google Gemini preview |

**⭐ Recommended for PocketAgent:**
- **General Assistant:** `kimi-k2.5` (multimodal, agentic)
- **Coding:** `minimax-m2.5` or `qwen3-coder-next`
- **Reasoning:** `glm-5` or `deepseek-v3.2`
- **Efficient:** `ministral-3:8b` or `gemma3:12b`

Full list: https://ollama.com/search?c=cloud

## Setup Guide

### Step 1: Get Ollama API Key

1. Sign up at https://ollama.com
2. Go to https://ollama.com/settings/keys
3. Create a new API key
4. Save it securely

**Test Your API Key:**

```bash
# Quick test
curl https://ollama.com/api/tags \
  -H "Authorization: Bearer YOUR_API_KEY"

# Or use the test script
./test_ollama_cloud.sh
```

### Step 2: Configure in PocketAgent

Add to your `.env` file:

```bash
OLLAMA_API_KEY="your_api_key_here"
```

**Correct Base URLs:**
- OpenAI-compatible: `https://ollama.com/v1` (Recommended)
- Native Ollama: `https://ollama.com`

### Step 3: Configure OpenClaw

OpenClaw will auto-configure Ollama Cloud during onboarding, or you can manually configure:

**Configuration Options:**

Ollama Cloud supports BOTH API formats:

**Option 1: OpenAI-Compatible API (Recommended)**
```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "openai/minimax-m2.5",
        "fallbacks": ["openai/kimi-k2.5"]
      }
    }
  },
  "models": {
    "providers": {
      "openai": {
        "baseUrl": "https://ollama.com/v1",
        "apiKey": "${OLLAMA_API_KEY}",
        "api": "openai"
      }
    }
  }
}
```

**Option 2: Native Ollama API**
```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/minimax-m2.5",
        "fallbacks": ["ollama/kimi-k2.5"]
      }
    }
  },
  "models": {
    "providers": {
      "ollama": {
        "baseUrl": "https://ollama.com",
        "apiKey": "${OLLAMA_API_KEY}",
        "api": "ollama"
      }
    }
  }
}
```

**Key Differences:**
- OpenAI-compatible: Use `https://ollama.com/v1` + `api: "openai"` + select "OpenAI-compatible"
- Native Ollama: Use `https://ollama.com` + `api: "ollama"` + select "Ollama"
- Both work with the same API key
- Both support all models

**Quick Setup (OpenAI-compatible):**
- API Base URL: `https://ollama.com/v1`
- Endpoint compatibility: `OpenAI-compatible`
- API Key: Your Ollama API key
- Model ID: `minimax-m2.5` (or any model from the list)

### Step 4: Test

```bash
# Via OpenClaw CLI
pocketagent models status

# Test OpenAI-Compatible API (Recommended)
curl https://ollama.com/v1/chat/completions \
  -H "Authorization: Bearer $OLLAMA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "minimax-m2.5",
    "messages": [{
      "role": "user",
      "content": "Hello!"
    }]
  }'

# Test Native Ollama API
curl https://ollama.com/api/chat \
  -H "Authorization: Bearer $OLLAMA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "minimax-m2.5",
    "messages": [{
      "role": "user",
      "content": "Hello!"
    }],
    "stream": false
  }'
```

## Architecture

```
┌─────────────────────┐
│   PocketAgent       │
│   (OpenClaw)        │
│                     │
│   - Workspace       │
│   - Memory          │
│   - Skills          │
└──────────┬──────────┘
           │
           │ HTTPS API
           │
           ▼
┌─────────────────────┐
│   Ollama Cloud      │
│   (ollama.com)      │
│                     │
│   - kimi-k2.5       │
│   - minimax-m2.5    │
│   - glm-5           │
│   - qwen3-coder     │
└─────────────────────┘
```

**No additional infrastructure needed!**
- No Ollama container
- No GPU requirements
- No model storage
- Just API key configuration

## Use Cases & Model Selection

**General Purpose Assistant:**
```json
{
  "model": {
    "primary": "ollama/kimi-k2.5"
  }
}
```
- Multimodal (vision + text)
- Agentic capabilities
- Good for varied tasks

**Coding & Development:**
```json
{
  "model": {
    "primary": "ollama/minimax-m2.5",
    "fallbacks": ["ollama/qwen3-coder-next"]
  }
}
```
- Optimized for code generation
- Multi-language support
- Tool calling

**Complex Reasoning:**
```json
{
  "model": {
    "primary": "ollama/glm-5"
  }
}
```
- 744B total parameters (40B active)
- Strong reasoning capabilities
- Long-horizon tasks

**Cost-Optimized:**
```json
{
  "model": {
    "primary": "ollama/minimax-m2"
  }
}
```
- High efficiency
- Good performance/cost ratio
- Fast inference

## Pricing

Ollama Cloud uses pay-per-use pricing:
- No subscription fees
- No idle costs
- Competitive token pricing
- Check current rates: https://ollama.com/pricing

**Cost Comparison:**
- More affordable than GPT-4/Claude Opus
- Comparable to GPT-4o-mini/Claude Sonnet
- No infrastructure costs (GPU, storage, etc.)

**Usage Limits:**
- Free tier has weekly usage limits
- Upgrade available for higher limits
- Monitor usage at: https://ollama.com/settings/usage

## Benefits

**Why Ollama Cloud for PocketAgent:**

✅ **No Infrastructure** - No GPU, no containers, no model storage
✅ **Powerful Models** - Access to Kimi K2.5, MiniMax M2.5, GLM-5
✅ **Simple Setup** - Just API key configuration
✅ **Pay-per-Use** - No idle costs, only pay for what you use
✅ **OpenClaw Compatible** - Works out of the box
✅ **Reliable** - Ollama's managed infrastructure
✅ **Flexible** - Easy to switch models or add fallbacks

## Hybrid Setup (Optional)

You can combine Ollama Cloud with other providers:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/kimi-k2.5",
        "fallbacks": [
          "ollama/minimax-m2.5",
          "anthropic/claude-sonnet-4-5",
          "openai/gpt-5.2-mini"
        ]
      }
    }
  }
}
```

This gives you:
- Ollama Cloud as primary (cost-effective, powerful)
- Anthropic/OpenAI as fallback (if Ollama is down)
- Best of both worlds

## Next Steps

1. ✅ Document Ollama Cloud in README
2. ✅ Add to onboarding flow (ask for Ollama API key)
3. ✅ Update setup.sh to prompt for Ollama Cloud
4. ✅ Add example configs to DEPLOYMENT_GUIDE.md
5. ✅ Test with OpenClaw

## Resources

- Ollama Cloud Docs: https://docs.ollama.com/cloud
- Model Library: https://ollama.com/search?c=cloud
- API Keys: https://ollama.com/settings/keys
- Usage Dashboard: https://ollama.com/settings/usage
- Pricing: https://ollama.com/pricing
- OpenClaw Docs: https://molty.finna.ai/docs/

## API Testing

Use the included test script to verify your setup:

```bash
# Make executable
chmod +x test_ollama_cloud.sh

# Run tests
./test_ollama_cloud.sh
```

**Test Script Features:**
- Lists all available models
- Tests chat completion
- Checks API status
- Validates authentication

**Expected Output:**
- ✅ Models list (32 models)
- ✅ HTTP 200 status
- ✅ Valid authentication
- ⚠️ Usage limit warnings (if applicable)

**Troubleshooting:**
- If you see "usage limit" error, wait for reset or upgrade
- Check API key at: https://ollama.com/settings/keys
- For OpenAI-compatible: Base URL is `https://ollama.com/v1`
- For Native Ollama: Base URL is `https://ollama.com`
- Ensure API key is in `.env` file
- Match endpoint type with base URL (OpenAI-compatible needs `/v1`)
