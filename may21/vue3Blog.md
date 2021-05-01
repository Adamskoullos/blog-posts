## Introduction

I already had a Vue 3 app half built and wanted to add a blog to it. After much research I opted to go with **Marked**. There were others that looked good but Marked is endorsed directly on the Vue website so it made sense.

The goal was to add a markdown previewer to the `NewBlog` page to display the markdown as I write it. Then to save the parsed markdown to Firestore and then to render the content to the `SinglePost` page.

## The Set-Up

The next issue was working out how to incorporate Marked into the Vue 3 composition api as I could only find examples using the options api. I got there and this is how it all went together:

First add **marked** to the app: `npm install marked`

Then add **Highlight** for code snippet highlighting: `npm install highlight.js`

Add sanitizer: `npm install dompurify`

And that is everything needed to set up the markdown previewer. 

## The Code

With a focus on keeping things super simple, I only needed to parse one input field so decided to import and use the above directly in the `NewBlog` and `SingleBlog` components. The below snippet shows the imports for the above installs and `computed` is imported from Vue:

```js
import marked from 'marked'
import hljs from 'highlight.js';
import 'highlight.js/styles/a11y-light.css';
import { computed } from '@vue/runtime-core'
```

Then this is the computed property that does the magic:

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

I have this code less the sanitizer also in the `SinglePost` component that pulls the data in from Firestore. I do not know at this stage if this is the best practice, but it all works as it should......except I am not getting any background color for code snippet or highlighting for block quotes. I feel this is more to do with either styling or plugins. The investigation goes on.. 

