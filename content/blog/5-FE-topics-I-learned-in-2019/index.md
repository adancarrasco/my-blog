---
title: 5 Front End topics I learned in 2019
date: "2020-01-01T10:35:03.284Z"
description: "Front End summary about topics learned in 2019"
---

2019 was a year of many changes for me, I moved to another continent, switched jobs and started to work and live fully in English among other things. In the other hand I kept learning about Software Development, that's what I will share in this post.

Over the last decade Front End changed a lot, and nowadays even tough the FE Frameworks are pretty stable there is still some fragmentation on the whole FE Tech Stack. I had the opportunity to play with some FE Technologies, and because of that there's been a lot of learning during this past year. To make the story short, I will try to summarize it in 5 topics, what I consider the highlights of the year.

## Micro FrontEnds

The trend topic in FE Development. Micro Front Ends (MFEs) started as a proposal to prevent having a monolith in the FE side, since the FE layer has been increasing in complexity during the last 5 years there's been the necessity to make it bigger as a whole taking care of performance and trying to make it feel as Rich Application, this means no lag or waits. Having a huge codebase in the FE has made the projects hard to scale, so what MFE proposes is to have isolated your Application typically by domains, this means dividing your application in modules simulating the MicroServices architecture (which is the solution for the BE monolith).

One of the main advantages of MFEs is to have different projects, so you can have different teams, technologies, deployments, versions, etc and even different frameworks/libraries such as Angular, VueJS, ReactJS (mentioning the most popular ones).

If you want to learn more about the topic [this is a good post](https://medium.com/@tomsoderlund/micro-frontends-a-microservice-approach-to-front-end-web-development-f325ebdadc16) explaining what are the current implementations of MFE.

![MicroFrontEnds - https://livebook.manning.com/book/micro-frontends-in-action/chapter-1/v-2/44](https://dpzbhybb2pdcj.cloudfront.net/geers/v-2/Figures/CH01_FIG_06_MFIA_pages_and_fragments.png)

## Design Systems

If you have done Front End for some time you may have faced the fragmentation in design; if you are new to FE you can prevent that fragmentation from happening. Components that look the same but are used slightly different, font families, sizes, boldness changed in different pages, even colors. I have seen that, this can cause some patching in your code making it complex and hard to maintain. This year I heard about a solution to this issue, having a Design System will save your code and will make your code fragmentation harder to happen.

A Design System is like having the rules to play the game, having some Legos to build something, and more important, helps you preventing fragmentation in design and code. If you want to learn more about Design Systems you can check it out [here](https://www.invisionapp.com/inside-design/guide-to-design-systems/).

![Design System at Slack](https://miro.medium.com/max/3000/0*9wjQodozMFLwagx7)

## Atomic Design

Another topic I heard this year is Atomic Design, this could be in some cases part of the framework you use to create your Design System. It basically divides the components in three categories: Atoms, Molecules, Organisms. Advantage is you can create your Pages mixing those three basic elements, at the end your Page turns out to be more maintainable, doing separation of concerns and also making your components generic.

If you want to learn more about this topic you can visit my [post about it](../scaling-atomic-design-with-react).

![Atomic Design by Brad Frost](https://github.com/adancarrasco/my-blog/blob/master/content/assets/5-FE-topics-I-learned-in-2019/atomic-design-process.png)

## CSS in JS

Having and maintaining CSS can be painful, unique ids, class names following BEM, etc. A solution that worked for me in 2019 is to use CSS in JS which is basically styling your components using conventional CSS and relying on a tool to do the unique naming for you. There are some tools you can use to do this. I have done it using EmotionJS which is a library letting you have CSS inside your JS.

You can learn more about it in the [EmotionJS website](https://emotion.sh/docs/introduction). And if you have issues applying media queries in EmotionJS you can read [this post](../media-queries-in-emotion)

![EmotionJS example](https://github.com/adancarrasco/my-blog/blob/master/content/assets/5-FE-topics-I-learned-in-2019/css-in-js.png?raw=true)

## Cypress

Testing has been always important and it's something that has evolved as well. Now that we have deployments more frequent it's easy to break things that's why it's important to have a good testing strategy, FE can be a lot of clicking and typing, having different browsers and different OSs can make this more complex to check on every deployment we do. Hopefully there are solutions to prevent issues or regressions from happening. Selenium was a good technology to use however, in this case Cypress has shown to be a better tool for doing End to End testing or at least to me having working with Selenium + Java in the past seems that Cypress is more elegant and easy to use.

[![Cypress](https://github.com/adancarrasco/my-blog/blob/master/content/assets/5-FE-topics-I-learned-in-2019/cypress-thumb.jpg?raw=true)](https://vimeo.com/237527670 "Cypress")

This is the summary of the topics I consider the core to create a robust Web Application in 2019. Let's see how 2020 changes these topics.

Thanks for reading!
