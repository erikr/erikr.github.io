---
layout: post
title: One-click journal access via MIT proxy bookmarklet 
tags: academia
---

> Disclaimer: this post is for people affiliated with MIT.

[proxybookmarklet.com](https://proxybookmarklet.com) is the best way to quickly log in to medical journals. Thanks, [Nate Gross](http://nategross.com)!

Unfortunately, that site does not have MIT among its institutions, so you can't generate a bookmarklet.

The [MIT Libraries site](https://libraries.mit.edu/research-support/connect) does not help either; although there is a section on browser bookmarklets, it is a nonfunctional JPG.

Here is a working MIT proxy bookmarklet. To use, drag the button to your bookmarks toolbar:

[![MIT proxy](/assets/mit-proxy-button.jpg)](javascript:(function(){window.s0=document.createElement('script');window.s0.setAttribute('type','text/javascript');window.s0.setAttribute('src','https://bookmarkify.it/bookmarklets/15285/raw');document.getElementsByTagName('body')[0].appendChild(window.s0);})();)

Ready to give it a try? Go to this [example article](https://www.nature.com/articles/s41586-019-1284-2) in Nature, then click the proxy bookmarklet in your browser toolbar.
