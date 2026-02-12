# Vascura FRONT

https://github.com/user-attachments/assets/e0d3f51d-e8ca-4f71-b9f4-cdcd587f82e5

### Frontend is designed around core ideas:
- **On-the-Spot Text Editing:** Fast and precise control over editing and altering text.
- **Dependency-Free:** No downloads, no Python, no Node.js - just a single compact (400~ kb) HTML file that runs in your browser.
- **Focused:** Only essential tools and features that serve the main concept.
- **Context-Effective Web Search:** Should find info and links and fit in 4096 tokens limit.
- **OpenAI-compatible API:** The most widely supported standard, chat-completion format.
- **Open Source:** under the Apache 2.0 License.

### What's new?
- Macro Engine System.
- LLM Initiative System.
- Lorebook System.

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
By default, the frontend is configured for an easy start with LM Studio: just turn `"Enable CORS"` to ON, in LM Studio server settings, enable the server in LM Studio, choose your model, launch Vascura FRONT, and say “Hi!” - that’s it!

9. **Thinking Models Support:**
Supports thinking models that use `<think></think>` tags or if your endpoint returns only the final answer (without a thinking step), enable the "Thinking Model" switch to activate compatibility mode - this ensures Web Search and other features work correctly.

10. **Lorebook:**
Use the Lorebook System to create text entries and dynamically inject them into the System Prompt as internal LLM memory. Injection is triggered by custom tags detected in the last messages.

11. **LLM Initiative:**
Timer based system that force LLM to take Initiative and start new conversations to engage with the user (even with empty chats), messaging multiple times in a row trying to engage with AFK user on its own, continue to perform given task, acting as different characters each new message (if instructed). Will use Lorebook injections, can use Web Search to find fresh information about last topic of conversation.

12. **Macro Engine:**
Inspired by SillyTavern's macro system, this engine allows macros / functions execution by parsing System Prompt + Lorebook Entries. Example: `Current Date is {{date}}, Time is {{time}}, Today is {{weekday}}, User Language is {{language}}, User will drink {{pick Tea|Milk|Coffee}}, User's happy number for today is {{roll 1d100}}, Favorite songs: {{lore My Favorite Songs List}}, Random Math: {{math {{roll 1d20}} + (5 + 2)}}.` for LLM this text in System Prompt will be converted to `Current Date is 2023-10-27, Time is 12:30, Today is Friday, User Language is en-US, User will drink Milk, User's happy number for today is 78, Favorite songs: My Top 10..., Random Math: 25.` Please read the Macro Engine Guide for more info about each macro.

### Macro Engine Guide (WIP):
1. **How it works:** Right now Macro Engine parse only the final System Prompt (System Prompt + Lorebook Entries + Web Search). So please use this MACRO keys in Lorebook and System Prompt, they will not work anywhere else. Engine will search for MACRO keys like `{{time}}` execute its function and replace it with the result data `14:30`. A temporal processed System Prompt will be created and pushed to LLM, not affecting the permanent System Prompt or Lorebook, so everything will be recalculated every time message is sent to LLM. Keys are not-case sensitive and can be used as `{{time}}`, `{{TIME}}`, `{{Time}}` etc.
2. **MACRO Keys:** 
- {{time}} -> "14:30" - Current Time.
- {{date}} -> "2023-10-27" - Current Date.
- {{weekday}} -> "Friday" - Current Day of the Week.
- {{language}} -> "en-US" - System Language.
- {{roll 2d6}} -> "8" - Random Dice Generator (Any Kind).
- {{pick A|B|C}} -> "B" - Random Choice out of 2+ Options.
- {{lore Red Dragon}} -> "Red dragons are ancient..." - Injects Lore by Entry Name.
- {{math 10 + 5}} -> "15" - Math functions +, -, *, /, () supports Macro nesting.

### Useful Links to OpenAI API Compatible Endpoints:
- `https://generativelanguage.googleapis.com/v1beta/openai/v1` (Google API).
- `https://openrouter.ai/api/v1` (OpenRouter API).
- `https://api.mistral.ai/v1` (Mistral API).

### allOrigins

- Web Search works via allOrigins - https://github.com/gnuns/allOrigins/tree/main
- By default it will use allorigins.win website as a proxy.
- But by running it locally you will get way faster and more stable results (use LOC version).

### License

Apache 2.0 License.
