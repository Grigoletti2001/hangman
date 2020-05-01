![GA Logo](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Hangman OOP Game

## Setup

Fork this repo and clone your fork. Make sure you're doing all the work in your fork. If you did it wrong, delete it and do it right.

## Assignment

Build an OOP Hangman game from scratch.

## Requirements

* Letters must be guessed with keypresses
* Track and inform user of guessed letters and guesses remaining
* User loses after 7 or 8 guesses
* You must be able to win or lose one round (either guess word correctly or die trying).
* You must have a `game` object, a `Word` class, and event listeners/handlers.
* There must be no other code in the global scope, everything goes in either the `game` object, the Word class (or the `Letter` class if you decide to create one), or event listeners/handlers.
* Declare your array (`wordBank`) of possible words in a separate file, linked up in the html _before_ the main `app.js` file.  
* You may use _either_ plain vanilla JavaScript or jQuery for all event handling and DOM manipulation, **but not both**.

## Strong Suggestions

* Functionality to determine if a guess is valid could go in `game` object ***or might best be split between `game` object and `Word` class*** (i.e. `getUserInput()` in game and `checkLetter()` in `Word` (which might also update the data structure, and also call the function that causes the word to reprint itself)).
  * Hint: In addition to changing the Word properties, `checkLetter()` could return true or false... how might this be useful?

* Avoid referencing the `game` object inside the `Word` class if possible; if something has to do with the game more so than the game, it should go in the `game` object. You want your `Word` class to be just word-specific stuff. In general a class should be a totally standalone thing—that you could even drop in and use in another application.

* Event listers/handlers should call `game` object methods containing game play logic. 

* Do NOT put game logic (checking guesses, updating html, keeping score, etc) in listeners/handlers. Just have your listeners/handlers get info from the user, and call `game` methods and pass that info into them.

* You may or may not want a `Letter` class.


## Guidance..

Here's a sensible approach you might use to build this.  We are steering you towards object-oriented app design.  

Also, COMMIT COMMIT COMMIT COMMIT.  At a minimum, you should commit for each one of these steps, but several of them might be multiple commits. 

## `Word`

* Create a `Word` class.  When a round of the game starts, we will instantiate this and store it in the `game` object. 

* The `constructor()` should take a word (string) as an argument.

* Inside the constructor, use the word to create an array of objects.  Each object will correspond to a letter.  Each object will have 2 properties: `character`, which is the letter, and `guessed` which is a boolean that indicates whether the letter has been guessed or not.  You could optionally create a `Letter` class and make these objects instances of that. 

* **Test your `Word` class.** Instantiate a `Word` a few times.  Print the instantiated word in the console and drill down into the object and make sure it instantiates correctly.  Commit when it works.

* Add a method `getWordString()` to the `Word` class.  It should **return** a string corresponding to the word in its current state.  If a letter has been guessed, it should print that letter.  If not, it should print a `_` character.  Be sure to put a space between each character in the returned string so you can see the individual blanks for the unguessed letters. Commit when it works.

* **Test your `getWordString()` method.** Instantiate a few words and call `getWordString()` on them, making sure you get the correct number of `_`s. Manually change some of the letters' `guessed` to `true` and make sure that causes the letters to show up in the returned string. Commit when it works.

* Add a method `checkLetter()` to your `Word` class.  It should:
  * take a `character` as an argument, and
  * go through the array of letter objects and change the `guessed` boolean to true if the guessed letter matches that letter in the array (if the letters are instances of the `Letter` class, maybe `letter` should have a method `markGuessed` that does this), and
  * return `true` if the letter was found in the array, or return `false` otherwise. 
  * A few commits would have been appropriate as you did the above steps.

* **Test your `checkLetter()` method.** Instantiate a few words, and call `checkLetter()` on them and:
  * make sure the return value is either `true` or `false` as appropriate
  * call `getWordString()` on them to make sure the words print correctly.
  * Commit when it works.

Now, you've got a `Word` class (and maybe also a `Letter` class) that could be used in _any_ hangman game.  That's a great thing—you're being modular!  You want to make each part (class or function) of your code as reusable as possible. 


## `game`

Let's move on to the `game` object.

Think about how hangman works.  And take look at the guidelines in the assignment, that's often a good place to start thinking about what code you might want to write, at least at a high level.

> Track and inform user of guessed letters and guesses remaining

* Any time you need to "track" data like this, that's application [**"state"** (check out the "Program State" section)](https://en.wikipedia.org/wiki/State_(computer_science)) data, (i.e. the "state" of your app at any given point).  You should create some properties in your main object to track this type of data, so add an empty `guessedLetters` array, and a `guessesRemaining` value (initial value of 7 or 8 as indicated in instructions).

* What other data do you need to "track" for one round of the game? How about your instantiated word?  What would be a good name for that property? How about `currentWord`. Don't try to set/instantiate the random word directly as the object property, just initialize that property to be `null` for the moment.  Commit maybe?

* But when _will_ the word get initialized?  Well, when the round starts, right?  So create a method `startRound()` that gets a random word from your `wordBank` array, instantiates a `Word` with it, and stores that instance in that `game.currentWord` property (remember to use `this` when referring to the `game` object inside of methods on the `game` object!). Commit when it works.

* **Test your game so far.**  Put a call to `game.startRound()` in the global scope, and then type `game` in your console and drill down and check that a random word got instantiated.  Sweet!  You're committing right?  This would be a great time. 

* What else should happen when the round starts?  Think about playing hangman in real life.  We want the user to guess a letter, right?  But first, we should probably show them the blanks... that's usually how hangman works, right?

* Seems like we're going to need to keep showing the word with its guessed/unguessed letters to the user over and over again, so maybe we should have a method in the game object that prints the word.  Let's call it `printWord()`.  It should use Vanilla JS or jQuery to print the word on the page, perhaps in some container you create in the html.  Pro tip: use CSS to style that container to have a fixed-width font like Courier or Courier New.  When it works, commit.  

* _Ok we're going to stop telling you to commit all the time.  It's up to you to remember to commit frequently as you're building_.

* Now, call the `printWord` method inside of `startRound` after you instantiate your `Word` class.

* Test it again!  You should see your word on the page when you reload.  Check everything by comparing what you see on the page (number of blanks) to what should be there (i.e. check the word's length in the console, see if the number of blanks are the same)

Alright!  Now you're ready to start thinking about guesses.

> Letters must be guessed with keypresses

* Despite the mention of keypresses, _don't worry about the event listener yet._ Add a method called `checkUserInput()` in the `game` object.  It should take a parameter -- a letter, and call the `currentWord`'s `checkLetter()` method.  Think: what should it do next? 

  * `currentWord.checkLetter()` will return `true` or `false` depending on whether the letter was there or not.  How can you use this information? 
  
  * If the letter was there, then what should happen next? The user should get some feedback.  **_Your apps should ALWAYS clearly communicate to the user what is happening._**  If the letter wasn't there, well, we should communicate that to the user too. 

  * So we need a way to display a message to the user in multiple places in our app.  How about a game method called `displayMessage()` that takes a message string as a parameter and displays it in the DOM? 

  * And, in `checkUserInput()` write some conditional logic to tell them something nice if they did good or adminish them if they guessed wrong. 

  * If the guess was bad, don't forget to reduce `guessesRemaining`, and store the guessed letter in `lettersGuessed`!

  * And either way, there are two other things that need to happen:

      * We should show them the word again, in case anything changed.  Do we have a method that prints the `currentWord` in the HTML with its letters and blanks?  (Hint: yes -- Call it!)

      * It would also be nice to tell the user how many guesses they have left. And what letters they've guessed.  In fact, we're going to need to do this a lot of times.  Make a method `printStats()` in the `game` object that prints the `lettersGuessed` and the `guessesRemaining` on the page. 

      * make `checkUserInput()` call that `printStats()` method after it checks a guess.  So now `checkUserInput()` does the following:
        * checks the guess
        * tells the user what happened
        * maybe adjusts `guessesRemaining` and `lettersGuessed` or maybe not
        * calls `printStats()`
        * calls `printWord()`
        * Bonus: if they guess a letter they've already guessed, don't count it against them, instead just display a message saying: "you've already guessed that letter."

  * Throughout this process, notice how we're keeping our logic that prints things on the page separate from our logic.  This [_**separation of concerns**_](https://en.wikipedia.org/wiki/Separation_of_concerns) is a critical concept in Computer Science and programming.

  * **Now** would be a great time to add a keypress or keydown listener.  Add a listener that:
    * figures out what key was pressed
    * calls `game.checkUserInput()` and passes in the key pressed

  * Optional, but nice to include: figure out if the key pressed was a letter.  Use google to see how to figure out if a pressed key is a letter. Once you've got it, then in `checkUserInput()`, have logic like this pseudocode: 
    ```
    If it wasn't a keypress
        tell the user 
    Else 
        everything checkUserInput was already doing
    ```
  * Also nice to include: it shouldn't care about case.  You can accomplish this by lowercasing the all the letters in your comparisons, and where you store the guessed letters.  Just lowercase everything.

> You must be able to win or lose one round (either guess word correctly or die trying).

* Phew!  You've basically got the game done now, you just need to figure out how this critical last step.  There are several different ways to pull this off, and if you've made it this far, you probably have a pretty good idea of how this could work.  Try to think through how to implement it!

  <details>
    <summary>Hints -- only look if you've been stuck on win condition for a while</summary>

    * add an `isCompletelyGuessed()` method to the `Word` class that returns `true` if all the letters are correctly guessed, or false otherwise

    * use it with some conditionals in the `checkUserInput()` method

  </details>


### That's a hangman MVP!  Nice! 


---

## Hungry for More

* Have the game able to play multiple rounds.  After one round, prompt the user to play again.  If they accept, it should reset everything, pick a new word, and keep track of rounds won and lost. 

* Slice up an image of a little man and have his parts appear like a traditional hangman game!

* Research Canvas [here](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) and [here](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial) and make the little man be done using Canvas graphics

