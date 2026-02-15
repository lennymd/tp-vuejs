# 02 - Upvote

## Goal

Create an app where we can vote on posts. The more votes a post has, the higher the post will be on the page. This project is based on one of the exercises in [Fullstack Vue](https://www.newline.co/vue).

You can find a video of me doing this assignment here: **✨TBD✨**

---

## Part 1 - Project Setup

Create a new Vue project named: `upvote`. Don't select any of the additional or experimental features, but select `Yes` when asked "Skip all example code and start with a blank Vue project?"

```bash
npm create vue@latest
```

Run the project and confirm the web app loads correctly in the browser. Close the test version of the project when you see it working.

Then, open the project in your code editor.

- Create an `assets` and a `components` folder inside of `src` folder

- Clean `App.vue`. Remove the existing HTML and add a heading with the title Upvote.

  ```html
  <h1>Upvote v2</h1>
  ```

Now test your app to make sure the new title is visible. Run `npm run dev` in a terminal and keep that terminal open. As you complete the next parts, check back to make sure you see the app developing.

---

## Part 2 - Add Main-level css

In this part we add some general CSS for our app. Create a `main.css` file in the `/assets` folder. Add the following CSS to it:

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
  font-size: 18px;
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

Finally, import your css file in the `main.js`.

```js
import "./assets/main.css";
```

Check out how the app is in the browser. The heading should now be centered and the font should have changed from something serif to now Inter.

---

## Part 3 - Create Post component

The Post is the core of our app. Let's build the most basic version of the component for our app.

Create a component called `Post` for this app. Inside the template block, add the following for our post:

```html
<article class="post">
  <header class="post-header"><strong class="author"> BoyNamedSue </strong></header>
  <section class="post-content">
    <h2 class="title">A Post named Hi</h2>
    <p class="description">This describes the entire content of the post.</p>
  </section>
  <footer class="post-footer">
    <span class="count">42</span>
    <span class="vote">⬆️</span>
  </footer>
</article>
```

Import and render this component in `App.vue`. Save your files and then view the app. You should see all the info we're using for posts: some author (BoyNamedSue), a title for the post (A post named Hi), some text or description for the post (This describes the entire content of the post.), as well as some arbitraty number that is how many votes this post has (42), and an arrow that we would use to upvote the specific post (⬆️).

In the next part we will make it look a little better.

---

## Part 4 - Styling the post Component

Let's add some style to the Post component.

```css
/* Turn the post into a card with a border, some radius, and spacing so it looks neater. */
article {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  max-width: 400px;
  margin: 16px auto;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: stretch;
}

/* Organize the button and the vote count so they're not stuck together */
footer.post-footer {
  display: flex;
  justify-content: space-between;
  gap: 8px;
  font-size: 1.2em;
}
```

Make sure to save your changes and view the app in the browser.

---

## Aside 1 - How could we add more components?

_In these asides you will explore some ideas that are worth considering, but maybe aren't so great. At the end of the aside, I will let you know what code needs to be kept in the app._

We have one post, but we may want more. How would you add multiple posts? Try adding two copies of Post component in `App.vue`. It should look like this:

```html
<template>
  <h1>Upvote v2</h1>
  <Post />
  <Post />
  <Post />
</template>
```

Then complete the following:

**Q1:** _Describe what you see when you copy the Post component several times._
**Q2:** _What are the different things we want to track with our post? Look at the Post component now and see what we can change for each post._
**Q3:** _How could you change the data so we have three different posts?_

Now make two new components `Post2` and `Post3`. They should have all the same script, template, and style code as `Post`, but change the content inside each template so you have three different posts. Then render `Post`, `Post2`, and `Post3` in your `App.vue`. Don't forget to import the components.

```html
<template>
  <h1>Upvote v2</h1>
  <Post />
  <Post2 />
  <Post3 />
</template>
```

**Q4:** _Does this seem sustainable to do for a lot of posts (think 100, or 1000)? Explain why._

### Post-aside cleanup

Before moving on to the next part, clean up your App. Remove `Post2` and `Post3` from the app. App.vue should look like this at the end:

```vue
<script setup>
import Post from "./components/Post.vue";
</script>

<template>
  <h1>Upvote</h1>
  <Post />
</template>

<style scoped></style>
```

---

## Aside 1.5

_We didn't discuss this in the session, but it's an in-between step that makes the next part make a bit more sense._

With Vue we can make our components more reusable through reactivity and by passing data. We saw this briefly in the previous activity using the `ref()` function. Let's do that again with our `Post` component. Create a new component called `PostReactive`. Then copy and paste everything from `Post` into `PostReactive`. I share it here as well:

```vue
<script setup></script>
<template>
  <article class="post">
    <header class="post-header"><strong class="author"> BoyNamedSue </strong></header>
    <section class="post-content">
      <h2 class="title">A Post named Hi</h2>
      <p class="description">This describes the entire content of the post.</p>
    </section>
    <footer class="post-footer">
      <span class="count">42</span>
      <span class="vote">⬆️</span>
    </footer>
  </article>
</template>
<style scoped>
/* Turn the post into a card with a border, some radius, and spacing so it looks neater. */
article {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  max-width: 400px;
  margin: 16px auto;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: stretch;
}

/* Organize the button and the vote count so they're not stuck together */
footer.post-footer {
  display: flex;
  justify-content: space-between;
  gap: 8px;
  font-size: 1.2em;
}
</style>
```

To make our code reactive, we have to create a variable for each thing we want to keep track of: post author, post title, post description, and post votes. And then use that variable in the tmeplate. That would look like this:

```vue
<script setup>
import {ref} from "vue";

const vote_count = ref(42);
const post_author = ref("BoyNamedSue");
const post_title = ref("A Post named Hi");
const post_description = ref("This describes the entire content of the post.");
</script>
<template>
  <article class="post">
    <header class="post-header">
      <strong class="author"> {{ post_author }} </strong>
    </header>
    <section class="post-content">
      <h2 class="title">{{ post_title }}</h2>
      <p class="description">{{ post_description }}</p>
    </section>
    <footer class="post-footer">
      <span class="count">{{ vote_count }}</span>
      <span class="vote">⬆️</span>
    </footer>
  </article>
</template>
```

If we import and render this component in `App.vue` instead of `Post`, everything looks the same, but if we change a variable, the app will update. As this component is now, we still don't have a great way to make multiples of it without making copies of the component. In the next part we'll use props to pass data into the component every time we render it.

### Post-aside cleanup

Remove the `PostReactive` component, and instead load the `Post` component again:

```vue
<script setup>
import Post from "./components/Post.vue";
</script>

<template>
  <h1>Upvote</h1>
  <Post />
</template>

<style scoped></style>
```

## Part 5 - Pass data for posts

Looking at Aside 1 and Aside 1.5, it doesn't matter how we make the component if we can't make a lot of it. It's a pain to create copies of the same component. With Vue, we can pass data when we render components. We usually pass data as a json object.

For our app we care about the following data:

- `title`: title for our post
- `description`: the description of what the post is about
- `votes`: the number of votes a post has
- `author`: name of person who wrote the post.

When passing data, we should also include an `id` to make sure that each post is unique. Using the info currently in our Post component, the data object looks like:

```json
{
  "id": 1,
  "title": "A Post named Hi",
  "description": "This describes the entire content of the post.",
  "votes": 42,
  "author": "BoyNamedSue"
}
```

To make this work we have to do two things:

1. Modify the Post component to be able to receive the data
2. Send the data from the App (the parent in this setup) to the Post component

We'll address each point in the following subparts.

### 5.1 - Modify Post component

To modify our Post component, we have to make sure we can accept the incoming data properties.Then we modify the template so the data can be viewed.

In the `script` block, add the following code. It imports the `defineProps` function that we use to define what properties the component accepts. In this case it's an Object named `post`. Now, when we want to use the Post component in the App, we have to pass some data into the post.

```js
defineProps({
  post: Object,
});
```

In the template, we have to change the static data into one of the data fields we mentioned before. Change your template to look like this:

```html
<article class="post">
  <header class="post-header"><strong class="author"> {{ post.author }} </strong></header>
  <section class="post-content">
    <h2 class="title">{{ post.title }}</h2>
    <p class="description">{{ post.description }}</p>
  </section>
  <footer class="post-footer">
    <span class="count">{{ post.votes }}</span>
    <span class="vote">⬆️</span>
  </footer>
</article>
```

This looks similar to the code in Aside 1.5, but now we're using items from the `post` object in the template instead of individual variables and we can pass different data objects.

**Q5:** _Our new Post component doesn't use the `id` in the template. Why do we include it in the object then?_

### 5.2 - Modify App.vue

Now we turn to `App.vue` to pass data to the new version of the Post component. In the script block, create a new variable called `aPost` with the sample data that was be in our Post component before.

```js
const aPost = {
  id: 1,
  title: "A Post named Hi",
  description: "This describes the entire content of the post.",
  votes: 42,
  author: "BoyNamedSue",
};
```

To pass the data to the Post component, we need to use the name of the prop (which is short for "property") we defined inside the Post component using `v-bind:` or it's shortcut `:`.

```vue
<Post :post="aPost" />
```

---

## Aside 2 - Practicing with props

Previously we made two components, `Post2` and `Post3` that were copies of the old version of `Post` with different content. In this aside we practice using props using the data from thos posts.

Create two new variables in your `App.vue` called `post2` and `post3` with the Post data you used in the `Post2` and `Post3` components. The variables might look something like this:

```js
const post2 = {
  id: 2,
  title: "Something here",
  description: "Something a little longer",
  votes: 22,
  author: "Someone",
};
const post3 = {
  id: 3,
  title: "Important",
  description: "Still, but less important",
  votes: 12,
  author: "Another Person",
};
```

Then pass them as the post property for two new Post components in `App.vue`. That part of your code would look like:

```vue
<template>
  <h1>Upvote</h1>
  <Post :post="aPost" />
  <Post :post="post2" />
  <Post :post="post3" />
</template>
```

**Q6:** _This is still repetitive. What can we use from Vue to loop through data?_

Now let's combine the information in `aPost`, `post2`, `post3` into a new array called `posts`. Use `v-for` to create the three posts in one go only.

1. Create your posts variable in the `script` block

```js
const posts = [aPost, post2, post3];
```

2. Then change your `template` to only have the following.

```html
<h1>Upvote</h1>
<Post v-for="post in posts" :key="post.id" :post="post" />
```

### Post-aside cleanup

We've already cleaned up things quite a bit in this aside.

---

## Part 6 - Loading posts

In the final stretch of upvote we'll work on making the app reactive. When you upvote a post, the vote count should change. Our first step in this direction is to load a sample of posts so we can see this on a bigger scale than the 3 posts we had made before.

Inside your `/src` folder make a file called `seed.js`. Inside put the following code. It's a group of posts made using ChatGPT.

```js
export const posts = [
  {
    id: 1,
    title: "V4 at the new gym feels sandbagged",
    description: "Anyone else think the setters went a little wild this week?",
    votes: 18,
    author: "Alex Martin",
  },
  {
    id: 2,
    title: "How often do you train finger strength?",
    description: "Trying to balance board work with actual climbing.",
    votes: 12,
    author: "Sophie Nguyen",
  },
  {
    id: 3,
    title: "Tips for committing to dynos",
    description: "I hesitate every time and miss the move.",
    votes: 25,
    author: "Lucas Bernard",
  },
  {
    id: 4,
    title: "Chalk preference: loose or liquid?",
    description: "My gym banned loose chalk and I’m not convinced.",
    votes: 7,
    author: "Emily Carter",
  },
  {
    id: 5,
    title: "Rest days feel harder than training days",
    description: "Anyone else feel guilty not climbing?",
    votes: 15,
    author: "Daniel Lopez",
  },
  {
    id: 6,
    title: "Best shoes for steep bouldering?",
    description: "Looking for something aggressive but still wearable.",
    votes: 22,
    author: "Maya Patel",
  },
  {
    id: 7,
    title: "How long did it take you to send your first V6?",
    description: "Curious what progression looks like for most people.",
    votes: 30,
    author: "Thomas Wilson",
  },
  {
    id: 8,
    title: "Campus board: useful or overrated?",
    description: "Thinking about adding it to my weekly routine.",
    votes: 9,
    author: "Hannah Fischer",
  },
  {
    id: 9,
    title: "Favorite warm-up sequences",
    description: "Trying to avoid tweaky fingers this season.",
    votes: 14,
    author: "Ryan O’Connor",
  },
  {
    id: 10,
    title: "Outdoor bouldering alone?",
    description: "Safe enough or generally a bad idea?",
    votes: 5,
    author: "Laura Kim",
  },
  {
    id: 11,
    title: "Slab problems terrify me",
    description: "Any mental tricks for trusting feet?",
    votes: 19,
    author: "Ben Rossi",
  },
  {
    id: 12,
    title: "Do grades feel inconsistent between gyms?",
    description: "My V5 here feels like a V3 somewhere else.",
    votes: 27,
    author: "Claire Dubois",
  },
  {
    id: 13,
    title: "How often do you resole shoes?",
    description: "Trying to keep my favorites alive longer.",
    votes: 11,
    author: "Julien Moreau",
  },
  {
    id: 14,
    title: "Training endurance for long boulders",
    description: "Pump hits halfway through and ruins attempts.",
    votes: 16,
    author: "Natalie Brooks",
  },
  {
    id: 15,
    title: "Best stretches post-session?",
    description: "What actually helps with recovery?",
    votes: 6,
    author: "Marcus Hill",
  },
  {
    id: 16,
    title: "Recording attempts on video: helpful or not?",
    description: "Not sure if it helps or just adds pressure.",
    votes: 13,
    author: "Isabelle Laurent",
  },
  {
    id: 17,
    title: "How do you approach limit bouldering?",
    description: "More attempts or more rest between goes?",
    votes: 21,
    author: "Kevin Zhang",
  },
  {
    id: 18,
    title: "Skin care routines that actually work",
    description: "Flappers are killing my sessions lately.",
    votes: 17,
    author: "Olivia Reed",
  },
  {
    id: 19,
    title: "Music in headphones while climbing?",
    description: "Focus boost or safety issue?",
    votes: 4,
    author: "Ethan Walker",
  },
  {
    id: 20,
    title: "What’s your proudest send?",
    description: "Grade doesn’t matter — just curious.",
    votes: 29,
    author: "Sarah Klein",
  },
];
```

Then in your `App.vue` import the file in your script block using the following line: `import { posts as seedPosts } from './seed';` We are importing the list of posts into a variable called `seedPosts`.

To use this data we'll modify the variable we made earlier. It should now look like this:

```js
const posts = ref(seedPosts);
```

**Q7**: _What should the ref() function do in the code above? What other code do we need to make it work?_

Because we only changed what goes into the posts variable, we don't need to change the template code. At this point, feel free to remove the previous `aPost`, `post2` and `post3` variables in your script block.

---

## Part 8 - Enable voting

Now we turn our attention to enable upvoting on the posts.

First add CSS to the Post component so the upvote button seems like something we can click:

```css
span.vote {
  cursor: pointer;
}
```

To start, we attach an empty event to the button in the template using `v-on:` or `@`. `<span class="vote" @click="">⬆️</span>`

**Q8:** _What do we want to happen when we click on this up arrow?_

### Part 8.1 - Emiting

Because data only flows from parent to child, we cannot call a function that updates the number of votes when we click the button. Instead we have to let the parent component know that someone upvoted a post. We do this with emits.

In the script block of the `Post` component, define an emit using the following block. This defines one type of message that Post can let it's parent know happened. Later you will add other messages.

```js
const emit = defineEmits(["upvote"]);
```

After, you can modify the click event to be `emit('upvote')`.

```html
<span class="vote" @click="emit('upvote')">⬆️</span>
```

Now, when we click on this button, the emit function runs, letting the App component know that something, an 'upvote', happens.

### Part 8.2 - Updating votes

Now that we can let the App component know something is up, we have to listen for this event and do something about it at the parent level.

In App.vue, modify how you call the Post component to do something if it receives the 'upvote' event. We'll use `console.log('hello!')` as a placeholder for what we want to actually happen so we can test that the emitting is working properly.

```html
<Post v-for="post in posts" :key="post.id" :post="post" @upvote="console.log('hello!')" />
```

Open the inspector in your browser (Right click on the app in the browser and click on 'Inspect' or 'Inspecter'), go the console tab, and try to vote on any post. When you upvote a post, you should a message that says `hello!`. Your `App` component is successfully listening to the `Post` and then printing that message.

Now we have to create a function that updates the vote for the post that was clicked. Let's call that function `upvotePost()`. It will take the id of the post, find it in the list of posts, and then update the vote count.

```js
function upvotePost(postId) {
  const post = posts.value.find(p => p.id === postId);
  if (post) post.votes++;
}
```

Finally we make sure to call that function in the upvote event instead of the `console.log()` we had before.

```html
<Post v-for="post in posts" :key="post.id" :post="post" @upvote="upvotePost(post.id)" />
```

Save everything and go back to your app, now the right numbers update when you click.

## Part 9 - Sort Posts

Right now, posts are displayed in the order they appear in the initial posts array. However, we want posts with more votes to appear higher on the page, to let people know what is popular. To do this correctly in Vue, we need to create and update a list of posts. We do that by defining a **computed property**. A computed property lets us derive new data from existing reactive data.

Import `computed` in `App.vue`. Then, create the new computed property, `sortedPosts`. We use the spread operator (`[...]`) to make a copy of the array so we don’t mutate the original `posts` state.

```js
import {ref, computed} from "vue";

const sortedPosts = computed(() => {
  return [...posts.value].sort((a, b) => b.votes - a.votes);
});
```

Update the template to loop over `sortedPosts` instead of `posts`.

```html
<Post v-for="post in sortedPosts" :key="post.id" :post="post" @upvote="upvotePost(post.id)" />
```

-Save and test the app. When you upvote a post, it should now move higher on the page automatically.

**Q9**: _Why do we use a computed property here instead of sorting the posts directly inside the template or inside the upvote function? What could happen if we directly sorted the data and something went wrong?_

## Part 10 - Conditional Styling

Now that posts are sorted by votes, we can add some visual feedback based on how many votes a post has. This is called conditional styling.

Let's add two style changes to the posts:

1. highlight posts that have 50 or more votes.
1. minimize posts that have fewer than 5 votes.

Update the Post component so we can add the classes conditionally. In this case we modify the `article` html:

```html
<article
  class="post"
  :class="{
  milestone: post.votes >= 50,
  low: post.votes < 5
}"
>
  ... rest of post...
</article>
```

Then add the CSS for these two new classes. If posts has 50 or more likes, the border will be gold, and if there are less than 5 votes on a post, it starts to become invisible.

```css
article.milestone {
  border-color: gold;
  background-color: #fffbe6;
}

article.low {
  opacity: 0.6;
}
```

**Q10:** _What could be some other style changes to include conditionally?_

## Aside 3 - Downvotes and removing posts

For this last part you will add two new buttons to the interface to further modify posts.

So far, users can only increase votes. In many real apps, users can also downvote or remove content entirely. In this aside, you will add both features.

1. Update the `Post` component to include new interactive elements (the buttons for downvoting and removing), as well as emit new messages to the `App` component. You can add more elements to the array inside of `defineEmits()`
1. Update your call to the `Post` component in `App.vue` so it can listen to the new emits, and you will have to write new methods to handle downvoting and removing posts.

Downvoting will be very similar to the upvote function. There are different ways to remove posts from the page. One way below:

```js
function removePost(postId) {
  posts.value = posts.value.filter(post => post.id !== postId);
}
```
