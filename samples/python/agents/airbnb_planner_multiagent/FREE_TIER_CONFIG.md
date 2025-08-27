# Google Gemini Free Tier Configuration Guide

## 🎯 Best Free Tier Models (December 2024)

### **🥇 RECOMMENDED: Gemini 1.5 Flash-8B**
- **Model Name**: `gemini-1.5-flash-8b-latest`
- **Free Tier Limits**: 
  - ✅ **1,500 requests per day**
  - ✅ **15 requests per minute** 
  - ✅ **1M input tokens, 8K output tokens**
- **Why Best**: Smallest, fastest, most generous limits
- **Perfect for**: Multi-agent systems with frequent API calls

### **🥈 Alternative: Gemini 1.5 Flash**  
- **Model Name**: `gemini-1.5-flash-latest`
- **Free Tier Limits**:
  - ✅ **1,500 requests per day**
  - ✅ **15 requests per minute**
  - ✅ **1M input tokens, 8K output tokens**
- **Why Good**: More capable but same limits as 8B

### **🥉 Backup: Gemini 1.5 Pro (Limited)**
- **Model Name**: `gemini-1.5-pro-latest`  
- **Free Tier Limits**:
  - ⚠️ **50 requests per day** (much lower)
  - ⚠️ **5 requests per minute**
- **Use Case**: Only for complex reasoning tasks

## 🔧 Updated Configuration

### Update All Agent .env Files

**weather_agent/.env:**
```bash
# RECOMMENDED FREE TIER CONFIG
GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash-8b-latest"
GOOGLE_GENAI_USE_VERTEXAI=FALSE
```

**airbnb_agent/.env:**
```bash
# RECOMMENDED FREE TIER CONFIG  
GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash-8b-latest"
GOOGLE_GENAI_USE_VERTEXAI=FALSE
```

**host_agent/.env:**
```bash
# RECOMMENDED FREE TIER CONFIG
GOOGLE_API_KEY="your_actual_api_key" 
GOOGLE_GENAI_MODEL="gemini-1.5-flash-8b-latest"
GOOGLE_GENAI_USE_VERTEXAI=FALSE
AIR_AGENT_URL=http://localhost:10002
WEA_AGENT_URL=http://localhost:10001
```

## 📊 Free Tier Usage Optimization

### **Daily Usage Strategy**
- **1,500 requests/day** = ~62 requests/hour
- **3 agents** = ~20 requests per agent per hour
- **Perfect for**: Development, testing, demos

### **Rate Limiting Best Practices**
```python
# Built-in retry logic handles rate limits
# No additional configuration needed
```

### **Usage Monitoring Commands**
```bash
# Check current usage (if available)
curl -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  "https://monitoring.googleapis.com/v1/projects/YOUR_PROJECT/timeSeries"

# Monitor API calls in your application logs
tail -f weather_agent_logs.log | grep "API call"
```

## 🚀 Quick Setup Commands

### Step 1: Kill Existing Processes
```bash
pkill -f "uv run" || true
sleep 2
```

### Step 2: Update Configuration Files
```bash
# Update weather agent
echo 'GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash-8b-latest" 
GOOGLE_GENAI_USE_VERTEXAI=FALSE' > weather_agent/.env

# Update airbnb agent  
echo 'GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash-8b-latest"
GOOGLE_GENAI_USE_VERTEXAI=FALSE' > airbnb_agent/.env

# Update host agent
echo 'GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash-8b-latest" 
GOOGLE_GENAI_USE_VERTEXAI=FALSE
AIR_AGENT_URL=http://localhost:10002
WEA_AGENT_URL=http://localhost:10001' > host_agent/.env
```

### Step 3: Start Agents (3 separate terminals)
```bash
# Terminal 1: Weather Agent
cd weather_agent && uv run .

# Terminal 2: Airbnb Agent (wait for weather to start)
cd airbnb_agent && uv run .

# Terminal 3: Host Agent (wait for both to start)  
cd host_agent && uv run .
```

## 💡 Free Tier Tips & Tricks

### **Maximize Your 1,500 Daily Requests**

1. **Development Hours**: 
   - Use during 8AM-6PM for best API response times
   - Avoid peak hours (9AM-11AM PST) when possible

2. **Efficient Query Patterns**:
   - Batch related questions: "weather and accommodation in LA"
   - Use specific queries: "weather in Los Angeles, CA" vs "weather" 
   - Cache responses locally during development

3. **Error Handling**:
   - 429 (Rate Limited): Wait 60 seconds, retry automatically
   - 500 (Server Error): Wait 30 seconds, retry up to 3 times
   - 403 (Quota Exceeded): Wait until next day or upgrade

## 📊 **Comprehensive Google AI Studio Models Free Tier Comparison (Latest - December 2024)**

### **📏 Parameter Size Legend**
- **Exact values** (e.g., `8B`, `1B`): Confirmed parameter counts from model names or official specs
- **Estimated values** (e.g., `~20B`, `~175B`): Estimated based on model class and performance characteristics
- **Size categories**: B = Billion parameters, M = Million parameters
- **Performance rule**: Smaller models = faster inference, larger models = better accuracy/capability

