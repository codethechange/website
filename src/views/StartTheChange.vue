<template>
  <div class="container-fluid">
    <div class="intro">
      <h1 id="hook">Start the Change</h1>
      <p class = 'hook-sub'>
        Get started with Code the Change by working through this starter project.
      </p>
    </div>
    <div id="white">
      <div id="content">
        <!-- Line breaks in the markdown are treated literally -->
        <VueShowdown markdown="
# Introduction

Welcome to the 2020 Code the Change onboarding process! As part of your first task, you will create a simple tic-tac-toe game in a JavaScript-supported browser. We won’t require you to use a specific framework, but the following guide will strongly emphasize the use of React with Hooks.

Please join the `#start-the-change` channel on Slack. Here you'll be able to ask questions about the project, get help, and see other members' questions. We encourage you to answer questions in that channel too!

## Our Goals

We hope that creating this game helps introduce you to what Code the Change is all about, starts to build your familiarity with web development, and gives you practice with troubleshooting. We also hope that you'll have fun building it! We also look forward to seeing what you create. At the end of your project, you will show us what you built to give us a better understanding of what your background and interests are.

Now, we want to be very clear that this starter project is not a coding challenge like those used by companies to screen job applicants. **Your performance on the starter project does not affect whether you can join Code the Change or what opportunities are available to you.** This project is about you learning and us getting to know you, not us evaluating you.

## Project Specification

At the end of this project, you should have a functioning, browser-based tic-tac-toe game that lets two players place their markers in turn. Note that the players can be on the same browser session so that your game can run as a static site. Once a player wins, the game should say so. For example:

<img src='./assets/starter/progress_functional_game.gif' alt='A tic-tac-toe game being played.' width='500em'>

In addition, we strongly encourage you to expand on this core game with a more aesthetically appealing interface or additional functionality. This guide provides one example of each.

The rest of this guide walks you through one way of building this game, which you are welcome to follow. However, we encourage you to experiment and surprise us with what you come up with! Code the Change involves a lot of self-directed problem solving and creativity, so you'll get a better picture of what Code the Change is all about if you deviate from this guide a little and explore on your own.

Once you're done with your app, you will upload your code to GitHub and deploy your game to the web. More detailed instructions are at the end of this guide. Once you finish, reach out to the club leaders so we can help find a project that matches your interests and experience.

## What is React?

React is a powerful and versatile JavaScript framework created by Facebook that enables developers to build their user interface and functionality as reusable components. In our case, we will build a tic-tac-toe game using 3 components: one for the squares, one for the board, and another for the complete game logic.

