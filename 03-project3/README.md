# 03 - Reminder manager

## Goal

Build a reminder app where users can add, reorder (based on priority), and delete reminders.

You can find a video of me doing this assignment here: **✨TBD✨**

---

## Part 1 — Project Setup

Create a new Vue project named `reminder-manager` and make it a blank project.

Clean the `App.vue` component:

```vue
<script setup></script>

<template>
  <h1>My Reminders</h1>
</template>

<style scoped></style>
```

Then add the following CSS to your project:

```css
/* Add a Google Font called Inter */
@import url("https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap");

/* Reset some general styling for the page */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Use the Inter font */
body {
  font-family: "Inter", sans-serif;
  font-size: 20px;
}

/* Center your app on the page */
div#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 80%;
  margin: 0 auto;
}
```

---

## Part 2 - Create ReminderItem Component

The core of our app is going to be the reminder so let's turn it into a component we can reuse.

Create a component called `ReminderItem`. Use the following template:

```html
<template>
  <article>
    <p>Buy milk.</p>
    <footer>
      <button>⬆️</button>
      <button>⬇️</button>
      <button>❌</button>
    </footer>
  </article>
</template>
```

Then add some CSS for the component:

```css
article {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  width: 400px;
  margin: 16px auto;
  display: flex;
  flex-direction: column;
  gap: 16px;
  align-items: stretch;
}

footer {
  display: flex;
  justify-content: space-between;
  gap: 8px;
}
```
