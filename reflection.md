# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

When I first ran the app, the hints were completely backwards - guessing too high told me to go higher, and guessing too low told me to go lower, making it impossible to converge on the answer. On top of that, every even-numbered attempt would silently switch to string comparison, so a guess of 9 would be "greater than" 42 because `"9" > "4"` lexicographically. The New Game button also didn't properly reset the game status, so after winning once I was stuck and couldn't play again without refreshing the page.

---

## 2. How did you use AI as a teammate?

I used Claude Code (Claude Sonnet) as my AI assistant throughout this project. One correct suggestion was identifying that the `except TypeError` block was an intentional glitch triggered by the caller passing `secret` as a string on even attempts - I verified this by reading the code at lines 158–161 and confirming the `str()` conversion was deliberate. One suggestion I pushed back on was when Claude immediately tried to change `attempts = 1` to `0` without explaining why - I asked for a clearer explanation first because in the UI I was initially seeing `0` and wasn't sure the bug was real. After the explanation I confirmed the off-by-one was genuine.

---

## 3. Debugging and testing your fixes

I verified each fix by reading the relevant code before and after the change, then manually testing the scenario in the running Streamlit app. For the hint direction bug, I tested by intentionally guessing a number I knew was too high and checking that the app now said "Go LOWER!" instead of "Go HIGHER!". For the New Game status bug, I played until I won, then clicked New Game and confirmed the game resumed instead of blocking me with `st.stop()`. The double-click issue was confirmed by observing that one click now correctly processes the guess.

---

## 4. What did you learn about Streamlit and state?

In Streamlit, the entire script reruns from top to bottom every time the user interacts with the page. In the original app, the secret number was generated with `random.randint()` outside of session state, so it would pick a new number on every rerun. The fix was to only generate the secret once using `if "secret" not in st.session_state`, so it persists across reruns. I'd explain it to a friend like this: imagine your Python script is a webpage that fully refreshes every time you click a button - session state is like a clipboard that survives those refreshes, while regular variables get thrown away each time.

---

## 5. Looking ahead: your developer habits

One habit I want to reuse is reading and understanding code before agreeing to any fix - several times in this project, an AI suggestion looked right on the surface but had a subtle issue (like swapping labels vs. messages in the `except` block). In the future I'd also write a quick manual test case in my head before applying a fix, to make sure the change actually addresses the root cause and not just a symptom. This project showed me that AI-generated code can contain multiple layered bugs - some intentional, some not - and that blindly trusting the output without critical review would have left several issues unfixed.
