---
title: Implementing Pub-Sub in JavaScript
date: "2020-02-02T23:05:03.284Z"
description: "How to implement Pub-Sub"
---

With Today's trendy topic Micro Front Ends, a big question is how to communicate different MFEs? This could be easy if you are using the same framework across your different projects, but if you choose different frameworks as: React, Angular or VueJS communicating and sharing the state could be hard. Another use case is if you are implementing partially a new Framework to live with legacy code.

In this post you will learn how to communicate those different sub-projects no matter which number or name of frameworks you are using. There's only one rule: your app should behave as a Single Page Application, which is the most common use case of MFEs, so we should be in good shape.

#### First, let's start by understanding the Design Pattern:

Publish-Subscribe is a design pattern where senders (publishers) can send messages to receivers (subscribers), without the subscribers knowing about what the publishers are doing. And how this can help us with communicating different projects? Easy, we will have publishers from one project and subscribers in other project and viceversa, facilitating the communication and the sharing of resources.

In the following illustration we can see the representation of the Design Pattern, where:

We have a publisher, which can be like the Satellite sending signal (sender), the channels being the different topics to which the subscribers can subscribe to, and the subscribers (receivers) which in the example are TVs that will be receiving the message (stars). If you see it that way you can interpret the Design Pattern and understand with a real case how it works.

[Pub-Sub](https://github.com/adancarrasco/my-blog/blob/master/content/assets/implementing-pub-sub-in-js/pub-sub.png?raw=true)

#### What's the use case?

Let's take the following illustration as the project we are creating. Now, imagine Team Checkout has to send the price and item that is selected when pressing the `Buy for 66,00 €` to Team Inspire (which in this case let's imagine is the Cart in your eCommerce project), the frameworks are different and there's no middleware to communicate them. Can you identify the publisher and the subscriber? Awesome!

![MicroFrontEnds - https://livebook.manning.com/book/micro-frontends-in-action/chapter-1/v-2/44](https://dpzbhybb2pdcj.cloudfront.net/geers/v-2/Figures/CH01_FIG_06_MFIA_pages_and_fragments.png)

#### Coding the Design Pattern

```JavaScript
  // topics should only be modified from the eventRouter itself, any violation to the pattern will reflect misbehave
  window.pubSub = (() => {
    const topics = {}
    const hOP = topics.hasOwnProperty

    return {
      publish: (topic, info) => {
        // No topics
        if(!hOP.call(topics, topic)) return

        // Emit the message to any of the receivers
        topics[topic].forEach(item => {
          // Send any arguments if specified
          item(info !== undefined ? info : {})
        })
      },
      subscribe: (topic, callback) => {
        // Create the array of topics if not initialized yet
        if(!hOP.call(topics, topic)) topics[topic] = []

        // We define the index where this receiver is stored in the topics array
        const index = topics[topic].push(callback) - 1

        // When we subscribe we return an object to later remove the subscription
        return {
          remove: () => {
            delete topics[topic][index]
          },
        }
      },
    }
  })()
```

Preparing our TVs to receive the signal:

```JavaScript
  let subscriber1 = pubSub.subscribe('hello', myArg => console.warn('hello', myArg))
  let subscriber2 = pubSub.subscribe('hello', myArg => console.warn('bye', myArg))
```

Now we have to subscribers that are listening to the same event: `'hello'`, whenever we send a message trough that channel those two receivers will emit the message.

```JavaScript
  // Executing
  pubSub.publish('hello', 'world')

  // Will output
  "hello world"
  "bye world"
```

Let's say we want only subscriber1 to continue communicating the messages from the publisher, we simply do:

```JavaScript
  // This remove the subscription to the channel/topic we subscribed to
  subscriber1.remove()
```

And this is how easily you can communicate different MFEs using Pub-Sub pattern.

This code was first seen in David Walsh's blog, enhanced by the readers and modified for the explanation and use of MFEs in the current project I'm working on.

Thanks for reading!