In React, there are two types of components: class components and functional components. If you’re interested in the differences between the two, take a look at this article [here](https://medium.com/@Zwenza/functional-vs-class-components-in-react-231e3fbd7108). The relevant difference here is that functional components are stateless, which makes it difficult to change behavior dynamically within a component. While class components can set its own state, creating a class requires writing more code.

Luckily, with the recent introduction of [Hooks](https://reactjs.org/docs/hooks-intro.html), there is now a way to set state within your functional components. Our implementation was written with Hooks, but feel free to use normal class components as well.

## Getting Started

Please clone the [repository for the starter code](https://github.com/codethechange/code-the-change-starter-2020) and follow the setup instructions in `README.md`. Most (if not all) of your code will be written in the src folder. The files that we expect you to need are `components/Square.js`, `components/Board.js`, `components/Game.js`, `calculate_win.js`, and `index.css`. Nonetheless, feel free to add helper files.

If you have trouble at any point throughout this process, please reach out! Part of this task will be to learn how to ask for help and find the right questions to ask. Regardless of your experience with software development, you may find some aspects of these projects difficult. It is totally normal to hit roadblocks on the road to completion, and we want to make sure that you get over the finish line. Good luck!

# Core Project

## Setting Up a Square

As an initial step, we’ll need to create the most basic building block of our program–the square.

* First, create a new CSS class called `squares` in `index.css`. This class should define a square’s background color, border, font size, etc. You may use the following CSS properties, but feel free to experiment and add/remove things.
    * Our basic program will use the same properties for X and O values, but feel free to add classes such as `X` and `O` that have different properties depending on the value (like fonts and colors).
    * **Note**: For font sizes, always aim to use relative units that depend on browser or HTML-wide font settings such as `em` and `rem` over fixed units like `px`. We will primarily be using `rem` here for its simplicity but if you are interested in understanding the differences between `em` and `rem` a bit more, check out this [blog post](https://zellwk.com/blog/rem-vs-em/)!
  ```
  .squares {
    background: white;
    color: black;
    border: none;
    font-size: 5rem;
    cursor: pointer;
    outline: none;
  }
  ```
* Within `components/Square.js`, pass two parameters within your Square component, `value` and `onClick`. `value` will correspond to the current value of the square (“X”, “O”, or null), while `onClick` will be the function that determines what your program should do when a square is clicked.
* In your return block, create a button with `squares` as the `className` attribute along with `onClick` as the `onClick` attribute. Let `value` be the element that appears in the button.
    * To get you started, your button should come in the form `<button className=... onClick=...>...</button>`
    * **Note**: When embedding JavaScript (JS) code within a JSX file, always use curly braces around the code being executed. In this example, `onClick` should be passed in as `{onClick}` and `value` should be passed in as `{value}` (squares can be passed in as a regular string since it is not a JS variable).
        * JSX is a React-specific syntax extension that combines markup syntax (like HTML) with JavaScript code syntax. If you would like to know more, visit the official React documentation [here](https://reactjs.org/docs/introducing-jsx.html).

Great job so far! If you want to render your progress at this point, feel free to do so by adding a `Square` component in the return block of `components/Game.js`. To do this, you’ll have to import the `Square` component within that file via `import Square from './Square'`. After assigning an arbitrary string to value (like 'X' or 'O') and an arbitrary function to `onClick`, add the following:

```html
<Square value={square} onClick={() => onClick()} />
```

<img src='./assets/starter/progress_single_x.png' alt='A screen with a single X at the center top.' width='500em'>

## Setting Up the Board

Now that we have the ability to render squares via the `Square` component, our next step is to create a new `Board` component that will lay out our squares in a 3x3 tic-tac-toe grid.

* To simplify things, we have added a `board` class in `index.css` that will nicely format the tic-tac-toe squares in a grid format. Feel free to play around with this if you’re interested, but you don’t need to change it for the game to work.
    * You may have noticed that we’re using the `fr` unit in `grid-template`. This is a unit that enables us to create elements of equal size, so `repeat(3, 1fr)` creates three elements that each take up a third of the available space.
* Within `components/Board.js`, pass two parameters within your `Square` component, `squares` and `onClick` (sound familiar? We did something similar in `Square.js.`). Assume that `squares` is an array that contains nine square values ordered by their position on the board. `onClick` will be the function that determines what your program should do when a square is clicked.  We will implement this `onClick` function when setting up the `Game` component, but for now, assume that it accepts one parameter, `i`–the index of the square (see figure below).

  <img src='./assets/starter/board_indices.png' alt='A 3-by-3 grid, with each square numbered 0-8 in row-major order.' width='500em'>

* Within the same file, replace the empty tag (``<></>``) with a `div` tag using `board` as a `className` attribute. Within the body of this tag, render an array of `Square` components, each with their own unique key attribute corresponding to their index `i`, in addition to the value and `onClick` attributes. (**Hint**: to create this array, consider using the `map()` method outlined [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).)
    * To get you started, your div should come in the form `<div className=....>...</div>`
    * Your `onClick` attribute should come in the form `{() => onClick(i)}`, rather than `{onClick(i)}`. The main difference between these two forms is that the former is telling the `onClick` attribute to execute `onClick(i)` when the component is clicked, while the latter invokes `onClick(i)` when it is assigned to the attribute; one is treated as a function, the other is treated as an object.

Almost there! Once again, if you would like to visually track your progress at this point, add a `Board` component in the return block of `components/Game.js`.

<img src='./assets/starter/progress_grid.png' alt='Screen with a 3-by-3 grid.' width='500em'>

## Setting up the Game

With our newly created `Board` component, we can render a tic-tac-toe grid equipped with the ability to display values and exhibit certain behavior when one of its squares is clicked. However, we still need to go one level higher and design a `Game` component that is responsible for controlling the board and other components beyond it. Going through this step might take a while, so we’ll split this process into several parts.

### Initializing States

If we’re going to create a functional tic-tac-toe game, how are we going to keep track of all of the values on the board, whether X or O is next, step number, and other key variables? We’ll want to re-render the entire screen whenever a state change occurs, which is exactly why we’re using `state` variables here over normal instance variables.

* In Hooks, setting up state is easy; we’ll need to create the variables themselves along with their corresponding setters. For helpful documentation, check out [this introduction to Hooks](https://reactjs.org/docs/hooks-intro.html).
    * The first state we’ll want to keep track of is `board`, which is an array of size 9 that tracks the value of each square ('X', 'O', or null/empty string). Create `board` along with its setter `setBoard`, where `board` is initially an array of 9 null values.
    * Next, we’ll want to keep track of `stepNumber`, which is a zero-indexed value indicating how many turns have been taken before the current turn. Create `stepNumber` along with its setter `setStepNumber`, where `stepNumber` is initially 0.
    * Finally, for our last state variable, we’ll create `xIsNext`, which is a boolean that will determine whether it is X’s turn or not.  Create `xIsNext` along with its setter `setXisNext`, where `xIsNext` is initially true.

### Initializing Other Variables

Although state is important, normal JavaScript variables still have a place within our program. These variables are computed every time our screen is re-rendered (that is, when the state changes).

* We’ll be creating two variables that are part of this section, one of which will be the result of an imported function. As part of this task, you will be writing this function as well.
    * First, create a variable that calculates the current winner of the board by passing in the return value from `calculateWinner()`. The winner can be X, O, or null (if there is no current winner).
        * We’ll leave the details of implementing `calculateWinner()` up to you.
    * Next, create a variable that either holds the string 'X' or 'O' depending on whether X is next or not. This is the string that will be passed into the board when the current player chooses a square.

### Creating Helper Functions

As part of our program’s functionality, we need to have functions that enable us to click on a specific square, return to the start of a new game, and display the current game result. In this section, we will go over a few helpful pointers that you’ll need to write these functions. As far as styling goes, we recommend that you use arrow functions (more information [here](https://www.javascripttutorial.net/es6/javascript-arrow-function/)).

* Create a function called `handleClick` with one parameter–the index of the square that is being clicked. This function should prevent an action from being taken if a winner has already been determined and/or that square has been taken. Otherwise, make sure that the right value is passed into the chosen square and change the player from X to O (or vice versa).
    * As you might be guessing at this point, `handleClick` is the `onClick` function that will be passed into our `Board` component (more on that later).
* Create a function called `jumpToStart` with no parameters; this should reset the board and bring the game back to the starting state.
* Finally, create a function called `result` which will return a different string depending on the current state.
    * If a winner is found, return the string `Winner: <winner>` where `<winner>` is either X or O.
    * If there is a tie game, return the string `Tie Game`.
    * Otherwise, return the string `Next Player: <player>` where `<player>` is either X or O.

### Rendering the Components

* Now that we have all of our variables and functions set up, let’s go ahead and render a working version of our program that incorporates everything we’ve done (we’ll let you fill in the blanks):

  ```html
  <>
      <h1>Tic Tac Toe</h1>
      <Board squares={...} onClick={...} />
      <div className='info-wrapper'>
          <div>
            <button onClick={...}>Go to Start</button>
          </div>
          <h3>{...}</h3>
      </div>
  </>
  ```
* I created a new `info-wrapper` class in order to render the `<div>` and `<h3>` elements with maximum space between the two (see note below), in addition to doing some other CSS formatting with the buttons and text.
    * **Note**: To layout your elements in an easy and dynamic way, consider using Flexbox. It is a powerful CSS layout tool that enables you to position and format your elements without explicitly defining their positioning. In the `info-wrapper` class above, we used space-between to create a gap between the `<div>` and `<h3>` elements.  You can also find an example of Flexbox being used in the starter code.
* Now that you have rendered the components, you should see a functional tic-tac-toe game on the page. For guidance on how this should look, refer to the GIF below.

<img src='./assets/starter/progress_functional_game.gif' alt='A tic-tac-toe game being played.' width='500em'>

## Conclusion

We hope you enjoyed this project guide! If you’ve come this far, then you’re more than ready to take on important non-profit work here at Code the Change. While it is almost impossible to categorize all of the fields of expertise needed to create successful software, we have found that most work (especially front-end development) falls into two buckets–design and functionality. We have offered two short bonus guides below so that you can explore each of these tracks further, and it is recommended that you try out at least one of the tracks that interests you. Nonetheless, if you’ve done everything up to this point then you have officially completed the project. Congratulations! 🎉

# UI Design Track

Welcome to the design track! Given how important design is to user experience and engineering quality, we’d like to show you a few ways to quickly improve the perceived quality of your product. In this section, we will focus primarily on `index.css` for the appearance of our tic-tac-toe game, but design is far more comprehensive than this. If you’re interested, be sure to read up on [this blog](https://djangostars.com/blog/ui-ux-terms-everyone-should-know/) to get a more in-depth view on what design is like.

* Our first step will be to round the corners of our elements a bit. To do so, go to `index.css` and set the `border-radius` of the board to `1rem` and each of the squares to `0.5rem`. You might also want to consider setting the `border-radius` of the “Go to Start” button to be around `1.25rem`. For more examples of `border-radius` being used, check out the [official Mozilla Developer Network web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/border-radius).
* Next, we are going to change the background and foreground colors to fit a more “Material Design” look. For every element in the foreground (text labels, buttons, squares, etc.) I used a light color like white (#FFFFFF) to complement the background’s dark blue color (#3171D6).
    * CSS already stores `white` as a variable, but in order to use a custom hex color like #3171D6, it might make things easier to create a new CSS variable if you use it in multiple places. To do so, try putting the following at the top of `index.css`:
      ```css
      :root {
        --style-color: #3171d6;
      }
      ```
    * From here, you can change the colors of each of the necessary elements (site background, square background, and button text) by setting the `color` or `background-color` to be `var(--style-color)`.
    * Similarly, you can change the color of the board/square borders, button color, and text to be white by using the variable `white`.
        * To set the default text color within this webpage, place `color: white` in the `body` selector.
* Lastly, we will add a few finishing touches by providing feedback to the user when a button is hovered or a valid square is clicked.
    * To show that the “Go to Start” `info-wrapper` button is being hovered, we should change the button’s color from white to a light gray (#BFBFBF). To do so, use the following CSS:
      ```css
      .info-wrapper button:hover {
        background: #bfbfbf;
      }
      ```
    * The next step (but a trickier one) is to add a fade-in animation to a square when an ‘X’ or an ‘O’ is being placed. The easiest way to do this in CSS is to use the @keyframes rule. Since we want to add a fade-in animation, slowly bringing the element from 0% to 100% opacity, consider using the following CSS:
      ```css
      @keyframes fadeInOpacity {
          0% {
              opacity: 0;
          }
          100% {
              opacity: 1;
          }
      }
      ```
        * First, you should create a separate class for the ‘X’ and ‘O’ text if you haven’t done so already. Then, if one of the squares is validly selected, you can pass it into the `className` attribute of the button along with `squares` within `components/Square.js`. Then, you can implement the following for that class:
          ```css
          .class {
              opacity: 1;
              animation-name: fadeInOpacity;
              animation-iteration-count: 1;
              animation-timing-function: ease-in;
              animation-duration: 0.5s;
          }
          ```

<img alt='White tic-tac-toe game on blue background being played.' src='./assets/starter/progress_ui_track_complete.gif' width='500em'>

Great job! If you’d like, feel free to play around more with the design aspect of this project. As mentioned previously, there are several other considerations when designing software, including aspects like dark mode support, accessibility, or mobile responsive design!

# UI Functionality Track

Welcome to the functionality track! The term ‘functionality’ can mean a lot of things, but we will define it to be the complement of design. In other words, we will focus on the internal software *implementation* of design, which is known to be the more ‘algorithmic’ aspect of software engineering. Within this track, we will implement a history stack of our tic-tac-toe game in `components/Game.js` so that we can go back to previous moves and board positions.

* To start, we should replace the state that maintains the board positions, board, with a state that maintains history, a history of the board positions. In other words, our array of board positions will now be an array of arrays of board positions. Create `history` along with its setter `setHistory`, where `history` is initially an array containing one element: an array of 9 null values.
* Notice that `calculateWinner` takes the entire board as input. However, now that we have replaced `board` with `history`, you should make sure to calculate the winner of the game by passing the current board as a parameter to `calculateWinner`. (**Hint**: At any point where `board` was used in this file, you’ll be able to replace it with `history[stepNumber]`. You won’t need to edit the `calculateWinner` function to make this work.)
* Next, edit the `handleClick` function so that it also changes `history` instead of `board`. In addition to `history`, you will also have to add code that alters `stepNumber` as well.
* Now that the user can jump to the board at any point in time (not just the start), consider changing the `jumpToStart()` function to `jumpTo(step)`, where `step` is a number between 0 and 8 that determines which previous turn the user would like to go back to. Here, you will have to alter the `stepNumber` and `xIsNext` states to ensure that the board state being returned to is accurate.
* At this point, you should update the `result` function so that it passes in `history[stepNumber]` instead of `board`. No other changes should be needed in this function.
* Finally, we should create a function `renderMoves` that populates the page with all of the valid `history` buttons. For instance, if a user chooses to click 'Go to Move #3', then `renderMoves` should render the buttons 'Go to Start', 'Go to Move #1', 'Go to Move #2', and 'Go to Move #3', such that each button jumps to the corresponding point in history. Other than knowing that this function should return a list of buttons, the details of this implementation is up to you. (Consider using the `map()` method on your `history` state.)
* By the end, your return function (based on our implementation) will only need to replace the `<button onClick={...}>Go to Start</button>` part with the following:
  ```html
  <h3>History</h3>
  {renderMoves()}
  ```

Nice work! We hope you enjoyed implementing this advanced tic-tac-toe functionality. If you’re even more keen on improving your game, consider adding in features like computer AI, past game history, or quantum tic-tac-toe!

<img alt='A tic-tac-toe game being played that tracks history.' src='./assets/starter/progress_func_track_complete.gif' width='500em'>

# Deploying Your Code

**Warning:** These instructions are a work in progress, but hopefully they point you in the right direction.

## Upload to GitHub

If you haven't already, create a GitHub account. Then create a new, public repository for your project code and upload your code by running:

```console
git add -A
git commit -m 'completed project'
git push <your repository URL ending in .git> master:master
```

Having your code on GitHub will make it easy for you to share your work.

## Deploy to Stanford AFS

As Stanford students, you have access to [AFS server space](https://uit.stanford.edu/service/afs). Try logging in over SSH to the `cardinal` machines:

```console
$ ssh <your sunet>@cardinal.stanford.edu
The authenticity of host 'cardinal.stanford.edu (<yours might differ>)' can't be established.
ED25519 key fingerprint is SHA256:0NpYmboT5OLGlIzXIoE0/Gp6Wkg6CG6NwibnXk9lrBA.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'cardinal.stanford.edu,171.67.24.75' (ED25519) to the list of known hosts.
<your sunet>@cardinal.stanford.edu's password: <type your password here. nothing will show up>
Duo two-factor login for <your sunet>

Enter a passcode or select one of the following options:

 1. <depends on your account>
 <...>

Success. Logging you in...
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.3.0-62-generic x86_64)

Stanford Research Computing Center (https://srcc.stanford.edu) -------

<...>

<your sunet>@cardinal<machine number>:~$
```

If you look in your current directory by running `ls`, you should see a `WWW` folder. Anything you put in that folder will be visible on the web at `https://stanford.edu/~<your sunet>`. Go ahead and try by creating a test file there:

```console
echo 'test' > WWW/index.html
```

Now at `https://stanford.edu/~<your sunet>` you should see a page that says `test`!

Now we need to upload your game to your WWW folder. So that the build files resolve to the right URL, add a `homepage` field to `package.json`
with the value `https://stanford.edu/~<your sunet>/`.

Next, build the site:

```console
yarn build
```

This generates the site files in `./build`. We can upload them to Stanford AFS like this:

```console
scp -r build/* <your sunet>@cardinal.stanford.edu:~/WWW/
```
        "/>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '../theme.scss';

.container-fluid {
  padding: 0;
}

.intro {
  padding: 50px 50px 50px 250px;
  @media only screen and (max-width: 700px) {
    padding: 20px;
  }
}

#white {
    background-color: #ffffff;
    display: flex;
    justify-content: center;
}

#content {
    padding: 60px;
    max-width: 900px;
    color: #5e5e5e;
    font-family: 'Comfortaa';
    font-size: 1em;
    font-style: normal;
    font-stretch: normal;
}

#hook{
  font-size: 5em;
}

.hook-sub{
  font-size: 1.2em;
  padding-top: 2.5vh;
}
</style>
