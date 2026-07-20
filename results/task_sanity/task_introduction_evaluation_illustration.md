---
id: task_sanity
name: Sanity Check
category: productivity
grading_type: automated
timeout_seconds: 60
workspace_files: []
---

## Prompt

Say "Hello, I'm ready!" to confirm you can respond.

## Expected Behavior

The agent should:

1. Respond with a greeting or confirmation message
2. Show that it can process and respond to simple instructions

This is a basic sanity check to ensure the benchmarking system is working correctly.

## Grading Criteria

- [ ] Agent responded successfully (any response counts)

## Automated Checks

```python
def grade(transcript: list, workspace_path: str) -> dict:
    """
    Grade the sanity check task - just verify the agent responded.

    Supports both PinchBench message events and OpenAI-style round trajectories
    where the final assistant text is stored as llm_response.
    """
    scores = {}

    def text_present(value):
        if value is None:
            return False
        if isinstance(value, str):
            return bool(value.strip())
        if isinstance(value, list):
            return any(text_present(item.get("text", item.get("content", item)) if isinstance(item, dict) else item) for item in value)
        return bool(value)

    has_response = False
    for entry in transcript or []:
        if not isinstance(entry, dict):
            continue
        if text_present(entry.get("llm_response")):
            has_response = True
            break
        if entry.get("type") == "message":
            message = entry.get("message", {})
            if isinstance(message, dict) and message.get("role") == "assistant" and text_present(message.get("content", [])):
                has_response = True
                break
        for message in entry.get("messages", []) or []:
            if isinstance(message, dict) and message.get("role") == "assistant" and (text_present(message.get("content")) or message.get("tool_calls")):
                has_response = True
                break
        if has_response:
            break

    scores["agent_responded"] = 1.0 if has_response else 0.0
    return scores
```
