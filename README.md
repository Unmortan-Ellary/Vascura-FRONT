# // Vascura FRONT
https://github.com/user-attachments/assets/e0d3f51d-e8ca-4f71-b9f4-cdcd587f82e5

### Frontend's Core Ideas:
- **Simplicity:** Easy to understand interface, curated features with "less is more" ideology.
- **Dependency-Free:** No downloads, no Python, no Node.js - just a single compact (450~ kb) HTML file that runs in your browser.
- **On-the-Spot Text Editing:** Fast and precise control over editing and altering text.
- **Context-Effective Web Search:** Should find info and links and fit in 4096 tokens limit.
- **Lorebook:** Dynamically injects text, macro scripts, images.
- **Macro Engine:** Built-in macro scripting language.
- **OpenAI-compatible API:** The most widely supported standard, chat-completion format.
- **Open Source:** under the Apache 2.0 License.

### What's new?
- New Macro `{{llm}}` allows to chain small LLM Sub-Requests.
- Duplicate Chat Message function via `ALT` + `Copy` Button.
- `First Message` from Lorebook on Chat Reset.
- New Macro `{{kwarg}}` that controls "chat_template_kwargs" parameters.
- Character Cards Support (Chub.AI etc) via `Vascura CARD Inspector` Tool.

### Features: 

1. **On-the-Spot Text Editing:**
Edit text just like in a plain notepad, no restrictions, no intermediate steps. Just click and type.

2. **React (Reactivation) System:**
Generate as many LLM responses as you like at any point in the conversation. Edit, compare, delete or temporarily exclude an answer by clicking “Ignore”.

