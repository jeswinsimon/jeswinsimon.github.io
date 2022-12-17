---
layout: post
title:  "I tried making ChatGPT write a command-line application for accessing ChatGPT"
date:   2022-12-17 19:22:00 +0530
---

**And I failed.** ChatGPT is not yet ready to replace designers, developers, or content writers, but it is probably the best tool for all of them.

I spent some time with ChatGPT and I was very impressed with the results. After playing around with it for a while, I wanted to build a command-line version of ChatGPT for two main reasons: easier access and the ability to persist my queries (unlike Dall-E, ChatGPT does not save history, and I was tired of taking screenshots).

I was planning to start by Googling 'ChatGPT API', but then I thought, why not ask ChatGPT itself whether it has an API?

![Asking ChatGPT for API](/images/chatGPT1/1.chat-gpt-api.png)

Perfect. Now how do I sign up for the API?

![Getting API keys](/images/chatGPT1/2.chat-gpt-api-signup.png)

I have my keys ready. Now it's time to build the application. The easiest thing for me to do would be to build a Node.js application, but I've been trying to learn Rust without much progress for a while, so I thought I'd try using Rust. It's a long shot, but I'm curious to see if ChatGPT will be able to code the entire thing in one go.

![Rust code for accessing ChatGPT](/images/chatGPT1/3.rust-code.png)

Not quite there, but something to get started with. Wait, how am I supposed to run this thing now?

![Cargo for MacOS](/images/chatGPT1/4.cargo-mac-os.png)

I created a new project in Cargo and pasted the previous code snippet into the `main` function in `main.rs`. But **reqwest** is an external dependency, shouldn't this be added to the project?

![Dependencies](/images/chatGPT1/5.cargo-dependency.png)

And what is the latest version of **reqwest**?

![Crate.io](/images/chatGPT1/6.crates.io.png)

At this point, the code was throwing up a bunch of syntax errors. I tried asking ChatGPT on how to use **reqwest**, but it gave back the same code snippet. So, I finally resorted to googling and came across [this](https://blog.logrocket.com/making-http-requests-rust-reqwest/) tutorial. A couple of things were missing. Since ChatGPT did not provide the `Cargo.toml` file, I had not enabled the json feature for reqwest and also did not import **serde_json** for using the `json!` macro. 

Here is the updated dependency list.

````

[dependencies]
reqwest = { version = "0.11", features = ["json"] } # reqwest with JSON parsing support
futures = "0.3" # for our async / await blocks
tokio = { version = "1.12.0", features = ["full"] } # for our async runtime
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"

````

After clearing all the syntax errors, I finally ran the project only to realize that the API endpoint and payload were wrong. Notice how, in one of the previous queries, ChatGPT provided the latest version of **reqwest** as of September 2021? ChatGPT has a cut-off date of September 2021 and is oblivious to anything that happened or changed beyond that.

![FTX](/images/chatGPT1/7.ftx.png)

At this point, I ran out of time and gave up on updating the API endpoint by referring to the documentation. 

Getting ChatGPT to build an entire application as per your requirement is too much to ask for now, but ChatGPT can be used to write parts of code for your project. Ask ChatGPT how you can recreate the text effect seen in ChatGPT responses, and it will give you two solutions, one each in JavaScript and CSS. Ask again for the same in React, and ChatGPT will happily oblige. Or maybe you would like to get your blog post spell-checked and grammar-checked by ChatGPT, like how this one was.