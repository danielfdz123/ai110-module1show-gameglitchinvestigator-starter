# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [X] Describe the game's purpose.
   - This is a simple guessing number game from 1 to X that varies by difficulty. Players attempts would also varry depending on difficulty, but there will always be a given hint depending on what our guess is to help us out.
- [X] Detail which bugs you found.

   - The hints were backwards (Shows go lower then the number was higher & vise-versa)
   - The secret number never updated when you changed difficulty, so a leftover number could fall outside the new range (e.g. 75 while on Easy's 1–20), making the game unwinnable.
   - Changing difficulty kept your old attempts/score instead of starting a fresh game, giving you extra tries on the same number.
   - Winning on the first guess scored 80 instead of 90 because of an off-by-one in the points formula.
   - A "Too High" guess on an even attempt ADDED points, so a wrong guess could raise your score.
   - Easy difficulty originally gave us 6 tries instead of 10
- [X] Explain what fixes you applied.
   - Hints are displayed more accuaretly
   - Changing difficulties starts a new game instead of allowing users to get new tries
   - Removed the extra `+ 1` from the win-points formula so a first-guess win scores 90.
   - Made wrong guesses penalize consistently (both "Too High" and "Too Low" subtract 5), removing the even-attempt bonus.
   - Adjusted attempt accounts for each difficulty
   - Balloons properly displayed when winning
   - Refactored the game logic into `logic_utils.py` so it can be unit-tested with pytest.

## 📸 Demo Walkthrough

Describe your fixed game in numbered steps so a reader can follow along without watching a video:

1. User enters a random number as theri guess (Ex: 50)
2. Game returns a hint saying "Too Low" or "Too High"
3. User enters another guess with help of the hint
4. Score updates correctly after each guess
5. Game ends after user runs out of attempts or they guessed the correct secret number

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
$ python -m pytest -q
...                                                                      [100%]
3 passed in 0.01s
```

All three logic tests pass after refactoring the game logic into `logic_utils.py`:
- `test_winning_guess` — equal guess/secret returns "Win"
- `test_guess_too_high` — guess above secret returns "Too High"
- `test_guess_too_low` — guess below secret returns "Too Low"

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]