3. **Agents for Web Search:**
Each agent gathers relevant data ([using allOrigins](https://github.com/gnuns/allOrigins/tree/main)) and adapts its search based on the latest messages. Agents will push findings as "internal knowledge", allowing the LLM to use or ignore the information, whichever leads to a better response. The algorithm is based on more complex system but is streamlined for speed and efficiency, fitting within an 4K context window (all 9 agents, instruction model).

4. **Token-Prediction System:**
Available when using LM Studio or KoboldCpp as backend, this feature provides short suggestions for the LLM’s next response or for continuing your current text edit. Accept any suggestion instantly by pressing `Tab`.

5. **Any OpenAI-API-Compatible Backend:**
Works with any endpoint that implements the OpenAI API - LM Studio, Kobold.CPP, Llama.CPP Server, Oobabooga's Text Generation WebUI, and more. With "Strict API" mode enabled, it also supports Mistral API, OpenRouter API, and other v1-compliant endpoints.

6. **Markdown Color Coding:**
Uses Markdown syntax to apply color patterns to your text.

7. **Adaptive Interface:**
Each chat is an independent workspace. Everything you move or change is saved instantly. When you reload the backend or switch chats, you’ll return to the exact same setup you left, except for the chat scroll position. Supports custom avatars for your chats.

8. **Pre-Configured for LM Studio:**
By default, frontend is configured for an easy start with LM Studio: just turn `"Enable CORS"` to ON, in LM Studio server settings, enable the server in LM Studio, choose your model, launch Vascura FRONT, and say “Hi!” - that’s it!
     <img width="1341" height="335" alt="image" src="https://github.com/user-attachments/assets/033f48e0-879d-43f1-904e-479115672bc2" />


9. **Thinking Models Support:**
Supports thinking models that use `<think></think>` tags or if your endpoint returns only the final answer (without a thinking step), enable the "Thinking Model" switch to activate compatibility mode - this ensures Web Search and other features work correctly.

10. **Lorebook:**
Use Lorebook System to create text entries (with macro scripts support) and dynamically inject them into the System Prompt as internal LLM memory. Injection is triggered by custom tags detected in the last messages. **Lorebook Image System:** Images also will be injected into chat, parameter "Messages to Scan" controls how often same image will be injected. **First Message:** If Lorebook have entry titled "First Message" it will be placed as first "LLM Message" in the Chat (by pressing `🧹` Clean / Reset Chat).

11. **LLM Initiative:**
Timer based system that force LLM to take Initiative and start new conversations to engage with the user (even with empty chats), messaging multiple times in a row trying to engage with AFK user on its own, continue to perform given task, acting as different characters each new message (if instructed). Will use Lorebook injections, can use Web Search to find fresh information about last topic of conversation. **Backfill Mode:** In this mode LLM scan Chat from the start and for each USER Message that doesn’t have a corresponding LLM Reply - Generates an appropriate response. You can create five USER Messages in a row with different instructions and LLM will execute them one by one, filling the gaps.

12. **Macro Engine:**
Inspired by SillyTavern's macro engine, this engine allows macros / scripts execution by parsing System Prompt + Lorebook Entries. Example: `Current Date is {{date}}, Time is {{time}}, Today is {{weekday}}, User Language is {{language}}, User will drink {{pick Tea|Milk|Coffee}}, User's happy number for today is {{roll 1d100}}, Favorite songs: {{lore My Favorite Songs List}}, Random Math: {{math {{roll 1d20}} + (5 + 2)}}.` for LLM this text in System Prompt will be converted to `Current Date is 2023-10-27, Time is 12:30, Today is Friday, User Language is en-US, User will drink Milk, User's happy number for today is 78, Favorite songs: My Top 10..., Random Math: 25.` Please read [Macro Engine Guide](https://github.com/Unmortan-Ellary/Vascura-FRONT/tree/main#macro-engine-guide) for more info about each macro.

13. **VAR Snapshot System:**
Each `USER Message` stores a VAR `Snapshot` state of variables, flags, texts that were set at the time of this USER Message was created. This allows to freely regenerate any LLM Message using exact data that was actual on this moment or to correctly form next USER Message. You can return to any point and continue your convesration without loosing any data that was stored using `{{set}}` macro in VAR Array at this exact moment. If you have a complex RPG system full of variables with flags, stats, texts etc, it will work as a save point.

### Tools:
- [Vascura CARD Inspector](https://github.com/Unmortan-Ellary/Vascura-FRONT/blob/main/Tools/Vascura%20CARD%20Inspector%20v0.413.html): A Reader \ Converter Tool for Character Cards (V1-V2-V3 Chub.AI etc). Vascura FRONT can imitate work of `SillyTavern` or `Serene Pub` using its Lorebook System and Macro Engine (for single card), but first CHARA Card need to be converted into the right format (Lorebook Card) - this Tool do exactly that. You can inspect any CHARA card and EXPORT it for Vascura FRONT with different options. Also supports native Lorebook JSONs from Vascura FRONT.
     <img width="998" height="613" alt="2brave_screenshot" src="https://github.com/user-attachments/assets/d68620de-2954-4845-89ce-cd54fd8d8e61"/>


### Macro Engine Guide:
1. **How it Works:** Right now Macro Engine parse only the final System Prompt (System Prompt + Lorebook Entries). So please use Macro keys in Lorebook and System Prompt, they will not work anywhere else. Engine will search for Macro keys like `{{time}}` execute its function and replace it with result data `14:30`. A temporal processed System Prompt will be created and pushed to LLM, not affecting the permanent System Prompt or Lorebook, so everything will be recalculated every time message is sent to LLM. Keys are not-case sensitive and can be used as `{{time}}`, `{{TIME}}`, `{{Time}}` etc. Engine supports nesting - Macro in Macro, nested most inner Macros will be executed first.
     
     <img width="1001" height="296" alt="image" src="https://github.com/user-attachments/assets/6fe2c524-79a8-4b4c-8199-31531bf55b99"/>

2. **Simple Data:**
    - **`{{time}}`**: `Local Time is {{time}}` -> `Local Time is 14:30` - Current Time.
    - **`{{date}}`**: `Today date is {{date}}` -> `Today date is 2023-10-27` - Current Date.
    - **`{{weekday}}`**: `Today is {{weekday}}` -> `Today is Friday` - Current Day of the Week.
    - **`{{language}}`**: `User System Language is {{language}}` -> `User System Language is en-US` - Browser Language in a form of `en-US`, `en-GB` etc.
3. **Injections:**
    - **`{{lore ENTRY_NAME}}`**: `You found a Book about Red Dragons: {{lore Red Dragon}}` -> `You found a Book about Red Dragons: Red dragons are ancient...` - Will search Lorebook for Entry Name (Red Dragon) and injects its Entry Prompt along with image, not-case sensitive. If added Lorebook Entry have MACRO keys inside it - those keys also be processed. This MACRO have high priority and will work even if Lorebook is disabled.
    - **`{{image ENTRY_NAME}}`**: `Red Dragon spreads its wings and takes to the air. {{image Red Dragon Flying}}` -> `Red Dragon spreads its wings and takes to the air.` - Image from Lorebook Entry (Red Dragon Flying) will appear in the chat bellow the last USER message.
    - **`{{anote TEXT}}`**: `{{anote - Think like Dragon.}}` - Temporary adds `- Think like Dragon.` to the end of the last USER Message with newline, works similar to classic Author's Note.
    - **`{{llm NUMBER TEXT}}`**: `{{llm 2 Summarize content in one sentence, use "SHORT:Text" format}}` -> `SHORT: Hero found a Book about Red Dragons...` - Calls LLM Sub-Request using `Context` (Recent `2` Chat Messages) and `Prompt` (`Summarize content in one sentence, use "SHORT:Text" format`). Allows Light-Agent like usage - `NUMBER` controls how many Chat Messages (recent ones) to use as `Context` for Sub-Request, `TEXT` works as `System Prompt` and as `Latest Message from USER`. If Sub-Request returns macros, they will be processed. Can be nested to form a chain that use previous Sub-Request result in next one. Split complex instructions into small pin-point tasks each with its own `Context` and `Prompt` to follow. Using with `{{anote}}` macro allows to push instruction from LLM as direct USER command for Main Request.
4. **Random**:
    - **`{{roll NdN}}`**: `The Dragon hits you on {{roll 2d6}} HP` -> `The Dragon hits you on 8 HP` - Random Dice Roll function, can generate any kind of rolls that follows NdN loggic: 1d6, 2d10, 3d20. Can be used as simple random number generator: 1d1000 etc.
    - **`{{pick A|B|C}}`**: `You better choose {{pick right|left|middle}} section` -> `You better choose right section` - Random Choice out of 2+ Options.
5. **Variables:**
    - **`{{set VAR_NAME DATA}}`**: `{{set PlayerHP 100}}` - Create or modify the `PlayerHP` variable and set its value to `100`, it can be number or text.
    - **`{{get VAR_NAME}}`**: `{{get PlayerHP}}` -> `100` - Injects `PlayerHP` variable value or text.
    - **`{{wipe}}`**: This will CLEAR the entire array of variables for chat, each chat has its own unique array.
6. **Logic:**
    - **`{{math EXPRESSION}}`**: `You have {{math {{get PlayerGold}} + 5}} gold left` -> `You have 15 gold left` - Math functions + - * / (), will remove any text inside the key.
    - **`{{if VALUE COND VALUE ?? IF_TRUE::IF_FALSE}}`**: `Is five more than three? Oracle: {{if 5 >> 3 ?? Yes::No}}` -> `Is five more than three? Oracle: Yes` - Classic IF functions with a twist, it can be number or text. Twist: It is not a branching function by itself, if you put code in `IF_TRUE` or `IF_FALSE` both parts will be executed.
        - `>=` - Greater than or equal.
        - `<=` - Less than or equal.
        - `!=` - Not equal.
        - `==` - Equal.
        - `>>` - Greater than.
        - `<<` - Less than.
        - `includes` - String contains check, `{{if "Is this dress red" includes "red" ?? Yes::No}}` -> `Yes`.
7. **Samplers:**
   - **`{{model MODEL_NAME}}`**: `{{model Skyfall-31b-Q3-16k}}` - Temporary overrides current `Model Name (Optional)` setting with different name (`Skyfall-31b-Q3-16k`), `MODEL_NAME` should match one of listed models from `Model Name (Optional)` setting. You can switch to different models for LLM Sub-Requests using `{{llm}}` macro, for example `Gemma 4 31b` to create an Answer Plan for Main-Request that will use `Skyfall 31b`. You can use `Qwen 3 4b` that will analyze chat history and then choose the right model for answer: `Gemma 4 31b` for Creative \ Chat or `Qwen 3.8 27B` for Coding \ Agentic answer. Works with any endpoint that supports model switching, works fast even with `llama.cpp` set to `--models-max 1` (only single model loaded at a time) due to RAM cache.
   - **`{{kwarg KWARG_NAME VALUE}}`**: `{{kwarg enable_thinking false}}` - Adds `"chat_template_kwargs": {"enable_thinking": false}` argument for next message, KWARGs usually used to disable `thinking` of the model or to switch any other parameter. Each single `{{kwarg}}` parameter should have its own macro line, if same `{{kwarg}}` parameter repeated multiple times in the same request, last one will be used.
        - Core KWARGs: `enable_thinking`, `thinking`, `preserve_thinking`, `reasoning_effort`.
   - **`{{sampler SAMPLER_NAME VALUE}}`**: `{{sampler temperature 0.2}}` - Temporary overrides existing sampler with different value (`temperature` from 1.0 to 0.2) or adds totally new one supported by Endpoint, for example `presence_penalty` have no dedicated slider, but it can be used just by adding `{{sampler presence_penalty 1.5}}` to System Prompt or Lorebook. You can create sampling presets in Lorebook and then fast-switch between them by pinning the right Lorebook entry. You can randomize any sampler using `{{sampler temperature 0.{{roll 1d9}}}}`. VALUE can be a number, false\true or text. All LlamaCpp supported [samplers](https://github.com/ggml-org/llama.cpp/tree/master/tools/server#api-endpoints).
        - Core Samplers: `temperature`, `top_k`, `top_p`, `min_p`, `typical_p`, `repeat_penalty`, `presence_penalty`, `frequency_penalty`, `xtc_probability`, `xtc_threshold`, `dry_multiplier`, `dry_base`, `dry_allowed_length`, `dry_penalty_last_n`, `dynatemp_range`, `dynatemp_exponent`, `n_predict`, `seed`, `ignore_eos`, etc.
        
9. **Nesting and Syntax Example:**
    ```
    {{ // Everything that is between empty {{}} will be deleted, use this to clean newlines, comments etc.
       - This code checks that player has 300 Gold to buy a Sword.
       - Search for empty Inventory Slot (if any).
       - Set Sword in this Inventory Slot and remove 300 gold from balance.
       - Inform about results in the end.
    
    {{wipe}}                                                       // Wipes all the variables in the array.
    {{set Gold 1000}}                                              // Gold for the Player.
    {{set Slot1 Axe}}                                              // Player got Axe in Slot 1.
    {{set AddedFlag 0}}                                            // Logic Flag.
    {{set FeedbackText 0}}                                         // For End result.
    {{set CanAfford {{if {{get Gold}} >= 300 ?? 1 :: 0}}}}         // Checks that Player has 300 gold.
    
    {{set DoAdd {{if {{get Slot1}} == "" ?? {{if {{get AddedFlag}} == 0 ?? {{get CanAfford}} :: 0}} :: 0}}}}
    {{set Slot1 {{if {{get DoAdd}} == 1 ?? Sword :: {{get Slot1}}}}}}
    {{set AddedFlag {{if {{get DoAdd}} == 1 ?? 1 :: {{get AddedFlag}}}}}}
    {{set FeedbackText {{if {{get DoAdd}} == 1 ?? "You buy a sword and place it in slot 1" :: {{get FeedbackText}}}}}}
    
    {{set DoAdd {{if {{get Slot2}} == "" ?? {{if {{get AddedFlag}} == 0 ?? {{get CanAfford}} :: 0}} :: 0}}}}
    {{set Slot2 {{if {{get DoAdd}} == 1 ?? Sword :: {{get Slot2}}}}}}
    {{set AddedFlag {{if {{get DoAdd}} == 1 ?? 1 :: {{get AddedFlag}}}}}}
    {{set FeedbackText {{if {{get DoAdd}} == 1 ?? "You buy a sword and place it in slot 2" :: {{get FeedbackText}}}}}}
    
    {{set DoAdd {{if {{get Slot3}} == "" ?? {{if {{get AddedFlag}} == 0 ?? {{get CanAfford}} :: 0}} :: 0}}}}
    {{set Slot3 {{if {{get DoAdd}} == 1 ?? Sword :: {{get Slot3}}}}}}
    {{set AddedFlag {{if {{get DoAdd}} == 1 ?? 1 :: {{get AddedFlag}}}}}}
    {{set FeedbackText {{if {{get DoAdd}} == 1 ?? "You buy a sword and place it in slot 3" :: {{get FeedbackText}}}}}}
    
    {{set Gold 
      {{math {{get Gold}} - {{math {{get AddedFlag}} * 300}}
      }}
    }}
    
    {{if {{get CanAfford}} == 0 
      ?? "Not enough money" 
      :: {{if {{get AddedFlag}} == 0 
          ?? "No empty slots" 
          :: {{get FeedbackText}}
         }}
    }}
    
    // Result will be Printed bellow.
    }}
    {{get FeedbackText}}, Gold Left {{get Gold}}.
    ```

### Hotkeys:
- `ALT + COPY (Message Button)` - Duplicate Chat Message.
- `ALT + F` or `Double-Click on DOTS` - Focus Mode.
- `F8` - Lorebook.

### Useful Links to OpenAI API Compatible Endpoints (free):
- `https://generativelanguage.googleapis.com/v1beta/openai/v1` (Google API).
- `https://openrouter.ai/api/v1` (OpenRouter API).
- `https://api.groq.com/openai/v1` (Groq API).
- `https://api.mistral.ai/v1` (Mistral API).

### allOrigins:

- Web Search works via allOrigins - https://github.com/gnuns/allOrigins/tree/main
- By default it will use allorigins.win website as a proxy.
- But by running it locally you will get way faster and more stable results (use LOC version).

### License:

Apache 2.0 License.
