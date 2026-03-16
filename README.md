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

**Game Purpose:**
A number guessing game built with Streamlit where the player tries to guess a secret number within a limited number of attempts. The game supports three difficulty levels (Easy, Normal, Hard) with different number ranges and attempt limits.

**Bugs Found:**

1. **Wrong hints** : `check_guess` returned "Go HIGHER!" when the guess was too high and "Go LOWER!" when it was too low (swapped).
2. **Intentional string comparison glitch** : On even-numbered attempts, `secret` was passed as a string, causing lexicographic comparison (e.g. `"9" > "42"` is `True`) instead of numeric comparison.
3. **Hard mode easier than Normal** : Hard had range 1–50 while Normal had 1–100. Hard should be harder.
4. **New Game ignored difficulty** : The "New Game" button always used `randint(1, 100)` regardless of selected difficulty.
5. **Hardcoded info message** : The UI always said "Guess a number between 1 and 100" regardless of difficulty.
6. **Attempts initialized to 1** : The attempts counter started at 1 before any guess was made, making "Attempts left" off by one from the start.
7. **Invalid guess burned an attempt** : Typing "abc" and clicking Submit would decrement the attempt counter even though no valid guess was made.
8. **New Game didn't reset status** : After winning, clicking "New Game" left `status = "won"`, so the game immediately blocked the player with `st.stop()`.
9. **Double-click needed to submit** : Using `st.text_input` outside a form caused a focus-loss rerun before the button click registered.

**Fixes Applied:**

- Swapped the hint messages in `check_guess` to return the correct direction.
- Removed the deliberate string conversion of `secret` in the submit block, making the `except TypeError` path unreachable.
- Changed Hard difficulty range to 1–200.
- Updated "New Game" to use `randint(low, high)` based on current difficulty.
- Updated the info message to dynamically show `{low}` and `{high}`.
- Changed attempts initialization from `1` to `0`.
- Moved `attempts += 1` inside the `else` block so only valid guesses count.
- Added `status = "playing"` and `history = []` reset to the New Game handler.
- Wrapped `st.text_input` and Submit in an `st.form` to fix the double-click issue.

## 📸 Demo

- [ ] [Insert a screenshot of your fixed, winning game here]

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, insert a screenshot of your Enhanced Game UI here]