### **🚀 TEXT GENERATION MODELS (Recommended for Multi-Agent Systems)**

| Model Name | Display Name | Parameters | Input Tokens | Output Tokens | Free Requests/Day | Free RPM | Best For | Status |
|------------|-------------|------------|--------------|---------------|-------------------|----------|----------|---------|
| `gemini-1.5-flash-8b-latest` | **Gemini 1.5 Flash-8B Latest** | **8B** | 1M | 8K | **1,500** ✅ | **15** ✅ | **Multi-agent, Fast** | **🏆 RECOMMENDED** |
| `gemini-1.5-flash-8b` | Gemini 1.5 Flash-8B | **8B** | 1M | 8K | **1,500** ✅ | **15** ✅ | Cost-effective | Stable |
| `gemini-1.5-flash-latest` | Gemini 1.5 Flash Latest | **~20B** | 1M | 8K | **1,500** ✅ | **15** ✅ | Versatile | Stable |
| `gemini-1.5-flash` | Gemini 1.5 Flash | **~20B** | 1M | 8K | **1,500** ✅ | **15** ✅ | General purpose | Stable |
| `gemini-1.5-flash-002` | Gemini 1.5 Flash 002 | **~20B** | 1M | 8K | **1,500** ✅ | **15** ✅ | Sept 2024 release | Stable |
| `gemini-1.5-pro-latest` | Gemini 1.5 Pro Latest | **~175B** | 2M | 8K | **50** ⚠️ | **5** ⚠️ | Complex reasoning | Stable |
| `gemini-1.5-pro` | Gemini 1.5 Pro | **~175B** | 2M | 8K | **50** ⚠️ | **5** ⚠️ | Advanced tasks | Stable |
| `gemini-1.5-pro-002` | Gemini 1.5 Pro 002 | **~175B** | 2M | 8K | **50** ⚠️ | **5** ⚠️ | Sept 2024 release | Stable |

### **🆕 GEMINI 2.x MODELS (Latest Generation)**

| Model Name | Display Name | Parameters | Input Tokens | Output Tokens | Free Requests/Day | Free RPM | Best For | Status |
|------------|-------------|------------|--------------|---------------|-------------------|----------|----------|---------|
| `gemini-2.5-flash` | **Gemini 2.5 Flash** | **~30B** | 1M | 65K | **1,500** ✅ | **15** ✅ | **Latest stable** | **🆕 June 2025** |
| `gemini-2.5-flash-lite` | Gemini 2.5 Flash-Lite | **~10B** | 1M | 65K | **1,500** ✅ | **15** ✅ | Lightweight | 🆕 July 2025 |
| `gemini-2.5-pro` | Gemini 2.5 Pro | **~200B** | 1M | 65K | **50** ⚠️ | **5** ⚠️ | Advanced reasoning | 🆕 June 2025 |
| `gemini-2.0-flash` | Gemini 2.0 Flash | **~25B** | 1M | 8K | **1,500** ✅ | **15** ✅ | Fast generation | 🆕 Jan 2025 |
| `gemini-2.0-flash-lite` | Gemini 2.0 Flash-Lite | **~8B** | 1M | 8K | **1,500** ✅ | **15** ✅ | Ultra-fast | 🆕 Latest |

### **🧪 PREVIEW & EXPERIMENTAL MODELS (Limited Free Tier)**

| Model Name | Display Name | Parameters | Input Tokens | Output Tokens | Free Requests/Day | Free RPM | Status | Notes |
|------------|-------------|------------|--------------|---------------|-------------------|----------|---------|--------|
| `gemini-2.5-flash-preview-05-20` | Gemini 2.5 Flash Preview | **~30B** | 1M | 65K | **100** ⚠️ | **10** ⚠️ | Preview | Limited quota |
| `gemini-2.0-flash-exp` | Gemini 2.0 Flash Experimental | **~25B** | 1M | 8K | **100** ⚠️ | **10** ⚠️ | Experimental | Unstable |
| `gemini-2.0-pro-exp` | Gemini 2.0 Pro Experimental | **~200B** | 1M | 65K | **20** ❌ | **2** ❌ | Experimental | Very limited |
| `gemini-exp-1206` | Gemini Experimental 1206 | **~250B** | 1M | 65K | **20** ❌ | **2** ❌ | Experimental | Very limited |

### **🎨 SPECIALIZED MODELS**

| Model Name | Display Name | Parameters | Input Tokens | Output Tokens | Free Requests/Day | Free RPM | Use Case | Notes |
|------------|-------------|------------|--------------|---------------|-------------------|----------|----------|--------|
| `gemini-2.0-flash-exp-image-generation` | Gemini 2.0 Flash (Image Gen) | **~25B** | 1M | 8K | **50** ⚠️ | **5** ⚠️ | Image generation | Limited |
| `learnlm-2.0-flash-experimental` | LearnLM 2.0 Flash | **~25B** | 1M | 32K | **100** ⚠️ | **10** ⚠️ | Educational content | Experimental |

