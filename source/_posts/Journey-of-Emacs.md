---
title: Journey of Emacs
date: 2017-02-22 00:12:21
tags: [Emacs,Markdown]
toc: true
---

I have switched my programming environment to Manjaro Linux recently. So I started to give a try on legendary editor Emacs for development. I started with zero basics for Emacs and spent only 2 week's spare time to get used to writing code and blog with it.

## Standing on the Shoulders of Giants
* Do not blindly start to learn Emacs commands or lisp language, learning without helping you to achieve your goal will make you give up soon. You should start from guru's setup to achieve what you want to do and learn the commands and everyting along the way.
* I started by taking advice from Chen Bin [redguardtoo ](https://github.com/redguardtoo). Quickly read his post ["mastering-emacs-in-one-year-guide"](https://github.com/redguardtoo/mastering-emacs-in-one-year-guide).
* Followed the recommendation, I spent a little time going through the emacs tutorial. ThenI downloaded his [configuration](https://github.com/redguardtoo/emacs.d) to play around.
* Very importantly, write down the important steps. Emacs has powerful plugin management functionality, when you made so many customizations to emacs you want to make sure to write down the steps.

## Explore the World of Emacs
* So many good things guru's has setup for you. Try it.  
1. Spent half an hour to learn basics of Org Mode and play with M-Org-Indent-Mode in Chen's setup. You can write down the steps using org-indent-mode as I did. ![](/images/org-mode.png)  
2. Configure neotree and install all-the-icons package for it
3. Configure c++ packages for development
4. Use Git version control in emacs with magit
5. Write blog in Markdown mode which is pretty much I am doing now. 
...

## Two Configurations I Recommended
1. Chen Bin's setup [redguardtoo]("https://github.com/redguardtoo")  
It comes with pretty much everything I need and a very clear structure inherit from Steve Purcell. The theme is very beautiful and the font colors are very comfortable for me to write programming code. Note that Chen carefully pick the stable version of ELPA packages and you need to add unstaple exceptions in his configuration file to see the package in package installation list.


2. [Spacemacs](http://spacemacs.org/)  
Spacemacs has very good documentation explaining its structure. The package management module of Spacemacs utilizes the packages as building blocks and assemble them into layers. You can customize your layers with all these building blocks. Be sure to checkout the awesome [emacs port for solarized theme ](https://github.com/sellout/emacs-color-theme-solarized).
A screenshot of my spacemacs workspace shows this beautiful theme.
![](/images/solarized-dark.png)
![](/images/solarized-light.png)

3. I prefer Chen's setting. I felt it's faster than Spacemacs and still easy for me to plugin anything else in due to the clear structure.

## Some Configuration Steps I Went Through
[In Org Mode, best read in emacs org-indent-mode](https://github.com/xueyouchao/todo/blob/master/EmacsToDo.org)
