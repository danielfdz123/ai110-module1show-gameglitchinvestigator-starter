# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior  | Console Output / Error |
| ----- | ----------------- | ---------------- | ---------------------- |
| 1     | "go higher" hint  | "go lower" hint  | none                   |
| 100   | "go lower" hint   | "go higher" hint | none                   |

## More bugs

- swithing difficulties changed the attempts count, eventually getting a negative value
- "easy" difficulty has less attempts than the "normal" difficulty
- new game button doesnt start a new game
- changing difficulty should start a new game instead of affecting count total (switching from hard -> easy increases amount of tries, but number stays the same)

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
  - I used Claude Code inside VS Code as a debugging partner. I described the glitchy game I was seeing when trying it myself and I asked it to explain what was actually happening in the code before changing anything.

- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
  - When I said "changing difficulty gives me a new number like a new game," the AI pointed out that the real bug was the opposite: the secret is only generated once and never updates when the range changes, so a leftover number could fall outside the new range (e.g. 75 on Easy's 1–20). I verified this using the Developer Debug Info panel, which showed the secret staying the same after I switched difficulty.

- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

  -The AI said that all difficulties went about their corresponding attempt count, but I noticed that all it did was simply change the number of attempts, which was intended, but the secret number stayed the same, giving me this idea that the user can simply give themselves more tries mid-game by changing the difficulty

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
  - Running multiple attempts on the app. One of them was by guessing very high and very low guesses to see if they were accurate or not
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
  - When adjusting the score upon winning, I tested the `update_score` function directly with assertions instead of clicking through the app each time. I checked that winning on the first attempt returns 90 (it used to return 80 because of an extra `+ 1` in the formula) and that a "Too High" guess on an even attempt no longer ADDS points. All assertions passed, which confirmed both the win-scoring off-by-one and the "wrong guess raises your score" glitch were actually fixed.
- Did AI help you design or understand any tests? How?
  - Yes. The AI suggested testing the scoring function in isolation rather than only playing the game, and helped me pick edge cases that would have failed before the fix (first-guess win = 90, showing "Too High" and "Too Low" when returning the same point penalty). This made it fast to prove the math was right without restarting Streamlit.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
  - Every time you click a button or change a setting, Streamlit re-runs the entire script from top to bottom, like reloading the whole page. That means normal variables get wiped and recreated each run, so anything you want to remember between clicks (the secret number, the score, the attempt count) has to live in `st.session_state`, which survives across reruns. This is also why I had to queue win/error messages and the balloons in session state and show them on the next run instead of right when the event happened.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
  - I want to keep the habit of having the AI explain what the code is actually doing before I let it change anything. Reading the code with it first stopped me from "fixing" the wrong thing (like the difficulty bug, where the real problem wasn't what I assumed). I also want to keep testing functions in isolation with quick assertions instead of only clicking through the UI.
- What is one thing you would do differently next time you work with AI on a coding task?
  - Next time I'd reproduce and confirm the bug myself first, then describe it precisely to the AI, instead of giving it a vague symptom. When I was vague ("it gives me a new number"), the AI agreed with my wrong description at first, and we only caught it by reading the code.
- In one or two sentences, describe how this project changed the way you think about AI generated code.
  - I learned that AI-generated code can look polished and "production-ready" while still hiding several real bugs, so I can't trust it just because it runs. The AI is most useful as a debugging partner that explains and verifies, not as a source of code I accept without checking.