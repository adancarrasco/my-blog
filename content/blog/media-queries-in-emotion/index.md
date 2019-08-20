---
title: Simplifying media queries in Emotion
date: "2019-08-19T22:12:03.284Z"
description: "How to use media queries in Emotion in a simple way"
---

Two months ago I started using [Emotion](https://emotion.sh/docs/introduction) and I loved it. It's amazing how it simplifies the creation of CSS in JS, keeping your components isolated in styling, no need to worry about other CSS overriding your styles. However, there was a pain point: **using media queries adds a lot of boilerplate, for each EmotionComponent you need to add breakpoint/media queries specifics**. I was trying to find a way to prevent the `@media` from being duplicated. So I came with the idea of using template literals. Which is an easy way to prevent duplication plus you can configure your breakpoints once and I used them across your project easily.

First let me show you how a Emotion Component looks using media queries:

**Before:**

```js
// React component
const MyComponent = () => (
  <MyComponentWrapper>
    <h3>The title for my dummy text</h3>
    <p>Some dummy text here. And more text to fill this up.</p>
  </MyComponentWrapper>
)

const MyComponentWrapper = styled.div`
  width: 100px;
  padding: 10px;
  border: 1px solid #ebebeb;

  @media (min-width: 567px) {
    width: 200px;
    border-width: 2px;
  }

  @media (min-width: 768px) {
    width: 250px;
    border-color: #ababab;
  }

  @media (min-width: 992px) {
    width: 300px;
  }
`
```

> We follow a convention to design mobile first. With this we don't need to specify things unless they are needed, as the case of the `padding`.

As you can see the in the example there's a lot of duplicated code. An easy approach to prevent duplication is to use [template literals from ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

Let's see how the code would look after refactoring using template literals.

**In progress:**

```js
// Our helper function
const mq = pixels => `@media (min-width: ${pixels})`

// React component
const MyComponent = () => (
  <MyComponentWrapper>
    <h3>The title for my dummy text</h3>
    <p>Some dummy text here. And more text to fill this up.</p>
  </MyComponentWrapper>
)

const MyComponentWrapper = styled.div`
  width: 100px;
  padding: 10px;
  border: 1px solid #ebebeb;

  ${mq("567px")} {
    width: 200px;
    border-width: 2px;
  }

  ${mq("768px")} {
    width: 250px;
    border-color: #ababab;
  }

  ${mq("992px")} {
    width: 300px;
  }
`
```

This is less code to be repeated, however, we haven't get rid of the strings (pixel definitions), we might end-up with a bunch of strings and strings in code means errors due typos and high cost in maintainance.

Let's do some refactor and compare.

**Final approach:**

```js
// Our helper file
const breakpoints = {
  _sm: "567px",
  _md: "768px",
  _lg: "992px",
  _xl: "1200px",
}

const media = pixels => `@media (min-width: ${pixels})`
const { _sm, _md, _lg, _xl } = breakpoints

export const sm = media(_sm)
export const md = media(_md)
export const lg = media(_lg)
export const xl = media(_xl)
```

This code can be easily put in a file and exposed as a helper which will export the whole template so we can use it as simple as: `${sm}`

Let's use it in our React Component...

```js
import { sm, md, lg } from "./breakpoints"

// React component
const MyComponent = () => (
  <MyComponentWrapper>
    <h3>The title for my dummy text</h3>
    <p>Some dummy text here. And more text to fill this up.</p>
  </MyComponentWrapper>
)

const MyComponentWrapper = styled.div`
  width: 100px;
  padding: 10px;
  border: 1px solid #ebebeb;
  ${sm} {
    width: 200px;
    border-width: 2px;
  }
  ${md} {
    width: 250px;
    border-color: #ababab;
  }
  ${lg} {
    width: 300px;
  }
`
```

Now that we don't have duplication and our breakpoints are isolated in a config file it's easier to define our specific styles for each breakpoint and if at some point we decide to change those values for media queries we just need to update them in one file.
