# 02 - Upvote

## Goal

Create an app where we can vote on posts. The more votes a post has, the higher the post will be on the page. This project is based on one of the exercises in [Fullstack Vue](https://www.newline.co/vue).

You can find a video of me doing this assignment here: **✨TBD✨**

---

## Part 1 - Project Setup

Create a new vue project named: `upvote`. Don't select any of the additional or experimental features, but select `Yes` when asked "Skip all example code and start with a blank Vue project?"

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

We have one post, but we may want more. How would you add multiple posts?

Try adding a new copy of Post component in `App.vue`. It should look like this:

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

Now: Create two copies of the Post component. Call one Post2 and the other Post3. Then change the data for each one so they're three different posts. Import and render Post, Post2, and Post3 in App.vue.

**Q4:** _Does this seem sustainable to do for a lot of posts? Explain why._

---

## Part 5 - Pass data for posts

With vue we can pass data from parent to children components. For our app we care about the following data:

- `id`: a unique id for the post
- `title`: title for our post
- `description`: the description of what the post is about
- `votes`: the number of votes a post has
- `author`: name of person who wrote the post.

For our current Post component, this might look more like:

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

1. Modify the Post component to receive data
1. Add the data to our App.vue (the parent)

### Part 5.1 - Modify Post component

To modify our Post component, we have to make sure we can accept the incoming data properties. Then we modify the template so the data can be viewed.

- In the `script` block, add the following code. It imports the defineProps function that we use to define what properties the component accepts. In this case it's some Object of name post. When we modify our template,

  ```js
  defineProps({
    post: Object,
  });
  ```

- In the template, we have to change the static data into one of the data fields we mentioned before. Change your template to look like this:

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

**Q5:** _Our new Post component doesn't use the `id`. Why do we include it then?_

### Part 5.2 - Modify App.vue

Now we turn to App.vue to pass data to the new version of the Post component.

- In the script block, create a new variable called `post` and give it the sample data that should be in our single post.

  ```js
  const aPost = {
    id: 1,
    title: "A Post named Hi",
    description: "This describes the entire content of the post.",
    votes: 42,
    author: "BoyNamedSue",
  };
  ```

- Pass the data to the Post component.

  ```vue
  <Post :post="aPost" />
  ```

---

## Aside 2 - Revisit Post2 and Post3

Previously you had made two new components Post2 and Post3 that were different from Post.

Now that we have a version of the Post component that is flexible, how can we create new posts using the data you made before?

- Create two new variables, `post2` and `post3` with the data from Post2 and Post3 components respectively. Use these new variables to render two new posts. The end result should look like:

  ```vue
  <Post :post="post2" />
  <Post :post="post3" />
  ```

**Q6:** _This still feels repetitive. What is vue's way of looping through data?_

- Combine the information in `aPost`, `post2`, `post3` into a new array called `posts`. Use `v-for` to create the three posts in one go.

---

## Part 6 - Reactive data

Currently the data being used is not reactive. If a user interaction changed it, Vue wouldn't know and the UI would not be updated properly.

- Import `ref` and use it to turn `aPost` into a reactive variable.

  ```js
  const aPost = ref({...});
  ```

---

## Part 7 - Loading posts

I have a sample of posts. Inside your `/src` folder make a file called `seed.js`. Inside put the following:

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

- In `App.vue` import the file: `import { posts as seedPosts } from './seed';`

- Use these posts to fill out your app.

  ```js
  const posts = ref(seedPosts);
  ```

  ```html
  <Post v-for="post in posts" :key="post.id" :post="post" />
  ```

---

## Part 8 - Enable voting

Now we will try to enable the voting on posts.

- First add CSS to the Post component so the upvote button seems like something we can click:

  ```css
  span.vote {
    cursor: pointer;
  }
  ```

- Attach an event to the button in the template using `v-on:` or `@`.

  ```html
  <span class="vote" @click="">⬆️</span>
  ```

**Q7:** _What do we want to happen when we click on this up arrow?_

### Part 8.1 - Emiting

Because data only flows from parent to child, we cannot call a function that updates the number of votes when we click the button. Instead we have to let the parent component know that someone upvoted a post. We do this with emits.

- In your script block, define an emit using the following block.

  ```js
  const emit = defineEmits(["upvote"]);
  ```

- And from this you can modify your vote element:

  ```html
  <span class="vote" @click="emit('upvote')">⬆️</span>
  ```

Now, when we click on this button, the emit function runs, letting the App component know that something happens.

### Part 8.2 - Updating votes

Now that we can let the App component know something is up, we have to listen for this event and do something about it.

- In App.vue, modify how you call the Post component to do something if it receives the 'upvote' event. We'll use `console.log('hello!')` as a placeholder for what we want to actually happen so we can test that the emitting is working properly.

  ```html
  <Post v-for="post in posts" :key="post.id" :post="post" @upvote="console.log('hello!')" />
  ```

- Open the inspector in your browser, go the console, and try to vote on any post.

- Now we have to create a function that updates the vote for the post that was clicked. Let's call that function `upvotePost()`. It will take the id of the post, find it in the list of posts, and then update the vote count.

  ```js
  function upvotePost(postId) {
    const post = posts.value.find(p => p.id === postId);
    if (post) post.votes++;
  }
  ```

- Finally we make sure to call that function in the upvote event on the component.

  ```html
  <Post v-for="post in posts" :key="post.id" :post="post" @upvote="upvotePost(post.id)" />
  ```

- Save everything and go back to your app, now the right numbers update when you click.

## Part 9 - Sort Posts

Right now, posts are displayed in the order they appear in the posts array. However, we want posts with more votes to appear higher on the page, to let people know what is popular. To do this correctly in Vue, we need to create and update a list of posts. We do that by defining a **computed property**. A computed property lets us derive new data from existing reactive data.

- Import `computed` in `App.vue`:

  ```js
  import {ref, computed} from "vue";
  ```

- Create the new computed property, `sortedPosts`. We use the spread operator (`[...]`) to make a copy of the array so we don’t mutate the original `posts` state.

  ```js
  const sortedPosts = computed(() => {
    return [...posts.value].sort((a, b) => b.votes - a.votes);
  });
  ```

- Update the template to loop over sortedPosts instead of posts.

  ```html
  <Post v-for="post in sortedPosts" :key="post.id" :post="post" @upvote="upvotePost(post.id)" />
  ```

- Save and test the app. When you upvote a post, it should now move higher on the page automatically.

**Q8**: _Why do we use a computed property here instead of sorting the posts directly inside the template or inside the upvote function?_

## Part 10 - Conditional Styling

Now that posts are sorted by votes, we can add some visual feedback based on how many votes a post has. This is called conditional styling.

Let's add two style changes to the posts:

1. highlight posts that have 50 or more votes.
1. minimize posts that have fewer than 5 votes.

- Update the Post component so we can add the classes conditionally. In this case we modify the `article` html

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

- Add the CSS for these two new classes.

  ```css
  article.milestone {
    border-color: gold;
    background-color: #fffbe6;
  }

  article.low {
    opacity: 0.6;
  }
  ```

**Q9:** _What could be some other style changes to include conditionally?_

## Aside 3 - Downvotes and removing posts

So far, users can only increase votes. In many real apps, users can also downvote or remove content entirely. In this aside, you will add both features.

- You will have to update the `Post` component to include new interactive elements (the buttons for downvoting and removing), as well as emit new messages to the `App` component.

- You will have to update your call to the `Post` component in `App.vue` so it can listen to the new emits, and you will have to write new methods to handle downvoting and removing posts.
