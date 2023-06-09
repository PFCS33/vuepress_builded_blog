---
title: Markdown Slot 1
date: 2019-2-26
tags: 
  - markdown
  - vuepress
author: ULIVZ
location: Hangzhou  
---

VuePress implements a content distribution API for Markdown. With this feature, you can split your document into multiple fragments to facilitate flexible composition in the layout component.

<!-- more -->

## Why do I need Markdown Slot?

First, let's review the relationship between layout components and markdown files:

<diagram-markdown-slot-relationship/>

Markdown files are providers of metadata (Page content, Configuration, etc.), while layout components consume them. We can use `frontmatter` to define some metadata for common data types, but `frontmatter` is hard to do something about markdown / HTML, a complex metadata that involves differences before and after compilation.

Markdown Slot is to solve this kind of problem.

## Named Slots

You can define a named markdown slot through the following markdown syntax:

``` md
::: slot name

:::
```

Use the `Content` component to use the slot in the layout component:

``` vue
<Content slot-key="name"/>
```

::: tip
Here we are using `slot-key` instead of `slot`, because in Vue, `slot` is a reserved prop name.
:::

## Default Slot Content

By default, the slot-free part of a markdown file becomes the default content of a markdown slot, which you can access directly using the `Content` component:

``` vue
<Content/>
```

## Example

Suppose your layout component is as follows:

``` vue
<template>
  <div class="container">
    <header>
      <Content slot-key="header"/>
    </header>
    <main>
      <Content/>
    </main>
    <footer>
      <Content slot-key="footer"/>
    </footer>
  </div>
</template>
```

If the markdown content of a page is like this:

```md
::: slot header
# Here might be a page title
:::

- A Paragraph
- Another Paragraph

::: slot footer
Here's some contact info
:::
```

Then the rendered HTML of this page will be:

```html
<div class="container">
  <header>
    <div class="content header">
      <h1>Here might be a page title</h1>
    </div>
  </header>
  <main>
    <div class="content default">
      <ul>
        <li>A Paragraph</li>
        <li>Another Paragraph</li>
      </ul>
    </div>
  </main>
  <footer>
    <div class="content footer">
      <p>Here's some contact info</p>
    </div>
  </footer>
</div>
```

Note that:
1. Unlike the slot mechanism provided by [Vue](https://vuejs.org/v2/guide/components-slots.html) itself, each content distribution is wrapped in a `div` whose class is `content` with the name of the slot.
2. Please ensure the uniqueness of the slot defined.
