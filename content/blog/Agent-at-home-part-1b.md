+++
title = "Agent at Home Part 1b"
date = "2025-11-30T22:40:01+01:00"
draft = true

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["agent at home series","ai agents","developer tooling","generative ai","software engineering"]
+++

Alright, we covered the conceptual stuff in Part 1a. Now it's time to actually
look at code. In this post I'm going to walk through the entire `agent.py` file
line by line. By the end you'll see that there really is no magic here, just a
while loop, some API calls, and tool execution. Let's dig in.

## Getting Started

Before we dive into the code, let's get the environment set up. I'm using
[uv](https://github.com/astral-sh/uv) for Python project management because
it's fast and just works. If you don't have it installed, check out their docs.
It's a single curl command.

To set everything up from scratch:

```bash
# Create a new project folder
mkdir agent-at-home
cd agent-at-home

# Initialize a new uv project
uv init
```

Now replace the contents of the generated `pyproject.toml` with:

```toml
[project]
name = "agent-at-home"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "litellm>=1.72.0",
    "python-dotenv>=1.2.1",
]
```

Then install dependencies and set up your API key:

```bash
# Install dependencies
uv sync

# Set up your API key based on which provider you want to use
export ANTHROPIC_API_KEY="your-key-here"  # For Claude models
export OPENAI_API_KEY="your-key-here"     # For GPT models
export GEMINI_API_KEY="your-key-here"     # For Gemini models

# Run the agent, this is for when we have finished writing the agent.py code
uv run python agent.py
```

You only need to export the key for the provider you're actually using. If you
set `MODEL_NAME = "anthropic/claude-haiku-4-5-20251001"` in the code, you need
`ANTHROPIC_API_KEY`. If you switch to `"openai/gpt-4o"`, you need
`OPENAI_API_KEY`. And so on.

That's it. Two dependencies: `litellm` for the unified LLM interface across
providers, and `python-dotenv` if you prefer keeping your API key in a `.env`
file instead of exporting it. Nothing else needed.

## The Imports

```python
import asyncio
import json
from pathlib import Path

from litellm import acompletion

MODEL_NAME = "anthropic/claude-haiku-4-5-20251001"
MAX_STEPS = 10
```

Nothing fancy here. We're using `asyncio` because LLM API calls can be slow and
we want to stream responses. `json` for parsing tool arguments. `pathlib`
because it's just nicer than string manipulation for file paths.

The interesting import is `litellm`. This is the one dependency I'm allowing
myself because it abstracts away the differences between OpenAI, Anthropic,
Google, and other providers into a single unified interface. The `acompletion`
function is the async version of their completion call. This means I can swap
`MODEL_NAME` to `"openai/gpt-4o"` or `"gemini/gemini-2.0-flash"` and everything
just works. No framework magic, just a thin wrapper over HTTP calls.

`MAX_STEPS` is our safety valve. We don't want the agent running forever if it
gets stuck in some weird loop. Ten iterations is plenty for most tasks.

## The Agent Class

```python
class CodingAgent:
    def __init__(self, working_dir="."):
        self.working_dir = Path(working_dir).resolve()
        self.messages = []
        self.running = True
```

Three pieces of state. That's it.

- `working_dir`: Where the agent operates. All file paths are relative to this.
- `messages`: The conversation history. This is the context I mentioned in
  Part 1a.
- `running`: A flag to keep the main loop going.

## The Main Loop

```python
async def run(self):
    print("\nCoding Agent Started")
    print("Commands: /clear /exit")
    print("Tip: Press enter twice to submit\n")

    while self.running:
        try:
            user_input = self.get_input()
            if not user_input:
                continue
            if user_input.startswith("/"):
                self.handle_command(user_input)
            else:
                await self.react_loop(user_input)
        except KeyboardInterrupt:
            print("\n\nGoodbye!")
            break
        except Exception as e:
            print(f"\nError: {e}")
```

This is the outer loop. Not the ReAct loop, just the user interaction loop. Get
input, check if it's a command (like `/clear` or `/exit`), otherwise hand it
off to the ReAct loop. The `KeyboardInterrupt` catch is so you can ctrl+c out
gracefully.

## Getting User Input

```python
def get_input(self):
    print("\nYou:")
    lines = []
    while True:
        line = input("> " if not lines else "  ")
        if not line and lines:
            break
        if line:
            lines.append(line)
    return "\n".join(lines)
```

This is a small quality of life thing. Instead of single-line input, you can
type multiple lines and submit by pressing enter on an empty line. The prompt
changes from `>` to `  ` (indented) after the first line so you know you're in
multi-line mode. Nothing revolutionary but makes it nicer to paste in code
blocks or longer prompts.

## Commands

```python
def handle_command(self, command):
    if command == "/clear":
        self.messages = []
        print("Context cleared.")
    elif command == "/exit":
        print("\n\nGoodbye!")
        self.running = False
    else:
        print(f"Unknown command: {command}")
```

Two commands for now. `/clear` wipes the conversation history which is useful
when context gets too long or you want to start fresh. `/exit` does what you'd
expect. We'll add more commands as the series progresses, things like `/undo`,
`/save`, etc.

## The System Prompt

```python
def get_system_prompt(self):
    prompt_path = Path(__file__).parent / "system_prompt.txt"
    return prompt_path.read_text()
```

The system prompt lives in a separate file. This is intentional. You'll be
tweaking this constantly as you figure out what instructions work best. Having
it in a separate file means you don't have to touch Python code to iterate on
prompting. The current system prompt is minimal:

Quick note on `pathlib` if you haven't used it much: `Path` objects overload
the `/` operator for path joining. So `self.working_dir / "somefile.txt"` isn't
division, it's equivalent to `os.path.join(self.working_dir, "somefile.txt")`
but reads much cleaner. You'll see this pattern throughout the code.


```
You are a coding assistant that uses tools to help with file operations.

Use the ReAct pattern:
1. Think through the problem
2. Use tools to gather information or make changes
3. Explain what you did

Available tools:
- read_tool: Read a file's contents
- list_files: List files in a directory
- write_tool: Create or update a file

Be concise and clear. Ask for clarification when needed.
```

We explicitly tell the model to use the ReAct pattern. This is that Chain of
Thought prompting technique I mentioned in Part 1a. We're asking it to show its
reasoning steps.

## Defining Tools

```python
def get_tools(self):
    return [
        {
            "type": "function",
            "function": {
                "name": "read_tool",
                "description": "A tool to read files",
                "parameters": {
                    "type": "object",
                    "properties": {"path": {"type": "string"}},
                    "required": ["path"],
                },
            },
        },
        # ... write_tool and list_files follow the same pattern
    ]
```

This is the OpenAI function calling format (which litellm normalizes across
providers). Each tool has a name, description, and a JSON Schema for its
parameters. The description matters a lot because that's what the model uses to
decide when to call each tool.

Three tools to start:
- `read_tool`: Read file contents
- `write_tool`: Write content to a file
- `list_files`: List directory contents

These are the basics you need for a coding agent. We'll add more sophisticated
tools later (grep, glob, bash execution, etc.) but this is enough to be useful.

## Executing Tools

```python
async def execute_tool(self, name, input_data):
    try:
        if name == "read_tool":
            path = self.working_dir / input_data["path"]
            return {"content": path.read_text()}
        elif name == "write_tool":
            print(f"\nWrite to {input_data['path']}? (y/n): ", end="")
            if input().strip().lower() != "y":
                return {"error": "Cancelled"}
            path = self.working_dir / input_data["path"]
            path.parent.mkdir(parents=True, exist_ok=True)
            path.write_text(input_data["content"])
            return {"success": True}
        elif name == "list_files":
            dir_path = self.working_dir / input_data.get("path", ".")
            files = [
                {"name": item.name, "type": "dir" if item.is_dir() else "file"}
                for item in dir_path.iterdir()
            ]
            return {"files": files}
        return {"error": f"Unknown tool: {name}"}
    except Exception as e:
        return {"error": str(e)}
```

This is where tools actually do things. A few things to note:

1. All paths are joined with `working_dir`. This is a basic sandboxing measure.
2. `write_tool` asks for confirmation before writing. This is important! You
   don't want an agent silently overwriting your files. We'll explore more
   sophisticated approaches later (diffs, undo, etc.) but for now a simple y/n
   works.
3. `list_files` returns structured data, not just strings. This makes it easier
   for the model to reason about the results.
4. Everything is wrapped in try/except. Tool execution failing shouldn't crash
   the agent. We just return an error message and let the model figure out what
   to do.

## The ReAct Loop (The Heart of It All)

```python
async def react_loop(self, user_input):
    self.messages.append({"role": "user", "content": user_input})

    for step in range(1, MAX_STEPS + 1):
        print(f"\n[Step {step}]")
        print("Assistant: ", end="", flush=True)

        messages_with_system = [
            {"role": "system", "content": self.get_system_prompt()}
        ] + self.messages

        response = await acompletion(
            model=MODEL_NAME,
            max_tokens=4000,
            tools=self.get_tools(),
            messages=messages_with_system,
            stream=True,
        )
```

Here's where the magic (that isn't magic) happens. Remember from Part 1a:
Thought -> Action -> Observation -> repeat.

