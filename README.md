# Hangman AI Tooling Game Challenge
This is a task to build out a HTTP RESTful API for a hangman game using Github Copilot. The game should allow players to create, retrieve and delete a game, along with modifying the game's state by making guesses. The development of the API has been started but there are bugs with logic, missing tests and the logic to make a guess is pending.

This challenge can be developed with C# (using .NET 8), Java (version 21), Golang(version 1.23.X), Javascript (node version 22) or Python (version 3.14.x). 

## Submission Of Challenge
To submit a challenge, a pull request from a forked repository should be created against this repository. It should:
- Implement requirements below.
  - You should try and build this application with best practices in mind; using constants where possible and implementing architectural patterns that make sense.
- Ensure test coverage exists for API request handlers and logic functions.
- Pass all Github Action checks triggered by `pull_request`.

## Learning Objectives
- Use Copilot effectively for code generation.
- Use prompts to guide Copilot’s suggestions.
- Use Copilot to generate tests and improve code.
- Understand Copilot’s context awareness with multi-file projects.

## Before You Start
The recommended approach for starting this exercise is to:
1. (optional) Install git onto your computer. See [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) 
2. (optional) Create a new github account [here](https://github.com/join), you can also use an existing account if you have one. 
3. (optional) Sign up for a Github Copilot account [here](https://github.com/github-copilot/free_signup).
4. (optional) Download and install Visual Studio Code.
    - NOTE: For this exercise we're using visual studio code; however it's also possible to use JetBrains applications or Visual Studio etc.
5. (optional) Install Copilot plugin and set it up in Visual Studio Code:
    - Open Visual Studio Code.
    - Go to the Extensions view by clicking on the Extensions icon in the Activity Bar on the side of the window or by pressing `Ctrl+Shift+X` on windows or `Cmd+Shift+X` on a mac.
    - Search for "GitHub Copilot" and click the Install button.
    - After installation, you may need to sign in to your GitHub account to activate the extension.
    - Follow the prompts to complete the setup process.
6. Fork this repository into your github account.
7. Review [here](#Testing-the-game) for guidance on executing requests against the API.
8. Follow the specific `README.md` instructions for the chosen platform for any additional information.

## Requirements
Below are the high level requirements for the Hangman Game API:

1. **Initialize Game**
   - **Endpoint:** `POST /games`
   - **Description:** Creates a new Hangman game.
   - **Response:**
     - 201 Created:
       - `gameId`: Unique identifier for the game.
       - `maskedWord`: The word to guess with letters masked (e.g., "____").

2. **Process Letter Guess**
   - **Endpoint:** `PUT /games/<game_id>`
   - **Description:** Processes a guess for a specific game.
   - **Request Body:**
     - `letter`: The letter being guessed.
   - **Response:**
     - 200 OK:
       - `updated_word`: The word with correctly guessed letters revealed.
       - `attempts_remaining`: Number of attempts left.
       - `status`: Current status of the game (e.g., "In Progress", "Won", "Lost").
     - 400 Bad Request:
       - `error`: Error message indicating invalid input.

3. **Check Game Status**
   - **Endpoint:** `GET /games/<game_id>`
   - **Description:** Retrieves the current state of a specific game.
   - **Response:**
     - 200 OK:
        - `gameId`: Unique identifier for the game.
        - `maskedWord`: The word to guess with letters masked.
        - `attemptsRemaining`: Number of attempts left.
        - `guesses`: The letters that have been guessed so far.
        - `status`: Current status of the game (e.g., "In Progress", "Won", "Lost").
    - 404 Not Found

4. **Clear Game**
   - **Endpoint:** `DELETE /games/<game_id>`
   - **Description:** Removes a specific game.
   - **Response:**
     - 204 No Content
     - 404 Not Found

### Game Flow

```mermaid
graph TD;
  A[Start] -->|POST /games| B[Initialize Hangman Game]
  B --> C{Generate Word}
  C -->|Random Word Selected| D[Store Game State]
  D -->|"GET /games/&lt;game_id&gt;"| E[Return Game ID & Masked Word]
  E -->|"PUT /games/&lt;game_id&gt;"| F[Process Guess]
  E -->|"DELETE /games/&lt;game_id&gt;"| M[Game Deleted]
  F --> G{Valid Guess?}
  G -- Yes --> H[Update Game State]
  H --> I{Game Won?}
  I -- Yes --> J[Return Won!]
  I -- No --> K{Attempts Remaining?}
  K -- No --> L[Return Lost!]
  K -- Yes --> E
  G -- No --> N[Return Invalid Input]
```

### Logic Requirements

1. **Generate Word**
   - Randomly select a word from a predefined list of words.
   - Store the selected word and its masked version in the game state.

2. **Valid Guess**
   - Check if the guessed letter is a valid alphabet character.
   - Check if the guessed letter has not been guessed before.
   - Handle both upper and lower case characters

3. **Update Game State**
   - If the guessed letter is in the word, reveal the letter in the masked word.
   - If the guessed letter is not in the word, decrement the number of attempts remaining.
   - Update the game status based on the current state (e.g., "won" if all letters are guessed, "lost" if no attempts remain).

4. **Retrieve Game State**
   - Return the current state of the game, including the masked word, attempts remaining, and game status.

## Testing the Game

Platform specific runtimes and README guides can be found within the repository platform folders; however once the application is running the API usage is the same across the languages. All of the hangman APIs run on port 4567 and can be accessed from http://localhost:4567

For this exercise we recommend using [Postman](https://www.postman.com/downloads/?utm_source=postman-home) for making the requests above. 

NOTE: If interested additional tutorials for postman general usage and additionally around using `Postbot` to build API Tests, guidance can be found [here](https://learning.postman.com/)

## Further Reading
In additional to this exercise, there are some great resouces for additional content:
- [Copilot VsCode Cheat Sheet](https://code.visualstudio.com/docs/copilot/copilot-vscode-features)
- [Microsoft Learning Path](https://learn.microsoft.com/en-us/training/paths/copilot/)
- [Open AI Prompt Engineering Best Practices](https://platform.openai.com/docs/guides/prompt-engineering)