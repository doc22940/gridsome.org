---
title: MDVue - Vue Components in Markdown
slug: gridsome-v06
author: [tommyvedvik]
date: 2019-05-21
excerpt: "Introducing MDVue for Gridsome. A plugin that let you use Vue Components inside Markdown posts, and build pages with Markdown. Perfect for building Documentation, Design Systems, Portfolios, Blogs etc."
---

![MDVue](./mdvue.png)

Using Markdown for content is great. It let you have all your content and code in same repository. With MDVue you can now import Vue Components and other Markdown files (!!) inside your Markdown files. This is perfect for adding extra functionality to your Markdown content. You can also use MDVue + Markdown to build all your pages in `src/pages` directory. You even get **code-splitting** at build time for all Markdown pages.


## How to use

1. Install `@gridsome/plugin-mdvue` with Yarn or NPM to your Gridsome project.
2. Add to `gridsome.config.js`

```js
module.exports = {
  plugins: [
    {
      use: '@gridsome/plugin-mdvue',
      options: {
        baseDir: './content/docs', // Where files are located
        pathPrefix: '/docs', // Route prefix
        layout: './src/layouts/Doc.vue' // What layout to use
        remark: {
          // optional remark options and plugins
        }
      }
    }
  ]
}
```

💥 **@gridsome/plugin-mdvue** is designed to be as a drop-in replacement for **@gridsome-filesystem** + markdown. If you're already using **filesystem-source** you can just change out the **use** and add **layout** and **pathPrefix** options.

In this example any `.md` file in `./content/docs` will be a page.

- `/content/docs/index.md` will be  **/docs**
- `/content/docs/how-to-use.md` will be **/docs/how-to-use**
- `/content/docs/folder/index.md` will be **/docs/folder**


## Example Markdown file

```jsx
---
title: A cool title
excerpt: Lorem Ipsum is simply dummy text.
---

# {{ $frontmatter.title }}
> {{ $frontmatter.excerpt }}

Add **Markdown content** here...

// Import Vue components
import Youtube from '~/components/Youtube.vue'

<Youtube id="xyxyxy" />

... Add some more Markdown content here.

```

> All **front-matter** fields are available in `$frontmatter` and in the **GraphQL data layer** if you want to query data from other pages.


### Use Markdown files as components.

**MDVue** let you use other `.md` files as Vue Components!

```jsx
---
title: A cool title
excerpt: Lorem Ipsum is simply dummy text.
---

import IntroBox from '~/components/IntroBox.md'

<IntroBox :text="$frontmatter.excerpt" />

```


### Use any components that are registered globally.

Register components globally in `src/main.js` if you don't want to import them for every page.
Learn how to [make a component global](/docs/components#make-a-component-global).


```jsx
---
title: A cool title
---

<GlobalComponent> Global component </GlobalComponent>

```

### Use all the power of Vue Components in Markdown
You can add GraphQL queries, script and style tags.

```jsx
---
title: A cool title
---

Some _markdown_ content...

<style lang="scss">
  .some-style {border: red}
</style>

<page-query>
  ...
</page-query>

<script>
  ...
</script>

```

## What about MDX for Vue?
MDX will have Vue.js support (https://mdxjs.com/guides/vue), but the problem is that it's still JSX. Under the hood **MDX** converts `.mdx` files into `JSX`, but **MDVue**  simply converts `.md` files into `.vue` files. That's better and more efficient for Vue projects. 

The MDX project is still great, so Gridsome will probably have a MDX source plugin at some point also.


## Gotchas
MDVue uses runs all files trough Webpack to render Vue Components. This will slow the build process down compared to for example **filesystem-source**. It's not recommended to use MDVue if you plan to have thousands of blog posts.  MDVue is perfect for documentation, design systems, portfolios and smaller blogs etc.


## Learn more..
- **MDVue** Documentation: https://gridsome.org/plugins/@gridsome/plugin-mdvue
- Learn more about Markdown https://www.markdownguide.org