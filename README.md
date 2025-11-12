# Vascura FRONT

https://github.com/user-attachments/assets/e0d3f51d-e8ca-4f71-b9f4-cdcd587f82e5

### I designed this Frontend around main ideas:
- On-the-Spot Text Editing: You should have fast, precise control over editing and altering text.
- Dependency-Free: No downloads, no Python, no Node.js - just a single compact (300~ kb) HTML file that runs in your browser.
- Focused on Core: Only essential tools and features that serve the main concept.
- Context-Effective Web Search: Should find info and links and fit in 4096 tokens limit.
- OpenAI-compatible API: The most widely supported standard, chat-completion format.
- Open Source under the Apache 2.0 License.

### Features: 

Please watch the video for a visual demonstration of the implemented features.

1. **On-the-Spot Text Editing:**
Edit text just like in a plain notepad, no restrictions, no intermediate steps. Just click and type.
2. **React (Reactivation) System:**
Generate as many LLM responses as you like at any point in the conversation. Edit, compare, delete or temporarily exclude an answer by clicking “Ignore”.
3. **Agents for Web Search:**
Each agent gathers relevant data ([using allOrigins](https://github.com/gnuns/allOrigins/tree/main)) and adapts its search based on the latest messages. Agents will push findings as "internal knowledge", allowing the LLM to use or ignore the information, whichever leads to a better response. The algorithm is based on more complex system but is streamlined for speed and efficiency, fitting within an 4K context window (all 9 agents, instruction model).
4. **Tokens-Prediction System:**
Available when using LM Studio or Llama.cpp Server as the backend, this feature provides short suggestions for the LLM’s next response or for continuing your current text edit. Accept any suggestion instantly by pressing Tab.
5. **Any OpenAI-API-Compatible Backend:**
Works with any endpoint that implements the OpenAI API - LM Studio, Kobold.CPP, Llama.CPP Server, Oobabooga's Text Generation WebUI, and more. With "Strict API" mode enabled, it also supports Mistral API, OpenRouter API, and other v1-compliant endpoints.
6. **Markdown Color Coding:**
Uses Markdown syntax to apply color patterns to your text.
7. **Adaptive Interface:**
Each chat is an independent workspace. Everything you move or change is saved instantly. When you reload the backend or switch chats, you’ll return to the exact same setup you left, except for the chat scroll position. Supports custom avatars for your chats.
8. **Pre-Configured for LM Studio:**
By default, the frontend is configured for an easy start with LM Studio: just turn "Enable CORS" to ON, in LM Studio server settings, enable the server in LM Studio, choose your model, launch Vascura FRONT, and say “Hi!” - that’s it!
9. **Thinking Models Support:**
Supports thinking models that use <think></think> tags or if your endpoint returns only the final answer (without a thinking step), enable the "Thinking Model" switch to activate compatibility mode - this ensures Web Search and other features work correctly.

### allOrigins

- Web Search works via allOirgins - https://github.com/gnuns/allOrigins/tree/main
- By default it will use allorigins.win website as a proxy.
- But by running it locally you will get way faster and more stable results (use LOC version).

### License

Apache 2.0 License.
