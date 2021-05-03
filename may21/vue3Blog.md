<h1 style="color: rgb(100, 100, 100); font-weight: 500;">Introduction</h1>

The goal was to add a markdown previewer to the `NewBlog` component so I could see the output as I write the post. The markdown to be sanitized on the way in before the post document is added to the Firestore database. Then to pull the document in and display the output on the `SinglePost` page.

I opted to go with **marked** as they are directly endorsed in the Vue docs.

<h1 style="color: rgb(100, 100, 100); font-weight: 500;">Set-Up</h1>

The next issue was working out how to incorporate Marked into the Vue 3 composition api as I could only find examples using the options api. I got there and this is how it all went together:

First add **marked** to the app: `npm install marked`

Then add **Highlight** for code snippet highlighting: `npm install highlight.js`

Add sanitizer: `npm install dompurify`

And that is everything needed to set up the markdown previewer. 

<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Code</h1>

With a focus on keeping things super simple, I only needed to parse one input field so decided to import and use the above directly in the `NewBlog` and `SinglePost` components. The below snippet shows the imports for the above installs and `computed` is imported from Vue:

```js
import marked from 'marked'
import hljs from 'highlight.js';
import 'highlight.js/styles/a11y-light.css';
import { computed } from '@vue/runtime-core'
```

Then this is the computed property that does the magic within the `NewBlog` component. The `DOMPurify.sanitize` function takes in the `marked` function which for the first argument takes in the input to be parsed and the second input is a callback which takes in the markdown adds code highlighting then returns the output, which then gets sanitized and returned as the computed property: 

```js
const markdown = computed(() => {
          return DOMPurify.sanitize(marked(mainOne.value, {
            highlight(md){
              return hljs.highlightAuto(md).value
            }
          }));
        })
```

The markdown is sanitised on the way in before being added to Firestore so the above snippet is from the `NewBlog` component. 

I have this code less the sanitizer also in the `SinglePost` component that pulls the data in from Firestore. I do not know at this stage if this is the best practice, but it all works as it should......except I am not getting any background color for code snippet or block quotes. I feel this is more to do with either styling or plugins. The investigation goes on... 

