---
title: Scaling atomic design with React
date: "2019-09-28T10:12:03.284Z"
description: "How to implement atomic design and React in a rapidly growing application"
---

Some time ago I heard for the first time about _Atomic Design_, the name sounded catchy, however I didn't spend too much time on researching about it, months later I joined a Team where was used. Same situation, heard about the concept again, read a little bit and understood the basics.

I'm not going to explain in detail Atomic Design (AD) in this post, however, I recommend you to take a look at this [post](http://atomicdesign.bradfrost.com/chapter-2/). It explains in detail how and why AD is a good approach.

If you read the article mentioned above you should recongnize the following parts of AD, and you can jump to the Design System section, otherwise, here is a summary of AD concept:

### Atoms

The smallest represetnation of something in your project. For example, a custom anchor `<a/>`.

### Molecules

A set of Atoms, for exmaple: A label and an input tag together.

### Organisms

A set of Molecules, for example: A form, which is a set of labels, inputs and buttons

### Templates

A set of organisms, molecules and/or atoms, this is the skeleton of our future page, but only as a skeleton, no data should be used here.

### Pages

The use of a template but with data, if you are using Redux in can be with data coming from the Redux Store, if you are using GraphQL it can be with data coming from your GraphQL, etc.

## Design System

Now that you have a better undersanding of AD, let's see how we can use it to create our Design System (DS). We can see a DS as a set of components/framework that we can combine to generate any pages that we need for our application. For example, [Bootstrap](https://getbootstrap.com/) has a set of components that can be used to create any page. However, we want our DS to be smarter and more scalable than Bootstrap.

Our DS doesn't need to be ready before we start creating our React + Redux application. It can have the basic components and be growing as it needs, of course you need to have all the elements of the page that you want to create. :)

Let's say we are going to create a Twitter App. I'm going to put some samples of each of them so the post doesn't get too big.

Atoms:

- Text
- Button

Molecules:

- Icon + Text
- Set of Buttons

Organisms:

- Tweet (composed by Text, Icon + Text, Image, Link, IconButton)
- Trending List

Template:
Taking as an example the template for the list of posts in a Profile Page in Twitter.

Page:
The Template with data filled.

But, how this looks in code?

Our folder structure will be like:
(This is in our DS so we can re-use it in different projects and keep the same look and feel)

- Atoms
- Molecules
- Organisms

This is our project implementation

- Templates
- Pages

Templates are defined as a set of Atoms, Molecules and Organisms, mailny dumb components, however there are some cases where the Organisms need to have some state (internal state), as _selected_ in the case of a Checkbox set, _shown_ in the case of a Modal, but the state that they handle is not specific for its implementation.
