# 03 - Reminder manager

## Goal

Build a reminder app where users can add, reorder (based on priority), and delete reminders.

You can find a video of me doing this assignment here: [https://youtu.be/rIlb-PXBjAI](https://youtu.be/rIlb-PXBjAI)

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
@import url('https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap');

/* Reset some general styling for the page */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Use the Inter font */
body {
  font-family: 'Inter', sans-serif;
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

Save your component file. Then render your component in `App` to see that it's working.

## Part 3 - Build Interactivity

In this part we will build interactivity for our app to manage components that are already in the list.

Each reminder will contain the following data fields:

- `id` — a unique identifier for the reminder
- `text` — the reminder content
- `priority` — a numeric value indicating importance (higher = more important)

### Part 3.1 - Update ReminderItem component

In `ReminderItem`, declare a new property to accept reminder data. We also define the emits we'll be using for the different actions.

```js
const props = defineProps({
  reminder: Object,
});

const emit = defineEmits(['increase', 'decrease', 'remove']);
```

Then update the template to show the `reminder.text`. We also add the click event using `v-on` or it's shortcut `@` as well as the emit function.

```html
<article>
  <p>{{ reminder.text }}</p>
  <footer>
    <button @click="emit('increase')">⬆️</button>
    <button @click="emit('decrease')">⬇️</button>
    <button @click="emit('remove')">❌</button>
  </footer>
</article>
```

Test this new version of the component by updating our `App` to include a sample reminder.

```vue
<script setup>
import ReminderItem from './components/ReminderItem.vue';

const r = {
  id: 1,
  text: 'Buy milk',
  priority: 0,
};
</script>

<template>
  <h1>Reminders</h1>
  <ReminderItem :reminder="r" />
</template>
```

Once you have tested your new component, delete the constant r as well as the call to that sample data.

**Q1**: _Why do we pass the reminder as a prop instead of hardcoding the text inside the component?_

### Part 3.2 - Loading and sorting reminders

In this part we will focus on loading and sorting a set list of reminders. Create a reactive array of reminders:

```js
import { ref } from 'vue';

const reminders = ref([
  { id: 1, text: 'Buy milk', priority: 2 },
  { id: 2, text: 'Call sister', priority: 1 },
  { id: 3, text: 'Make cheesecake', priority: 3 },
]);
```

We can also create a sorted version of these reminders using the priority. Import the `computed` function from Vue and created a computed property called `sortedReminders`. Your script block will look something like this:

```js
import { ref, computed } from 'vue';

const reminders = ref([
  { id: 1, text: 'Buy milk', priority: 2 },
  { id: 2, text: 'Call sister', priority: 1 },
  { id: 3, text: 'Make cheesecake', priority: 3 },
]);

const sortedReminders = computed(() => [...reminders.value].sort((a, b) => b.priority - a.priority));
```

Now we can render our sorted reminders using `v-for`. In the template block of App, add the following:

```html
<ReminderItem v-for="reminder in sortedReminders" :key="reminder.id" :reminder="reminder" />
```

**Q2**: _We currently sort reminders from highest priority to lowest. Where might it make sense to sort reminders lowest-first instead of highest-first?_

**Q3**: _What does the sortedReminders variable look like if we sort lowest-first?_

### Part 3.3 - Changing Reminder Priority

Earlier we already created the emits for when we increase or decrease the priority of a reminder. They are bound to the up and down arrows of the `ReminderItem` component. Now we work on listening for these emits in the `App` component and executing the appropriate methods.

First let's define two methods: `increasePriority()` and `decreasePriority()`.

```js
function increasePriority(id) {
  const r = reminders.value.find(rem => rem.id === id);
  if (r) r.priority++;
}

function decreasePriority(id) {
  const r = reminders.value.find(rem => rem.id === id);
  if (r) r.priority--;
}
```

Then we update the template to execute these methods if the App.vue receives the appropriate emit from a ReminderItem.

```html
<ReminderItem
  v-for="reminder in sortedReminders"
  :key="reminder.id"
  :reminder="reminder"
  @increase="increasePriority(reminder.id)"
  @decrease="decreasePriority(reminder.id)" />
```

**Q4**: _Look at the method `decreasePriority`. It can currently go into negative numbers. Does a negative priority make sense? Update the method so that the value of priority cannot go below 0._

### Part 3.4 - Removing Reminders

Finally we add the functionality to remove reminders that are in the array. Create a new method for removing the appropriate reminder. Then update the template.

Your App.vue file should look like this now:

```vue
<script setup>
import { computed, ref } from 'vue';
import ReminderItem from './components/ReminderItem.vue';

const reminders = ref([
  { id: 1, text: 'Buy milk', priority: 2 },
  { id: 2, text: 'Call sister', priority: 1 },
  { id: 3, text: 'Make cheesecake', priority: 3 },
]);

const sortedReminders = computed(() => [...reminders.value].sort((a, b) => b.priority - a.priority));

function increasePriority(id) {
  const r = reminders.value.find(rem => rem.id === id);
  if (r) r.priority++;
}

function decreasePriority(id) {
  const r = reminders.value.find(rem => rem.id === id);
  if (r) r.priority--;
}

function removeReminder(id) {
  reminders.value = reminders.value.filter(r => r.id !== id);
}
</script>

<template>
  <h1>Reminders</h1>
  <ReminderItem
    v-for="reminder in sortedReminders"
    :key="reminder.id"
    :reminder="reminder"
    @increase="increasePriority(reminder.id)"
    @decrease="decreasePriority(reminder.id)"
    @remove="removeReminder(reminder.id)" />
</template>

<style scoped></style>
```

## Part 4 - Add Reminders

In this part we focus on adding new reminders.

First, create a new `ReminderForm` to handle creating new reminders. Like with ReminderItem, it will communicate with App using emits. The structure for this component is below.

```vue
<script setup></script>

<template>
  <form @submit.prevent="submitReminder">
    <input type="text" placeholder="Enter new reminder" />
    <button type="submit">Add Reminder</button>
  </form>
</template>

<style scoped>
form {
  display: flex;
  gap: 8px;
  justify-content: center;
  margin: 16px auto;
  width: 800px;
}

input {
  flex: 1;
  padding: 8px;
  font-size: 1em;
}

button {
  padding: 8px 16px;
  font-size: 1em;
  cursor: pointer;
}
</style>
```

If you render this component as-is in the App, you will see a text bar along with a button to submit a new reminder.

Now we start adding the functionality to make it work. In your `ReminderForm` component, update the script block:

```js
import { ref } from 'vue';

// we use this reactive variable to keep track of the reminder we want to add.
const newText = ref('');

// communicate with App.vue using an emit message
const emit = defineEmits(['add']);

// Function to handle form submission
function submitReminder() {
  if (newText.value.trim() === '') return; // prevent empty reminders
  emit('add', newText.value); // send the new reminder text to parent
  newText.value = ''; // clear the input
}
```

And the template block:

```html
<form @submit.prevent="submitReminder">
  <input type="text" placeholder="Enter new reminder" v-model="newText" />
  <button type="submit">Add Reminder</button>
</form>
```

In these changes we see a few new things with Vue.

- `emit("add", newText.value);`: `emit()` is sending two things: the name of the event ("add"), as well as the data we want to add (newText.value).
- `@submit.prevent="submitReminder"`: Vue listens to when you submit the form (`@submit` is short for `v-on:submit`), prevent the default browser reload (`.prevent`) and instead it will execute the method `submitReminder`.
- `v-model="newText"`: Vue binds the text input to the reactive variable `newText`. This means that when the user types in the input, `newText` automatically updates. Also, if you change newText in the script, the input box updates automatically.

_**SIDE NOTE**_ `v-model` can do a lot more than just connect input to a variable. You can read more about it in the official documentation in [english](https://vuejs.org/guide/components/v-model) or [french](https://fr.vuejs.org/guide/components/v-model).

Now we return to `App.vue` to listen to the new emit and add the reminder to the list.

Create a method for adding a reminder to the list.

```js
function addReminder(reminderText) {
  // Find the largest existing id
  let maxId = 0; // default if there are no reminders

  if (reminders.value.length > 0) {
    // Extract all the ids into a new array
    const allIds = reminders.value.map(reminder => reminder.id);

    // Find the largest id in the array
    maxId = Math.max(...allIds);
  }

  const newReminder = {
    id: maxId + 1,
    text: reminderText,
    priority: 0,
  };
  reminders.value.push(newReminder);
}
```

Here, for `latestId` we do a shorthand of the following:

```js
if (reminders.value.length === 0) {
  return null;
} else {
  return reminders.value[reminders.value.length - 1].id;
}
```

Finally, update how you render `ReminderForm` to execute this method when the `add` event is triggered via the emit.

```html
<ReminderForm @add="addReminder" />
```

Save everything and test things out by adding a few reminders. However if you refresh, you will notice that the added reminders have disappeared. We'll tackle that later.

**Q5**: _Why do we handle adding reminders in the parent instead of inside the `ReminderForm`? Think about why we use emits and props._

---

## Part 5 - Conditional Formatting

We can make the app more visually useful by highlighting certain reminders. Let's focus on two right now:

1. The most recently added reminder
2. The reminder with the highest priority.

We can achieve this by passing a couple of extra props from App.vue to each `ReminderItem`.

In our app, all data is centralized in the `App.vue`. Before we can style any specific reminder, we have to identify which reminders we want to style. In the script block, let's define the new properties we need to send to each `ReminderItem`

We previously saw how to get the latestReminderId when we add a new reminder in Part 4.

```js
const latestId = computed(() => {
  if (!reminders.value.length) return null;
  // Find the maximum id in the current reminders array
  return Math.max(...reminders.value.map(r => r.id));
});
```

To get the reminder with the highest priority, we first find the value of the highest priority, and then find which reminder matches that priority. The following function does that.

```js
const highestPriorityId = computed(() => {
  if (!reminders.value.length) return null;
  const maxPriority = Math.max(...reminders.value.map(r => r.priority));
  const highest = reminders.value.find(r => r.priority === maxPriority);
  return highest ? highest.id : null;
});
```

But! If you have two reminders with the same priority that is the highest, it will pick the one with the smaller ID. That's not great. Instead, we can just find the highest priority in the reminders and pass that to the `ReminderItem`s.

```js
const highestPriority = computed(() => {
  if (!reminders.value.length) return null;
  return Math.max(...reminders.value.map(r => r.priority));
});
```

Now that we have these two values, we can pass them to the `ReminderItem` component.

```html
<ReminderItem
  v-for="reminder in sortedReminders"
  :key="reminder.id"
  :reminder="reminder"
  :latest-id="latestId"
  :highest-priority="highestPriority"
  @increase="increasePriority(reminder.id)"
  @decrease="decreasePriority(reminder.id)"
  @remove="removeReminder(reminder.id)" />
```

While we are sending this information to `ReminderItem`, the component is not yet able to use it. We need to update the component.

In `ReminderItem`, update the props in the script block to include `latestId` and `highestPriority`. We set them to Number because we are importing numerical values.

```js
const props = defineProps({
  reminder: Object,
  latestId: Number,
  highestPriority: Number,
});
```

Then we update the template to conditionally add new classes.

```html
<article
  :class="{
      'latest-reminder': reminder.id === latestId,
      'top-priority': reminder.priority === highestPriority,
    }">
  <p>{{ reminder.text }}</p>
  <footer>
    <button @click="emit('increase')">⬆️</button>
    <button @click="emit('decrease')">⬇️</button>
    <button @click="emit('remove')">❌</button>
  </footer>
</article>
```

And add the new CSS:

```css
article.top-priority {
  border-color: gold;
  background-color: #fffbe6;
  box-shadow: 0 0 8px rgba(255, 215, 0, 0.4);
}

article.latest-reminder {
  border-style: dashed;
  border-color: #1e90ff;
}

/* Do something special if the newest reminder is also the most important */
article.latest-reminder.top-priority {
  border-style: dashed;
  border-color: #28a745;
  background-color: #e9f9ee;
  box-shadow: 0 0 10px rgba(40, 167, 69, 0.4);
}
```

## Part 6 - Persistent Storage

Right now, if you refresh the page, all the reminders you’ve added will disappear. In this part, we’ll make the reminders persist using the browser’s `localStorage` (some [documentation](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)). This is a simple way to store data so it’s still available after a page reload. To make this work, we will be focusing on `App.vue`.

First, clear the reminders array we had from before. This will let us start fresh during the session.

```js
const reminders = ref([]);
```

Then we check if we have any already saved reminders in `localStorage`. Add this right after reminders and before any of the methods.

```js
const saved = localStorage.getItem('reminders');
if (saved) {
  reminders.value = JSON.parse(saved);
}
```

Finally, we import `watch()` from Vue, and then use it to save any changes to reminders to localStorage.

```js
import { computed, ref, watch } from 'vue';

watch(
  reminders,
  newReminders => {
    localStorage.setItem('reminders', JSON.stringify(newReminders));
  },
  { deep: true }, // ensures changes inside objects like priority are detected
);
```

Since Vue works reactively, nothing else needs to change. Save your file and try it out.

## Part 7 - One last fix

In this app, it's currently possible to create a reminder that has an `id` of some number, delete it, and then create a new reminder with the same `id` number. **Update how the app creates new IDs and how it keeps track of the latest ID so that this is no longer possible. You may have to create a new variable to keep track of the latest ID and use new or different props to make sure conditionally styling works.**
