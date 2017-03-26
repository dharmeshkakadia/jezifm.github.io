---
layout: post
title: 'First steps with emacs'
date:   2017-03-26 00:47:38 +0800
categories: emacs
---

Assuming you have sucessfuly installed emacs, running emacs for the first time you will be welcomed with a emacs splash screen.

![splash-screen](/assets/emacs-splash-screen.png)

Not as helpful for someone new to emacs, all I can see is a cursive font with a horse. This is where the challenge starts, what to do next? I suggest you start with reading the tutorial included in the package. To go thru the tutorial you only need to press `return` since the cursor defaults to the tutorial link.

> Press `return` as your first key, after opening emacs for the first time.

This will open up the emacs tutorial. You will go through the basics of navigation, editing, browsing. This is one thing good with emacs all you need is within the editor. The following is the summary of what I think is important.

### Basic Navigation

Emacs was built with the idea that you can do everything by only using the keyboard, when I say keyboard it is the one without arrow-keys! So how do we move without the arrow keys

- `C-p` will move up (p for previous)
- `C-n` will move down (n for next)
- `C-f` will move right (f for forward)
- `C-b` will move left (b for back)

`C-` means hold the `crtl` key. This drove me nuts the first time, then eventually grew on me. For some reason `p`, `n`, `f` and `b` feels more natural than the arrow keys as I don't need to move my fingers a lot.

I suggest you take a break here. Then move forward only when you feel comfortable with the keys.

Next is scrolling, scrolling in emacs moves the page up and down. `C-v` to scroll one page down, and `M-v` to scroll up. `M-` means hold the `meta` key or sometimes synonymous to `alt` key. When you scroll up and down the cursor remains at its current position. This is different from normal editor where srolling down will move your cursor to the bottom of the screen.

#### Opening a file

To open a file press `C-x C-f` *(hold ctrl while pressing x then f)* this will prompt user to input the path to open. Its like pressing `C-o` or *open file* in normal editor. The only difference is in emacs we call this `visiting a file`. Visit means if the path/file doesn't exist it will create one. This combines creating and opening of a file which is very intuitive.

### Editing

Now that we can move through the file, inserting text is the same with any editor, just type the characters and it will be inserted before the cursor.

For deleting text we can use `delete` or `backspace` to delete a character. When we want to do a bulk delete Eg. delete a whole word we highlight the word using the **mouse (double-click)** then press `backspace`. In emacs we just press `M-d` and the word after the cursor will be deleted. This is where emacs shine, normally **mouse and keyboard** are involved in performing deletion of word, in emacs you only use your **keyboard**. Same logic applies with deleting line, `C-k`.

#### Copy and Paste

In a normal editor we highlight the text and press `C-c` to save text to `clipboard` then `C-v` to insert the text from it. You can also do the same in emacs, the difference is in emacs' `clipboard` is a **15-slot clipboard** versus the standard **1-slot clipboard**.

To use the **multi-slot clipboard** of emacs we need to be familiarized with its term.

|Normal term|Emacs term|
|---:|:---:|
|cut|kill|
|delete|kill|
|copy|---|
|paste|yank|
|clipboard|kill-ring|

Everytime we `cut/delete` in emacs it is appended to its `clipboard`. So when we perform `paste`, the latest item in the `clipboard` will be inserted. But what if, what we want is the second to the last text in the clipboard, in that case we just perform the `paste` twice. This will replace the inserted word with the previous item in the clipboard. This is different from normal where pressing `paste` twice will result in duplicated text. You can use the same logic for accessing nth-slot of the `clipboard`.


We barely scratch the surface, as you can see the basic functions in emacs are not so basic at all. But when you get to familiarized with this commands it will start to feel natural. And you'll become aware of the inefficiencies of your current workflow. Then your journey to micro-optimization begins, and emacs will be you best buddy.
