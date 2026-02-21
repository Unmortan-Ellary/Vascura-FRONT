# Vascura FRONT

https://github.com/user-attachments/assets/e0d3f51d-e8ca-4f71-b9f4-cdcd587f82e5

### Frontend's Core Ideas:
- **On-the-Spot Text Editing:** Fast and precise control over editing and altering text.
- **Dependency-Free:** No downloads, no Python, no Node.js - just a single compact (500~ kb) HTML file that runs in your browser.
- **Focused:** Only essential tools and features that serve the main concept.
- **Context-Effective Web Search:** Should find info and links and fit in 4096 tokens limit.
- **Macro Engine:** Built-in macro scripting, easily triggered by LLM context parsing.
- **OpenAI-compatible API:** The most widely supported standard, chat-completion format.
- **Open Source:** under the Apache 2.0 License.

### What's new?
- Lorebook Image System.
- Macro Engine System.
- LLM Initiative System.

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
Use the Lorebook System to create text entries and dynamically inject them into the System Prompt as internal LLM memory. Injection is triggered by custom tags detected in the last messages. Lorebook works as database for text data, images, values. Images also will be injected into chat. Parameter "Messages to Scan" will control how often same image will appear in the chat.

11. **LLM Initiative:**
Timer based system that force LLM to take Initiative and start new conversations to engage with the user (even with empty chats), messaging multiple times in a row trying to engage with AFK user on its own, continue to perform given task, acting as different characters each new message (if instructed). Will use Lorebook injections, can use Web Search to find fresh information about last topic of conversation.

12. **Macro Engine:**
Inspired by SillyTavern's macro system, this engine allows macros / functions execution by parsing System Prompt + Lorebook Entries. Example: `Current Date is {{date}}, Time is {{time}}, Today is {{weekday}}, User Language is {{language}}, User will drink {{pick Tea|Milk|Coffee}}, User's happy number for today is {{roll 1d100}}, Favorite songs: {{lore My Favorite Songs List}}, Random Math: {{math {{roll 1d20}} + (5 + 2)}}.` for LLM this text in System Prompt will be converted to `Current Date is 2023-10-27, Time is 12:30, Today is Friday, User Language is en-US, User will drink Milk, User's happy number for today is 78, Favorite songs: My Top 10..., Random Math: 25.` Please read the Macro Engine Guide for more info about each macro.

### Macro Engine Guide:
1. **How it Works:** Right now Macro Engine parse only the final System Prompt (System Prompt + Lorebook Entries). So please use MACRO keys in Lorebook and System Prompt, they will not work anywhere else. Engine will search for MACRO keys like `{{time}}` execute its function and replace it with the result data `14:30`. A temporal processed System Prompt will be created and pushed to LLM, not affecting the permanent System Prompt or Lorebook, so everything will be recalculated every time message is sent to LLM. Keys are not-case sensitive and can be used as `{{time}}`, `{{TIME}}`, `{{Time}}` etc. Engine supports nesting - Macro in Macro, nested Macros executed first.

2. **Simple Data:**
    - **`{{time}}`**: `Local Time is {{time}}` -> `Local Time is 14:30` - Current Time.
    - **`{{date}}`**: `Today date is {{date}}` -> `Today date is 2023-10-27` - Current Date.
    - **`{{weekday}}`**: `Today is {{weekday}}` -> `Today is Friday` - Current Day of the Week.
    - **`{{language}}`**: `User System Language is {{language}}` -> `User System Language is en-US` - Browser Language in a form of `en-US`, `en-GB` etc.
3. **Injections:**
    - **`{{lore ENTRY_NAME}}`**: `You found a Book about Red Dragons: {{lore Red Dragon}}` -> `You found a Book about Red Dragons: Red dragons are ancient...` - Will search Lorebook for Entry Name (Red Dragon) and injects its Entry Prompt along with image, not-case sensitive. If added Lorebook Entry have MACRO keys inside it - those keys also be processed. This MACRO have high priority and will work even if Lorebook is disabled.
    - **`{{image ENTRY_NAME}}`**: `Red Dragon spreads its wings and takes to the air. {{image Red Dragon Flying}}` -> `Red Dragon spreads its wings and takes to the air.` - Image from Lorebook Entry (Red Dragon Flying) will appear in the chat bellow the last USER message.
4. **Randoms**:
    - **`{{roll NdN}}`**: `The Dragon hits you on {{roll 2d6}} HP` -> `The Dragon hits you on 8 HP` - Random Dice Roll function, can generate any kind of rolls that follows NdN loggic: 1d6, 2d10, 3d20. Can be used as simple random number generator: 1d1000 etc.
    - **`{{pick A|B|C}}`**: `You better choose {{pick right|left|middle}} section` -> `You better choose right section` - Random Choice out of 2+ Options.