### **📝 EMBEDDING MODELS (Text Similarity/Search)**

| Model Name | Display Name | Parameters | Input Tokens | Output Dims | Free Requests/Day | Free RPM | Best For |
|------------|-------------|------------|--------------|-------------|-------------------|----------|----------|
| `text-embedding-004` | **Text Embedding 004** | **~400M** | 2K | 768 | **1,500** ✅ | **15** ✅ | **Latest embedding** |
| `embedding-001` | Embedding 001 | **~300M** | 2K | 768 | **1,500** ✅ | **15** ✅ | General embedding |
| `gemini-embedding-001` | Gemini Embedding 001 | **~500M** | 2K | 768 | **1,500** ✅ | **15** ✅ | Gemini-based |

### **🤖 GEMMA OPEN SOURCE MODELS**

| Model Name | Display Name | Parameters | Input Tokens | Output Tokens | Free Requests/Day | Free RPM | License |
|------------|-------------|------------|--------------|---------------|-------------------|----------|---------|
| `gemma-3-1b-it` | Gemma 3 1B | **1B** | 32K | 8K | **1,500** ✅ | **15** ✅ | Apache 2.0 |
| `gemma-3-4b-it` | Gemma 3 4B | **4B** | 32K | 8K | **1,500** ✅ | **15** ✅ | Apache 2.0 |
| `gemma-3-12b-it` | Gemma 3 12B | **12B** | 32K | 8K | **1,000** ⚠️ | **10** ⚠️ | Apache 2.0 |
| `gemma-3-27b-it` | Gemma 3 27B | **27B** | 131K | 8K | **500** ⚠️ | **5** ⚠️ | Apache 2.0 |

## 🏆 **TOP RECOMMENDATIONS FOR MULTI-AGENT SYSTEMS**

### **🥇 BEST CHOICE: `gemini-1.5-flash-8b-latest`**
- ✅ **8B parameters** (optimal size for speed vs capability)
- ✅ **1,500 requests/day** (highest free tier)
- ✅ **15 requests/minute** (perfect for multi-agent)
- ✅ **Fastest response times** (smallest Flash model)
- ✅ **Most cost-effective** 
- ✅ **Stable production model**

### **🥈 ALTERNATIVE: `gemini-2.5-flash`** 
- ✅ **~30B parameters** (more capable than 8B)
- ✅ **1,500 requests/day** (same limit)
- ✅ **65K output tokens** (vs 8K in 1.5)
- ✅ **Latest 2.5 generation**
- ⚠️ **Potentially less stable** (newer)

### **🥉 BUDGET OPTION: `gemma-3-1b-it`**
- ✅ **1B parameters** (ultra-lightweight)
- ✅ **1,500 requests/day**
- ✅ **Open source model** (Apache 2.0)
- ✅ **Fastest inference times**
- ⚠️ **Lower capability than Gemini**

## 📊 **Free Tier Limits Summary**

| Tier | Daily Requests | RPM | Models Included |
|------|---------------|-----|-----------------|
| **Premium Free** | **1,500** | **15** | Flash models, Gemma, Embeddings |
| **Standard Free** | **1,000** | **10** | Some Gemma models |
| **Limited Free** | **500** | **5** | Large Gemma models |
| **Preview Free** | **100** | **10** | Preview/experimental |
| **Pro Free** | **50** | **5** | Pro models (1.5/2.5) |
| **Experimental** | **20** | **2** | Cutting-edge experimental |

## 💡 **Model Selection Strategy**

### **For Development & Testing:**
1. `gemini-1.5-flash-8b-latest` - Most requests, fastest
2. `gemini-2.5-flash` - Latest features, same limits
3. `text-embedding-004` - For embeddings/search

### **For Production (Free Tier):**
1. `gemini-1.5-flash-8b-latest` - Proven stability
2. `gemini-1.5-flash-latest` - Reliable alternative
3. Mix with embedding models for hybrid apps

### **For Experimentation:**
1. `gemini-2.5-flash` - Latest capabilities
2. `gemini-2.0-flash` - Good balance
3. Preview models - Cutting edge (limited quota)

## 🔍 Troubleshooting Free Tier Issues

### Common Quota Errors:
```bash
# Error: "Quota exceeded for quota metric 'requests' and limit 'requests per day'"
# Solution: Wait until midnight PST for quota reset

# Error: "Rate limit exceeded"  
# Solution: Built-in retry logic handles this automatically

# Error: "API key not valid"
# Solution: Verify API key in Google AI Studio
```

### Verify Setup:
```bash
# Test API key with free tier model
curl -s "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash-8b:generateContent" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{"contents":[{"parts":[{"text":"Hello"}]}]}'
```

## 📈 Upgrade Path

When you need more requests:
- **Gemini API Pro**: $0.075 per 1K input tokens
- **Vertex AI**: Enterprise features, higher quotas
- **Gemini Advanced**: 2M tokens context, higher limits

---

**💡 Pro Tip**: Start with `gemini-1.5-flash-8b-latest` for development. It's optimized for speed and has the same generous 1,500 requests/day limit as the regular Flash model, perfect for your multi-agent system!
