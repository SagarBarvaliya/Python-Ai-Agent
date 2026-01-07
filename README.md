# Python-Ai-Agent

A minimal local AI-agent experiment using LangChain-style tooling and multiple LLM adapters.

## Summary

This project demonstrates a small research assistant prototype that:
- Loads environment variables with `python-dotenv`.
- Uses LLM adapters (`langchain_openai`, `langchain_anthropic`) to call models.
- (Optionally) composes tools into an agent (search, wiki, save) — tool-based agent features require a compatible `langchain` version or updated code.

## Files

- `main.py` — entrypoint and example usage of the LLM adapters and agent scaffolding.
- `tools.py` — place to implement or export `search_tool`, `wiki_tool`, and `save_tool` for agent use. (Currently empty / placeholder.)
- `requirements.txt` — pinned Python dependencies for the environment.

## Setup (Windows)

1. Create and activate a virtual environment:

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
python -m pip install -r requirements.txt
```

3. Add environment variables in a `.env` file (e.g., API keys required by the LLM adapters).

## Running

Run the script with the venv Python:

```powershell
venv\Scripts\python.exe main.py
```

## Common issue: ImportError for `create_tool_calling_agent`

If you see:

```
ImportError: cannot import name 'create_tool_calling_agent' from 'langchain.agents'
```

Cause: `langchain` version 1.x removed/renamed older helper functions used in this repo. You have two options:

- Pin `langchain` to a 0.x release that exposes the old helpers:

```powershell
python -m pip install --upgrade "langchain<1.0.0"
```

- Or update the code to the current `langchain` API (replace `create_tool_calling_agent` + `AgentExecutor` with `create_agent` and the new factory usage). The code currently includes guarded imports and comments to help porting.

## Implementing tools

To enable agent tool-calling, implement `search_tool`, `wiki_tool`, and `save_tool` in `tools.py`. Each should be an object conforming to LangChain's tool interface (callable or `BaseTool` subclass) depending on the `langchain` version you use.

Example minimal placeholder (replace with real implementations):

```python
def search_tool(query: str) -> str:
	return "[search results placeholder]"

def wiki_tool(topic: str) -> str:
	return "[wiki summary placeholder]"

def save_tool(data: str) -> bool:
	return True
```

## Notes

- `main.py` currently guards imports so it won't crash if your `langchain` or `tools.py` are incompatible; see the file comments.
- Update `requirements.txt` as needed for the LLM providers you plan to use (OpenAI, Anthropic, etc.).

## Next steps

- Implement the real tools in `tools.py`.
- Choose whether to pin `langchain` or port the agent code to the modern API.
- Run the script and iterate on prompt/LLM settings.

If you'd like, I can: pin `langchain` in your venv, port `main.py` to `create_agent`, or add placeholder implementations for `tools.py`. Tell me which and I'll proceed.