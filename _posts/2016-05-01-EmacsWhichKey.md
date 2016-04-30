---
author: ecocode
layout: post
title: "When you can't remember all key combination in Emacs"
date: 2016-05-01 00:14
category : Emacs
comments: true
tags:
- emacs
---
Emacs has no limits. It has endless modules and even more functions. Logically you end up with numerous key-combinations. Even if you figure to remember a lot of them by using them, you might happen to forget key combinations you use less. Well, that is my case :)

The one nice thing in Emacs is that a lot of key-combinations are grouped by prefix. i.e. <code>C-c p</code> for projectile, <code>C-h</code> for help, etc. Personally I use <code>f5</code> as a prefix for custom functions I wrote.

Enters [which-key](https://github.com/justbur/emacs-which-key) mode. Install it from Melpa and add following line to your Emacs configuration.

{% highlight emacs-lisp lineos %}
(which-key-mode)
{% endhighlight %}

Which-key will pop-up a list of key-completions starting with the prefix you enter, adding the name of the function called.

There are some customizations available which you'll find in the <code>README.org</code> file of the project. Most concern the location of the list (to the right, left, bottom of the screen or in the mini-buffer or also to the right for short lists but moving to the bottom when the list becomes bigger). I choose to keep the default but modified the time lapse before it appears (default 1 second) to 1/2 second. This is the configuration line to add for this:

{% highlight emacs-lisp lineos %}
(setq which-key-idle-delay 0.5)
{% endhighlight %}

Check it out if you can't remember all keystrokes as I do!
