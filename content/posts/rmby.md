+++
authors = ["Yadullah Duman"]
title = "My first NPM library: rmby"
date = "2020-08-17"
description = "A Node.js library for removing files asynchronously with a fluent interface"
categories = ["Node.js"]
tags = [
    "Node.js",
    "NPM",
    "Backend"
]
+++

Recently, I released version 1.0 of my first Node.js library called [rmby](https://www.npmjs.com/package/rmby). In this post, I want to explain what it is, what it does and how you can use it. On the bottom of this post you will find the respective links.

## **What is rmby?**

rmby (_“remove by”_) is a zero-dependency Node.js library for removing files asynchronously on a given directory by certain filter criteria’s. It provides a [fluent interface](https://martinfowler.com/bliki/FluentInterface.html) that guides the user when creating the remove query.

## **Why rmby?**

In one of my side projects I had the use case where I needed to remove files on a directory that were older than X hours with a cronjob. The first thing you usually do is to look for existing libraries. I found one, but it only supported synchronous removal and I had to provide the time in minutes instead of hours.

It did the job, but somehow I felt like I could easily develop this by myself and it would also run asynchronously. So, I did and I could also provide the amount of hours.

Then, I thought about extending this behavior by more filter criteria’s. Why not remove files by minutes, or seconds, or names? Since I was always curious about library development in Node.js anyway, this was a good opportunity to try it out and think about the public interface surface.

So I began coding…

## **Usage**

In order to use rmby, install it with NPM or Yarn.

```bash
$ npm i --save rmby
$ yarn add rmby
```

And then import it in your project.

```tsx
// JS
const { remove } = require("rmby")

// TS
import { remove } from "rmby" 
```

## **The API**

rmby follows the pattern of a fluent interface. With this technique you define a grammar for your API. Every chain in the method has a rule on what to call next. This prevents incorrect usage of the API and guides the user through the method chain with code completion tools like IntelliSense. In order to execute the removal, you need to call the `run` method on the end of your chain, which will return the file paths that were removed upon termination.

### **Remove By Time**

Files can be removed by a time difference in milliseconds, seconds, minutes or hours. The time difference is always checked against the current time.

```tsx
remove().from('/some/dir').byTime().olderThan(1200).milliseconds().run()
remove().from('/some/dir').byTime().olderThan(90).seconds().run()
remove().from('/some/dir').byTime().olderThan(150).minutes().run()
remove().from('/some/dir').byTime().olderThan(18).hours().run()
```

### **Remove By Name**

Files can be removed regarding its name without considering the file extension. You can remove files that match exactly, start with, end with, or include the name that you provide.

```tsx
remove().from('/some/dir').byName().thatEquals('index').run()
remove().from('/some/dir').byName().thatStartsWith('ind').run()
remove().from('/some/dir').byName().thatEndsWith('dex').run()
remove().from('/some/dir').byName().thatIncludes('nde').run()
```

### **Remove By Extension**

Files can be removed regarding their file extension. You can remove files that match exactly with the extension you provide.

```tsx
remove().from('/some/dir').byExtension('.log').run()
```

### **Chaining Filters**

Files can be removed by combining the available filters. Therefore you can create more specific filters for your use case.

```tsx
// Remove all log files that start with "app" and are older than 12 hours
remove()
  .from("/some/path/to/dir")
  .byName()
  .thatStartsWith("app")
  .and()
  .byExtension(".log")
  .and()
  .byTime()
  .olderThan(12)
  .hours()
  .run();
```

## **The Future**

The library itself should work fine for a single directory. If I find more time besides work, I want to add more filters and also add the capability to remove files recursively in deeply nested directories.

## **Links**

[GitHub](https://github.com/yduman/rmby) [NPM](https://www.npmjs.com/package/rmby)