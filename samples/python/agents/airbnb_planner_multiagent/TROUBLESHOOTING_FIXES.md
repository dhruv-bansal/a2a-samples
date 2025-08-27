# Multi-Agent System Troubleshooting Fixes

## Current Issues Identified:

1. **A2A Task Management Error**: `Task does not exist`
2. **Google GenAI 500 Server Error**: API rate limiting or temporary issues
3. **Session state synchronization** between agents

## Immediate Fixes:

### 1. Update Model Configuration

Replace `gemini-2.5-flash` with a more stable model in all `.env` files:

```bash
# Update all .env files with:
GOOGLE_GENAI_MODEL="gemini-1.5-flash"
```

### 2. Environment Variables Fix

Ensure consistent configuration across all agents:

**weather_agent/.env:**
```bash
GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash"
GOOGLE_GENAI_USE_VERTEXAI=FALSE
```

**airbnb_agent/.env:**
```bash
GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash"
GOOGLE_GENAI_USE_VERTEXAI=FALSE
```

**host_agent/.env:**
```bash
GOOGLE_API_KEY="your_actual_api_key"
GOOGLE_GENAI_MODEL="gemini-1.5-flash"
GOOGLE_GENAI_USE_VERTEXAI=FALSE
AIR_AGENT_URL=http://localhost:10002
WEA_AGENT_URL=http://localhost:10001
```

### 3. Clean Restart Procedure

```bash
# Step 1: Kill all existing processes
pkill -f "uv run" || true
pkill -f "weather_agent" || true
pkill -f "airbnb_agent" || true
pkill -f "host_agent" || true

# Step 2: Wait for ports to be free
sleep 5

# Step 3: Start agents in correct order (separate terminals)

# Terminal 1: Weather Agent
cd samples/python/agents/airbnb_planner_multiagent/weather_agent
uv run .

# Wait for "Server started" message, then...

# Terminal 2: Airbnb Agent
cd samples/python/agents/airbnb_planner_multiagent/airbnb_agent
uv run .

# Wait for "Server started" message, then...

# Terminal 3: Host Agent
cd samples/python/agents/airbnb_planner_multiagent/host_agent
uv run .
```

### 4. Verification Steps

After restart, verify each agent:

```bash
# Test Weather Agent
curl http://localhost:10001/.well-known/agent-card.json

# Test Airbnb Agent  
curl http://localhost:10002/.well-known/agent-card.json

# Test Host Agent UI
open http://localhost:8083
```

### 5. Alternative Model Options

If `gemini-1.5-flash` still has issues, try:

```bash
# Option A: Gemini 1.5 Flash Latest
GOOGLE_GENAI_MODEL="gemini-1.5-flash-latest"

# Option B: Gemini 2.0 Flash
GOOGLE_GENAI_MODEL="gemini-2.0-flash"

# Option C: Gemini 1.5 Flash 002
GOOGLE_GENAI_MODEL="gemini-1.5-flash-002"
```

## Error-Specific Solutions:

### A2A Task Error Solution:
- **Cause**: State synchronization issues between agent sessions
- **Fix**: Clean restart clears all session state
- **Prevention**: Use consistent session IDs and task management

### Google API 500 Error Solutions:
- **Rate Limiting**: Wait 1-2 minutes between tests
- **Quota Issues**: Check Google Cloud Console quotas
- **Model Access**: Verify model availability in your region
- **Retry Logic**: The system has built-in retries

## Testing Procedure:

1. **Start agents individually** and watch logs for errors
2. **Test each agent endpoint** before starting the next
3. **Use simple queries first**: "weather in LA" before complex multi-agent queries
4. **Monitor logs** for detailed error information
5. **Check API quotas** in Google Cloud Console

## Common Issues:

| Issue | Symptom | Solution |
|-------|---------|----------|
| Task ID Error | "Task does not exist" | Clean restart all agents |
| 500 API Error | "Internal server error" | Change model or wait for rate limit reset |
| Connection Error | Agent not accessible | Check if agent started properly |
| Auth Error | "Invalid API key" | Verify API key in .env files |

## Success Indicators:

✅ **All agents return valid agent cards**
✅ **Host agent UI loads at localhost:8083**  
✅ **Simple weather query works: "weather in LA"**
✅ **Simple accommodation query works: "find room in LA"**
✅ **No task ID errors in logs**
✅ **No 500 API errors in logs**
