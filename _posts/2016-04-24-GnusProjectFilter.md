---
author: ecocode
layout: post
title: "Fast project mailbox filtering in Gnus with helm"
date: 2016-04-24 18:12
category : Mail
comments: true
tags:
- emacs
- gnus
- helm
---
I happen to have quite a lot of mailboxes on my multiple IMAP servers. Actually I have 1393 mailboxes. I tried to handle these from within Mail.app as I'm on MacOSX. Mail.app is a nifty mail client, but the graphical user interface just isn't made for that much mailboxes. I bought the MailHUB plugin which does quite a nice job at handling a numerous quantity of mailboxes, especially using fuzzy match. Eventually I figured the workflow was too slow to my needs.<!--more-->

For years now I'm also reading mails through Emacs [Gnus](http://gnus.org), while still using other IMAP clients to check my INBOX. Gnus handles lots of mailboxes without issues and my workflow is impressively fast. In my experience, Gnus expiry system is the most valuable feature. This post however is about workflow and speed.

On average each of my projects holds 10 mailboxes. I use a hierarchy for naming the mailboxes. So all mailboxes belonging to a specific project are located in a virtual mailbox named after that project. I call it "virtual" since the project top mailbox isn't actually existing on the mail server. This is a limited structure of my mailboxes:

{% highlight bash %}
.
└─ Projects
     ├─ 40660
     │   ├─ administration
     │   ├─ buyings
     │   └─ subcontractors
     │       ├─ a
     │       └─ b
     ├─ 40661
     │   ├─ administration
     │   └─ ...
     └─ 40662
         ├─ administration
         └─ ...
{% endhighlight %}

As my memory seems to be much more number-oriented than name-oriented, I refer to projects by numbers. Most people probably prefer to use significant names instead. One of the pros of using incremental numbers is that these numbers increase over time which somehow gives an idea to when a project was actually started.

When I started using Gnus I navigated through mailboxes by using incremental search `Ctrl-s`. This was clumbersome and I soon switched to [topic-view](http://www.gnu.org/software/emacs/manual/html_node/gnus/Group-Topics.html), which kept me going for some months at least.

You might have guessed that all my 1393 mailboxes also include finished projects. Arguably I could archive these far far away, removing a lot of mailboxes from my groups view. However you never know if a project somehow just got reactivated. Or you might need the archive for similar future projects. Therefor I move finished projects to another IMAP server, which Gnus handle just nicely. And of course I lower the group level of those mailboxes. Group levels really is a cool feature of Gnus! They reduce my listed mailboxes to something around 300 at level 3. Perhaps I could reduce them to around 100 which still wouldn't be appropriate to my project workflow.

Some years ago I discovered `A M` which filters shown mailboxes based on a regexp. I immediately understood the cleverness of this command and moved away from topic-view since. It was easy to overview all project-related mailboxes just by issuing `A M 40660`. The workflow speedup was just amazing! If you ever used a graphical mail client with more than 100 mailboxes, you easily understand the speed improvement this represents compared to scrolling and clicking a mailbox tree.

Eventually I figured most of my filtering mailboxes activity indeed concerns active projects. So I thought I could improve my workflow a little more with the use of a predefined list of active projects and use [helm](https://emacs-helm.github.io/helm/) to select one. Here comes my code for this:

{% highlight emacs-lisp lineos %}
(setq ec-active-projects '("40657" "40660" "40661" "40662" "40663" "40664" "40665" "40666"))

(defun ec/gnus-project ()
    (interactive)
    (let (project) (setq project (helm :sources (helm-build-sync-source "Project"
                                   :candidates ec-active-projects
                                   :fuzzy-match t)
                        :buffer "*helm Projects*"))
    (gnus-group-list-matching 6 project)))

(define-key gnus-group-mode-map (kbd "d") 'ec/gnus-project)
{% endhighlight %}

Since I'm using this filtering method quite a lot I needed to assign it to a single keystroke. I used "d" but you might prefer something else.

Helm allows fuzzy matching which is amazingly cool. In my case I now need following keystrokes to filter mailboxes of one of my active projects: `d666`. These 4 keys replace the already speedy `AM40666`, representing a gain of no less than 3 keystrokes!

It's awesome how we still can increase workflow speed by a little bit of code!
