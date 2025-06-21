# App Specification (initial prompt)

As an experienced front-end developer, create a web-app for learning German word articles.

The app should first show the user a text box for choosing a specific topic and the number of words to generate. Once this is chosen, the app should use the Gemini API to generate the requested number of German words of this topic. For each word it should also use Gemini to generate an image, showing the object or concept for this word themed to a particular color. If the word 's article is DER the color should be blue. If it is DIE, red or pink, DAS should be yellow.

Next, the app should show a flashcard-like interface where it first demonstrates the side of the card that only has the word along with three buttons under it (der / die / das). When clicked on the button:

- The card should flip to another side, that shows the correct article along with the image.
- The button should become red if the choice was wrong or green if it was correct.
- A "next" button should appear with a 5 second timer. When clicked or when the timer expires the app should move to the next word (I.e. reset the colors of the der/die/das buttons, hide the Next button and show the front side of the flash card with the next word (but no article) shown).

# Additional Requirements (follow-up requests, summarized)

Based on iterative testing and refinement, the following specific requirements must also be met:

- **Ability to replay a session:** At the end of the game there should be an additional button below "Start a new session" named "Replay session". This would restart the game with all the currently generated words and images.
- **Correct Flashcard Behavior:** The app must strictly enforce the flashcard logic. The front of the card must *only* show the word to be guessed. The answer (the article, colored border, and image) must only become visible *after* the user has made a guess and the card has flipped. The flipping animation must be smooth and correctly hide the front face to reveal the back.
- **Visual Glitch on "Next":** When transitioning to the next card, the old word must not be visible during the flip-back animation. The card should appear blank while flipping back to the front, and only show the new word once it has settled.
- **Image Caching:** Implement a caching mechanism using the browser's `localStorage`. Before generating a new image, check if it's already cached. If so, use the cached version. If not, generate the image and then save it to the cache. The app must also gracefully handle potential `QuotaExceededError` if localStorage is full, and continue running without crashing.
- **Error-Tracking List:** Add a new UI element below the main controls. If a user guesses an article incorrectly, the correct word (e.g., "der Hund") should be added to this "Words to Review" list, styled with its corresponding article color. This list should persist until the end of the session.
- **Settings Dialog:**
    - The UI must include a gear icon to open a "Settings" dialog.
    - **API Key:** The dialog must have a password-style input field for the user to provide their own Gemini API key. This key must be saved to `localStorage`.
    - **API Key Check:** If the user clicks "Start Learning" but the API key is empty, the app should not proceed. Instead, it must automatically open the Settings dialog and display a message prompting the user to enter their key.
    - **Image Prompt:** The dialog must have a multi-line text area allowing the user to customize the image generation prompt. The prompt template must support `{WORD}` and `{COLOR}` placeholders. The default prompt should be: `"A simple, clear, vector art illustration of a {WORD}. The main color of the object should be {COLOR}. Minimalist style, on a clean white background."` The app must programmatically replace `{COLOR}` with "blue", "red", or "yellow" based on the article.
    - **Cache Management:** The dialog must contain a "Clear Image Cache" button that removes all cached images from `localStorage`.
    - **Reset Session:** The dialog must include a button to "Reset" or "Restart", which takes the user back to the initial setup screen, clearing the current session.
- **Input Handling:** The "Number of Words" input field must correctly handle being empty without causing a `NaN` (Not a Number) error.
- **Final Output Format:** The entire application, including React, Babel, and all component logic, must be contained within a single, self-sufficient `index.html` file that can be deployed on a static web host like GitHub Pages.