1. Add the user message to history
2. Loop up to `MAX_STEPS` times
3. Each iteration: send the full conversation (system prompt + message history)
   to the model with the tool definitions

The `stream=True` is why we need all that chunk handling code coming next.
Streaming means we get tokens as they're generated instead of waiting for the
full response.

```python
        collected_content = ""
        tool_calls = []

        async for chunk in response:
            delta = chunk.choices[0].delta
            if delta.content:
                print(delta.content, end="", flush=True)
                collected_content += delta.content
            if delta.tool_calls:
                for tc in delta.tool_calls:
                    if tc.index >= len(tool_calls):
                        tool_calls.append({"id": tc.id or "", "name": "", "arguments": ""})
                    if tc.function and tc.function.name:
                        tool_calls[tc.index]["name"] = tc.function.name
                        print(f"\n[Tool: {tc.function.name}]")
                    if tc.function and tc.function.arguments:
                        tool_calls[tc.index]["arguments"] += tc.function.arguments
                    if tc.id:
                        tool_calls[tc.index]["id"] = tc.id
        print()
```

This chunk handling code is a bit gnarly, I'll admit. Tool calls come in pieces
across multiple chunks, so we need to accumulate them. The `tc.index` tells us
which tool call this chunk belongs to (models can request multiple tools at
once). We print content as it streams in for that nice typing effect.

