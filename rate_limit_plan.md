# Rate Limiting Implementation Plan

## 1. Environment Variable Addition
Add to .env.example:
```
# Delay in seconds between LLM API calls to avoid rate limits (default: 20)
LLM_API_DELAY=20
```

## 2. Code Changes in src/agent/custom_agent.py

1. Add import at the top:
```python
import asyncio
```

2. Add delay after LLM call in get_next_action method:
```python
# In get_next_action method, after line 195:
ai_message = self.llm.invoke(messages_to_process)
await asyncio.sleep(int(os.getenv('LLM_API_DELAY', 20)))  # Add rate limiting delay
```

This implementation will:
- Use environment variable LLM_API_DELAY if set
- Default to 20 seconds if not set
- Add delay after each LLM API call
- Work with both sync and async operations since get_next_action is an async method

Ready to switch to code mode for implementation.