# Modificated Jinja Chat Templates for Vascura FRONT.
- Based on Unsloth or llama.cpp templates.

## Pros.
- All mods follow the same usage logic.
- No thinking by default, faster answers on easy tasks.
- Faster Web Search phases without thinking, since it is a simple sum task.
- Simple `thinking` phase activation just by adding `/think` anywhere in the System Prompt, by hand or by Lorebook script.
- Simple `preserve_thinking` activation just by adding `/preserve` anywhere in the System Prompt, by hand or by Lorebook script (Qwen 3.6 Only).

## How to use.
- llama.cpp: just add `--chat-template-file D:\Gemma-4-26-31b-VASCURA-Mod.jinja` to the launch parameters.
- LM Studio: paste whole code from file inside `Prompt Template` -> `Template (jinja)`.
- Other backends should have similar options.
