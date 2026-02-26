# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
  - The game was confusing and broken when I first ran it. Specifically, the hints were completely backwards; guessing 1 prompted me to "Go LOWER," while guessing 100 prompted me to "Go HIGHER." Additionally, the "New Game" button failed to reset the game state correctly, causing an immediate "Game over" error loop instead of starting a fresh round.
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").
  - I noticed two major bugs immediately upon starting the application. First, the feedback logic was inverted, giving the opposite advice of what was needed to win. Second, the game state management was flawed, leading to a crash when attempting to restart the game after losing.
---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
  - For this project, I utilized Gemini Code Assist as my primary AI coding companion. I used it to help analyze the codebase, identify logic errors, and suggest fixes for the bugs I encountered. It served as a helpful guide for navigating the unfamiliar Streamlit framework.
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
  - The AI correctly identified that the hint logic in `check_guess` was swapped, causing the "Go HIGHER" and "Go LOWER" messages to appear at the wrong times. It explained that when `guess > secret`, the code was returning "Too High" but advising to "Go HIGHER" instead of "Go LOWER". I verified this fix by running the app and testing the boundary values of 1 and 100, confirming the hints were now logically consistent.
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
  - One misleading suggestion occurred when the AI immediately proposed refactoring code into `logic_utils.py` to fix a bug, rather than first explaining the root cause in the existing `app.py`. This jumped ahead of my understanding, making it harder to trace the actual fix. I resolved this by asking clarifying questions to understand the logic before moving files around.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
  - I decided a bug was fixed by manually running the Streamlit application and reproducing the specific scenario that caused the issue. If the game behaved as expected without crashing or giving false information, I considered the fix successful. I also relied on the visual feedback from the "Developer Debug Info" expander to verify internal state values.
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
  - I performed a manual test by entering the value 100, which previously told me to "Go HIGHER," which is impossible given the range limit. After applying the fix, I verified that entering 100 correctly prompted me to "Go LOWER." This confirmed that the comparison logic in the code was finally operating correctly.
- Did AI help you design or understand any tests? How?
  - The AI helped me understand testing by explaining what the expected behavior should be for specific edge cases. While I primarily ran manual tests, the AI's explanation of the logic flow helped me design better mental test cases. It clarified why certain inputs were producing specific outputs, allowing me to target my testing more effectively.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
  - The secret number appeared to change because the original code contained a bug that converted the secret number into a text string on every other turn. In Python, comparing strings works differently than comparing numbers; for example, the string "10" is considered "less than" the string "5". This caused the game to give conflicting "Too High" and "Too Low" hints for the same number, creating the illusion of a shifting target.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
  - Streamlit operates like a goldfish with no short-term memory; every time you interact with a widget, it forgets everything and reruns the entire script from the top. Session state acts as a notepad where the app can write down important information, like the score or secret number, to remember it between these resets. Without session state, the game would restart completely after every single guess.
- What change did you make that finally gave the game a stable secret number?
  - I fixed the unstable secret number by removing the specific block of code that converted `st.session_state.secret` into a string on even-numbered attempts. By deleting this logic, I ensured that the secret always remained an integer throughout the game. This guaranteed that the comparison logic remained consistent, eliminating the erratic behavior where hints would flip-flop.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - I plan to reuse the strategy of treating AI strictly as a tool rather than outsourcing my entire thinking process to it. I realized that I need to understand the underlying logic and implications of the code myself before I can effectively use what the AI generates. If I don't understand the "why" behind the code, I cannot debug it when it inevitably breaks or behaves unexpectedly.
- What is one thing you would do differently next time you work with AI on a coding task?
  - Next time, I will spend significantly more time in the planning stage to identify bugs and outline the logic structure on my own before prompting the AI. I intend to use specific comments like `#FIXME` to guide the AI to fix small, isolated sections step-by-step rather than asking for broad solutions. This approach prevents the AI from hallucinating complex solutions that are hard to verify and keeps the focus on the specific problem at hand.
- In one or two sentences, describe how this project changed the way you think about AI generated code.
  - This project changed my perspective by demonstrating that AI requires precise, step-by-step instructions derived from my own understanding to be effective. It reinforced the idea that AI serves best as a translator of my thoughts into syntax, rather than a replacement for the architectural thinking itself. Without clear guidance, AI generated code can easily diverge into incorrect or overly complicated directions.