5. **Variables:**
    - **`{{set VAR_NAME DATA}}`**: `{{set PlayerHP 100}}` - Create or modify the `PlayerHP` variable and set its value to `100`, it can be a number or a text.
    - **`{{get VAR_NAME}}`**: `{{get PlayerHP}}` -> `100` - Injects `PlayerHP` variable value or text.
    - **`{{wipe}}`**: This will CLEAR the entire array of variables for chat, each chat has its own unique array.
6. **Logic:**
    - **`{{math EXPRESSION}}`**: `You have {{math {{get PlayerGold}} + 5}} gold left` -> `You have 15 gold left` - Math functions + - * / (), will remove any text inside the key.
    - **`{{if VALUE COND VALUE ? IF_TRUE:IF_FALSE}}`**: `Is five more than three? Oracle: {{if 5 > 3 ? Yes:No}}` -> `Is five more than three? Oracle: Yes` - Classic IF functions, it can be a number or a text.
        - `>=` - Greater than or equal.
        - `<=` - Less than or equal.
        - `!=` - Not equal.
        - `=` - Equal.
        - `>` - Greater than.
        - `<` - Less than.
        - `includes` - String contains check, `{{if "Is this dress red" includes "red" ? Yes:No}}` -> `Yes`.
7. **Nesting and Syntax Example:**
    ```
    {{ // Everything that is between empty {{}} will be deleted, use this to clean newlines, comments etc.
       This code checks that player has 300 Gold to buy a Sword and place it into one of 3 Inventory Slots.
       At the end script will return result, success or not and the gold left.
    
    {{wipe}}                                                       // Wipes all the variables in the array.
    {{set Gold 1000}}                                              // Gold for the Player.
    {{set Slot1 Axe}}                                              // Player got Axe in Slot 1.
    {{set AddedFlag 0}}                                            // Logic Flag.
    {{set FeedbackText 0}}                                         // For End result.
    {{set CanAfford {{if {{get Gold}} >= 300 ? 1 : 0}}}}           // Checks that Player has 300 gold.
    
    {{set DoAdd {{if {{get Slot1}} = "" ? {{if {{get AddedFlag}} = 0 ? {{get CanAfford}} : 0}} : 0}}}}
    {{set Slot1 {{if {{get DoAdd}} = 1 ? Sword : {{get Slot1}}}}}}
    {{set AddedFlag {{if {{get DoAdd}} = 1 ? 1 : {{get AddedFlag}}}}}}
    {{set FeedbackText {{if {{get DoAdd}} = 1 ? "You buy a sword and place it in slot 1" : {{get FeedbackText}}}}}}
    
    {{set DoAdd {{if {{get Slot2}} = "" ? {{if {{get AddedFlag}} = 0 ? {{get CanAfford}} : 0}} : 0}}}}
    {{set Slot2 {{if {{get DoAdd}} = 1 ? Sword : {{get Slot2}}}}}}
    {{set AddedFlag {{if {{get DoAdd}} = 1 ? 1 : {{get AddedFlag}}}}}}
    {{set FeedbackText {{if {{get DoAdd}} = 1 ? "You buy a sword and place it in slot 2" : {{get FeedbackText}}}}}}
    
    {{set DoAdd {{if {{get Slot3}} = "" ? {{if {{get AddedFlag}} = 0 ? {{get CanAfford}} : 0}} : 0}}}}
    {{set Slot3 {{if {{get DoAdd}} = 1 ? Sword : {{get Slot3}}}}}}
    {{set AddedFlag {{if {{get DoAdd}} = 1 ? 1 : {{get AddedFlag}}}}}}
    {{set FeedbackText {{if {{get DoAdd}} = 1 ? "You buy a sword and place it in slot 3" : {{get FeedbackText}}}}}}
    
    {{set Gold 
      {{math {{get Gold}} - {{math {{get AddedFlag}} * 300}}
      }}
    }}
    
    {{if {{get CanAfford}} = 0 
           ? "Not enough money" 
           : {{if {{get AddedFlag}} = 0 
               ? "No empty slots" 
               : {{get FeedbackText}}
             }}
    }}

    // Result will be Printed bellow.
    }}
    {{get FeedbackText}}, Gold Left {{get Gold}}.
    ```

### Useful Links to OpenAI API Compatible Endpoints (free):
- `https://generativelanguage.googleapis.com/v1beta/openai/v1` (Google API).
- `https://openrouter.ai/api/v1` (OpenRouter API).
- `https://api.cerebras.ai/v1` (Cerebras API).
- `https://api.groq.com/openai/v1` (Groq API).
- `https://api.mistral.ai/v1` (Mistral API).

### allOrigins

- Web Search works via allOrigins - https://github.com/gnuns/allOrigins/tree/main
- By default it will use allorigins.win website as a proxy.
- But by running it locally you will get way faster and more stable results (use LOC version).

### License

Apache 2.0 License.
