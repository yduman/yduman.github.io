+++
authors = ["Yadullah Duman"]
title = "Preact: Alternative to React"
date = "2019-03-07"
description = "Sharing spike results from a project where we evaluated Preact"
categories = ["React", "External"]
tags = [
    "Preact",
    "JS",
    "Frontend",
    "External"
]
+++

The [original post](https://www.maibornwolff.de/know-how/preact-alternative-zu-react/) is written in German on the MaibornWolff website. 

The following is a plain translation of it.

---

In this blog post, I want to introduce you to [Preact](https://preactjs.com/). About a year ago, I had the opportunity to test it during a spike. As a React fan, it was exciting to try out a good alternative, and I was impressed by its performance. Recently, the new version 10, "Preact X," was released, bringing additional new features.

### What is Preact?

Preact is a fast 3kB alternative to React with the same ES6 API. It is one of the fastest [virtual DOM](https://legacy.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom) libraries currently available. Moreover, it is compatible with most React libraries.

### What makes Preact so special?

The Preact team places a lot of emphasis on performance and efficiency without losing compatibility with React. They achieve this by keeping Preact itself lightweight, optimizing internal algorithms like DOM diffing, and offering additional performance features like [Linked State](https://preactjs.com/guide/v8/linked-state/). Jason Miller, the creator of Preact, talks more about the internals in this [talk](https://www.youtube.com/watch?v=LY6y3HbDVmg).

In the modern web, it is important not to make users wait too long for a page load. If the connection is poor, it ultimately leads to bad UX. Imagine having to wait more than 5 seconds every time you want to visit Twitter. Would you still want to visit Twitter after a while? Probably not.

Such scenarios are crucial for businesses. They often determine whether a company gains or loses customers. This is one of the reasons why [companies](https://preactjs.com/about/we-are-using/) like Uber, Pepsi, or The New York Times use Preact.

### How to get started?

The best way to start is with the Preact CLI.

```bash
npm install -g preact-cli
preact create <template-name> <project-name>
```

The special thing about the CLI is that it achieves a score of 100 out of 100 points in [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) and follows the [PRPL pattern](https://web.dev/articles/apply-instant-loading-with-prpl) which stands for:

- **Push** critical resources for the initial URL route
- **Render** initial route
- **Pre-cache** remaining routes
- **Lazy-load** and create remaining routes on demand

Additionally, the CLI also handles aspects like code splitting, service workers, and SSR.

### The Spike

In a spike, I wanted to test Preact for our client. They were still using [Dojo 1](https://dojotoolkit.org/) for a small frontend application, which is practically obsolete in the frontend world. We had several problems with it:

- Bugs in the framework that had to be fixed by us
- Lacking documentation
- Steep learning curve
- Our code became complex and intimidating for new colleagues over time
- Increased likelihood of bugs due to unstable framework
- Slow CSR

At that time, we recommended React to the client, as it was the standard. Couple of us in the team were proficient in it and we'd be able to onboard the rest relatively quick. For some client-internal reasons, we were asked for an alternative, so we offered them Preact. From our perspective, the advantages were:

- Divide and conquer through reusable components
- Separation of concern through component architecture
- Shallow learning curve due to simplicity of Preact
- Usage of a modern framework
- Fast page loads with SSR
- Better DX in general

### Conclusion of the Spike

As expected, Preact performed much better than Dojo 1. It offered all the functionalities needed for the modern web. The code was more concise, more readable, and pages loaded fast. Everything felt more modern. Overall, I think Preact is perfect for use cases where React might not be the best choice (e.g. widgets, embedded systems, or small sites).

### And today?

Well, the spike was a while ago. At that time, Preact was not yet well-known, and the frontend world evolved rapidly.

At the time of the spike, [Dojo 2](https://dojo.io/blog/dojo2-0-0-release) had not yet been released. The Dojo team learned from feedback and made several improvements. They looked at the competition and adopted some concepts.

React became even more popular and improved its performance as well. The recently released [Hooks API](https://legacy.reactjs.org/docs/hooks-intro.html) brings exciting new possibilities. The client is now more interested in a React solution. What has Preact been up to in the meantime?

The Preact team continues to work on its goals. In early March, [Preact X](https://github.com/preactjs/preact/releases/tag/10.0.0-alpha.0) was released in its alpha version. It will be the next major release and introduces new features like Fragments and Hooks. Additionally, they are continuing to work on React compatibility and are striving to deliver even better DX.

I'm curious to see how the field will continue to develop.