# 01 - Personal Page (Updated: 2026-02-13)

## Goal

Build a simple personal website using Vue components, starting with plain HTML and progressively adding reactivity. As you work and test your web app, make sure you save your files often to see the changes in the browser.

You can find a video of me doing this assignment here: [https://youtu.be/Em_oKxqmyHU](https://youtu.be/Em_oKxqmyHU)

---

## Part 1 — Project Setup

- Create a new Vue project named **personal-page-YOURNAME**

  ```bash
  npm create vue@latest
  ```

- Run the project and confirm the web app loads correctly in the browser

**Q2.1:** _What command do we use to run the project locally?_

---

## Part 2 - Page Structure - Header

- Create a component called `Header.vue`. You will likely have to create a `components` folder in your `src` folder before creating the component.
- Add the basic blocks of a component:

  ```html
  <script setup></script>

  <template> </template>

  <style scoped></style>
  ```

**Q2.2:** _Where does HTML go in a SFC: `script`, `template`, or `style`?_

**Q2.3:** _What kind of code goes in the other two blocks of a SFC file?_

- Inside your `<template></template>` add a `<header></header>` html element. You can read more about the header element here: [https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements/header](https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements/header)

- Inside the header element, complete the following actions:
  1. Display your name in an `<h1></h1>` heading element.
  1. Display your student email inside an `<a></a>` element. Make the email link using `mailto:` in the `href` attribute. You can read about the anchor element here: [https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a)

- Import and render the Header component in your `App.vue`.

  ```vue
  <script setup>
  import Header from './components/Header.vue';
  </script>

  <template>
    <Header />
  </template>
  ```

---

## Part 3 - Page Structure - Main

- Create a component called `Main.vue`. Populate it with the basic blocks.
- Inside the `template` block add a `<main></main>` element. You can read about the main element here: [https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements/main](https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements/main)
- Inside the main element, complete the following:
  1. Create an image element. Leave the `src` and `alt` attributes blank for now. [More about the img element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img)
  1. Create a `<div></div>`. A div element is used for grouping content on a page. Read more here: [https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/div](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/div)
  1. Inside your div element add a paragraph about yourself. Use the `<p></p>` tag.
  1. Also inside the div element, add one ordered list with five of your favorite songs, and one unordered list with three hobbies you enjoy. You can read the documents for [ordered](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol) and [unordered](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul) lists to figure out the implementation.

- Find and place a photo in the `public/` folder.
- Go back to the `img` element in the template block and update the `src` attribute so the image loads from your public folder.

**Q2.4:** _Where else can we load an image file from in a Vue app?_

- In your `<style scoped></style>` block, add CSS to make your image exactly 300px wide.

  ```vue
  <style scoped>
  img {
    width: 300px;
  }
  </style>
  ```

- Review the Mozilla documentation for the width property ([here](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/width)). Change the width of the image to a different length or percentage value.

- Import and render the Main component in your `App.vue`. Render it under your Header component.

---

## Part 4 - Page Structure - Footer

- Create a component called `Footer.vue`. Populate it with the basic blocks.
- Inside the `template` block add a `<footer></footer>` element. [More info about footer](https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements/footer).
- Inside the footer element add a paragraph tag. Inside include the copyright mark and the year 2024. You can use `&copy;` for the copyright mark/symbol.

- Import and render the Footer component in your `App.vue`. Render it under your Footer component. The final layout should be:
  ```vue
  <Header />
  <Main />
  <Footer />
  ```

---

## Part 5 - Base Styling - Main.css

At this point you have an app that's quite unappealing to look at. Let's work on that.

- Create an `assets` folder. Inside this folder create a `main.css` file. Add the code below. It resets parts of the default CSS and makes sure content is centered.

  ```css
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  div#app {
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 80%;
    margin: 0 auto;
  }
  ```

- At the top of your `main.js` file, import the main.css:
  ```js
  import './assets/main.css';
  ```

---

## Part 6 - Base Styling - Main vs Component CSS

For this part, you'll probably need to look at documentation for CSS: [https://developer.mozilla.org/en-US/docs/Web/CSS](https://developer.mozilla.org/en-US/docs/Web/CSS). You'll work to style your page at both the main and the component levels.

- For the main css level, make the following style changes:
  1. Import and use a Google Font on your page. ([https://fonts.google.com/](https://fonts.google.com/))
  1. Change the background color of the page.

- Find the right component to make these style changes in:
  1. Change the font size of your bio paragraph only.
  1. Change the color of your email link
  1. Give your footer a different background color than the rest of the page.

**Q2.6:** _Why do we have two different places for CSS in our project: the main.css and the SFC style block? What kind of CSS goes in each place?_

- In your Main component add the following to get the photo and the div to show up next to each other:

  ```css
  main {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
  }
  ```

---

## Part 7 — Make things reactive

For this final part you will practice creating various reactive variables. You will have to import the `ref()` function in the components who will get reactive variables.

```js
import { ref } from 'vue';
```

- In the Header component:
  1. Make your name a reactive variable
  1. Make your email address a reactive variable
  1. Make the href value into a reactive variable. Use `v-bind:` or `:`.

- In the Main component:
  1. Convert both of your lists into reactive variables. Render the items using `v-for`.
  1. Move your photo from the public to the assets folder. Import it from the assets folder. You'll have to use `v-bind:` for `src` attribute.

- In the Footer component:
  1. Make the copyright year a reactive variable. Change the value to 2026.

**Q2.7:** _Why does the UI update when data changes?_

---

## Part 8 - Submission

Answer the questions in the `README.md` file for the project. Then compress your folder and submit it on Moodle.
