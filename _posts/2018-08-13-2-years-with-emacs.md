---
layout: post
title: 'Using emacs after 2 years'
date:   2018-08-13 08:07:11 +0800
categories:
---

## Preface

I've been busy recently and forgot about this blog but since I've left
my previous job I have all this free time to revisit things. 2 years
has passed since I switch from sublime to emacs. I must say I've grown
to like it more than ever.

This is about what I've experienced over the time I used emacs.

## Philosophy

I am now a converted fan of **Emacs**. I've noticed that there always
religious debate between emacs and vim or any other editors. I believe
the cause of it is mostly due to nature of the emacs users, because
they expect other people to attain same conclusion they had.

You see, emacs has different philosophy. Maybe it is because of FSF
mission.

> Free software developers guarantee everyone equal rights to their
> programs; any user can study the source code, modify it, and share
> the program

Not all developers wants to exercise this right. This I believe is
the main cause of division between Emacs and Non-Emacs users.

## Experience

As I use Emacs as my editor, I started to see things
differently. There was a frustration at first, mainly because
mainstream application/editor I was used to, doesn't behave like
emacs. But as I started to do things in emacs way. I slowly started to
realize things. It has this liberating feeling.

Sounds philosophical yet true.

## Realizations

# Mouse vs Keyboard

The first thing I realized is the **Mouse**. Most of emacs commands
doesn't require you using the mouse only your keyboard. And there are
advantage to it.

- Keyboard allows you to use all your 10 fingers
- No input switching between mouse and keyboard (it slows you down,
  believe me)
  
I am a photographer, I know that auto-mode is almost only used on
entry level cameras. This is the equivalent of mouse to
computers. Mouse are created to lower the barrier on using
computers. But as your proficiency increases you will want more than
what the mouse can offer.

# Text vs GUI

After learning how to use the keyboard alone, I now realize that I am
not dependent on GUI anymore. I see how simple things are and that
almost everything can be done using text only.

Also because I am getting used to text-only I started notice the
consistency in the way I navigate, edit or use programs. Something I
never achieve when I am using GUI applications.

# Customization (Open Source)

Now I'm familiar to emacs way of doing things, I started to think of a
better way of doing things. Example is I would like that whenever I
save I also want to run `make test` on the other window. So I can see
If I am breaking my code. I just write a code to do just that and bind
it to a key like `Ctrl + r`. Another one, when I press `Alt + .` i
want to go to the definition of the function I'm editing then `Alt + ,` 
to go back.

```elisp
(progn
  (defun save-test()
    "Save current file then run `make test'"
    (interactive)
    (save-buffer)
    (with-current-buffer "sh-program"
      (erase-buffer)
      (insert "make test")
      (comint-send-input)))
  (global-set-key (kbd "C-r") 'save-test))
```

It is straightforward on how to customize things. Everything is
available and properly documented. I can see how opening a file is
implemented and work from there. I did create plug-ins in sublime
before using python plug-ins to help me on frequent tasks I am
doing. Sublime exposes limited set of API that I can use but it is
nowhere what Emacs offer.

In emacs almost everything is exposed or available to the
user. Everything is a function. When I'm just inserting a character
`'c'` it is calling the function `insert-command`, this allows me to
redefine almost anything.

Because everything is accessible and editable, this awakens the
developer spirit in me. As a developer we develop software that helps
the user. In this case I am the user and I want to develop things that
will help me. Just like a chef cooking for himself.

# Lisp vs Python vs Javascript

Most editor now can be extended using Python (Sublime) or Javascript
(Atom). Emacs can be extended using Lisp. Lisp has the simplest syntax
of the three and is the most flexible. It is homogeneous, there is almost
no difference between code and data. Everything can be
manipulated. You want your own `if-else` clause or even your own
`do-while` flow you can define it yourself.

```elisp
;; --------------------------------------------------------------------
;; Macro Definition

(defmacro do (body while cond)
  "Control Flow: DO WHILE (do <exp> while <cond>) implementation"
    `(progn
       ,@body
       (while ,cond
         ,@body)))

(defmacro ife (cond success else fail)
  "Control Flow: Custom IFELSE (ife <cond> <exp> else <exp>) implementation"
  `(if ,cond ,success ,fail))


;; --------------------------------------------------------------------
;; Usage: Do While

(setq temp-var 0)
(do ((setq temp-var (1+ temp-var))
     (insert "%s" temp-var))
    while (< temp-var 5))

;; Usage: If Else
(ife t
   (message "true")
 else
   (message "false"))
```

Because of this Lisp gives the programmer the ability to do
anything. It is up to the programmers creativity on how he want to use
it. This works extremely well with Emacs. Emacs allows me to customize
almost anything and the language it uses (lisp) allows me to code
in a whatever way I want.

## Freedom

Due to the realizations I had, It now changed the way I think. I
constantly has this urge to taylor fit any application on how I am
going to use it and whenever I can't it pisses me off.

I now see how cluttered the software ecosystem is. That most of it
originated from our unpreparedness to the sudden explosion of personal
computers. When ordinary people has access to computer they chose
the path with lower learning curve GUI despite the productivity when
using text-based programs.

I am not saying that efficiency trumps convenience. They are different
criteria and not correlated.

Some people would like efficiency over convenience like me. Others would choose convenience.

It is like normal person would choose cars that uses automatic
transmission, a racer would likely to use a manual type. Are you a
racer or a normal person? No point in convincing that everyone should
be a racer. Because it depends on the context on how you use your
tools.

## Recommendation

Are you frustrated with the currently available software? Feels like
nothing fits your workflow?

Then **YES** emacs is for you.

Are you contented on what current software has to offer? And currently
happy with your workflow?

Then **NO** emacs is not for you. You are better of using Vim, Sublime or
any IDE.

Its like the red pill or blue pill in the matrix. 
![Red Blue Pill](https://qph.fs.quoracdn.net/main-qimg-ed23d9866da0882d2b994338a31dc8fa)

`Emacs or Lisp` is the red pill
