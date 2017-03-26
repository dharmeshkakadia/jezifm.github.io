---
layout: post
title: "Why did I move from sublime to emacs?"
date: 2017-03-25 18:38:43 +0800
categories: blog emacs
future: true
---

While looking for a good article for **TDD**, I came across somewhat deep side of the net and stumble across a [video](http://emacsrocks.com/e01.html) showing how awesome emacs was. The way he changed `var` to `this` was magical, I was mesmerized. 

> I'm always a fan of text-editor, as it allows me to switch language easily. I can go from `python`, `R`, `SQL`, `bash` seamlessly.

As an avid user of **sublime**, I always thought that multi-cursor, package manager, file-search features from **sublime** was very powerful that I wouldn't need anything else. If you want to go to a file just type partial part of the text and in realtime a list of candidate files will be suggested. If I want to edit different parts of the text simultaneously I can shift+click on each location and my changes will propagate through all the cursor. I also have 3-5 python plugin and 20+ snippets for sublime customized for my daily work.

> I'm productive as hell when using sublime

Then suddenly here comes a video showing text manipulation in a very different way. What confused me the most is how did he manipulated the text without a **cursor**, how was even that possible. I mean how can you tell your editor to change only certain portion of the text without moving the **cursor**.

Another point was, I couldn't reason out why use something else If I have the almighty powerful `sublime`. So I went to google and search for *"emacs vs sublime"*, first result was from *quora* and is the exact question I had in mind.

> Why use Emacs over Sublime Text?

I was a suprised, what I'm expecting is a response like `sublime` is the best and `emacs` is a thing of the past and is not relevant in today's time, but it wasn't the case. The answer was detailed and too long that I was lost in all the points he was raising.

In summary these are his main points:

* **Lisp Dream Machine** - I did not know anything `lisp` for what I know its a dead but not dead language, the **Dream Machine** part sounds cool.

* **Opening file from remote file server** - I once did setup an SFTP connection in sublime, though not the best experience, I still prefer editing files in my local setup then SSH my way to the remote server to either `git pull` or `rsync` to cascade changes from my local machine

* **Shell Mode** - When using sublime, I always have a terminal window open beside it, to run terminal commands like `SSH`, `editing files`, `running scripts`, etc. I always wished I can use sublime features in the terminal, like `quick-search`, `snippets`.

* **Organizing notes** - this is new to me. I never used a text editor as an organizer, I preferred the real notebook for jotting down notes.

* **Writing documentation** - I use markdown for documentation, It was good enough for me.

* **Different programming language support** - I have syntax highlighting from different languages, setting up is very easy, just fire up the package manager and type the language then it will be installed to `sublime`. Though for snippets, they are shared across all the languages. Eg. typing `for` then activating `tab-completion` will expand to `for e in elements: ` even when editing SQL files. 

And many more, etc...

From this point, my interest in `emacs` grew. I feel I'm missing something big, and it is related to this unknown text editor that has this elite/hackerish feel. For the next post, I will talk about my first steps on using emacs.