```python
        assistant_message = {"role": "assistant", "content": collected_content}
        if tool_calls:
            assistant_message["tool_calls"] = [
                {
                    "id": tc["id"],
                    "type": "function", 
                    "function": {"name": tc["name"], "arguments": tc["arguments"]},
                }
                for tc in tool_calls
            ]
        self.messages.append(assistant_message)

        if not tool_calls:
            return
```

After collecting the full response, we add it to message history. If there were
no tool calls, we're done. The model has finished reasoning and given us a
final answer. This is the exit condition for our ReAct loop.

```python
        for tc in tool_calls:
            arguments = json.loads(tc["arguments"]) if tc["arguments"] else {}
            result = await self.execute_tool(tc["name"], arguments)
            print(f"[Result: {str(result)[:200]}...]")
            self.messages.append({
                "role": "tool",
                "tool_call_id": tc["id"],
                "content": json.dumps(result),
            })
```

If there were tool calls, execute them and add the results to message history
with the special `"role": "tool"` format. The `tool_call_id` links the result
back to the specific tool call. Then we loop back to the top and let the model
reason about these new observations.

That's the ReAct loop. Thought (model response) -> Action (tool execution) ->
Observation (tool result added to context) -> repeat until no more actions
needed.

## Entry Point

```python
async def main():
    print("=" * 50)
    print(f"Coding Agent ({MODEL_NAME})")
    print("=" * 50)
    agent = CodingAgent()
    await agent.run()

if __name__ == "__main__":
    try:
        asyncio.run(main())
    except KeyboardInterrupt:
        pass
```

Standard async entry point. Create agent, run it. The outer `KeyboardInterrupt`
catch is a fallback in case ctrl+c happens before the agent's main loop starts.

## Wrapping Up

And that's it. Around 220 lines of Python and you have a working coding agent.
No frameworks, no abstractions you don't understand. Every line is here, every
decision is visible.

Is this production ready? No. Is it competitive with Claude Code or Cursor? Not
even close. But that was never the point. The point is that you now understand
exactly what's happening when an agent "thinks" and "acts". There's no magic.
It's a while loop, some API calls, and executing tools based on the model's
decisions.

Here's the fun part: you can now use the agent to improve itself. Point it at
its own codebase and ask it to add a new tool, refactor the streaming logic, or
improve error handling. It can read `agent.py`, understand what's there, and
write changes. There's something satisfying about that recursive loop. Using
the thing you built to make the thing you built better.

In the next part we'll start adding more sophisticated features. Better tools,
smarter context management, maybe a nicer TUI. But the core loop you've seen
here? That stays the same.

The full code is available on GitHub:
[github.com/ramtinJ95/agent-at-home](https://github.com/ramtinJ95/agent-at-home/tree/Version-1-base)
(branch: `Version-1-base`). Clone it, run it, break it, improve it.

What I cannot create, I do not understand. Now you understand.
