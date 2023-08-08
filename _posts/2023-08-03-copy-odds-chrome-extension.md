---
title: Enabling Text Selection on Sportsbetting Sites
author: carl_zhou
date: 2023-08-03 14:33:00 +0800
categories: [Programming]
tags: [javascript, typescript, chrome extension]
pin: false
math: true
mermaid: true
image:
  path: /assets/pinnacle-outrights.png
  lqip: data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAGCAYAAAD68A/GAAAAAklEQVR4AewaftIAAAC8SURBVF3BP0tCYRTA4d+5Hd+r17egO6Q4NVwIQS7h5Bh+kL6EoUtzSzjV5mewtTmChjY357YQUrQ/4qXzBk7a80jv6jrM179wUAKJCAgl52idZfxsCmbzFbZ4R/uDHs8vr9zejVh+FwSNSWLHxeyJZt7kq3uJOoeKRLTPc+6HN0QYuxJf5TQ9JC6XUYKRHB2jPqXm2fPxWaAaMDNUhK3VGmqePZPpG4/jB7L6CWoIZgFE+M9XE/JORqOS8gdJ+DXW7agGTwAAAABJRU5ErkJggg==
  alt: Site with betting odds
---

## The Problem

Some websites have text selection disabled for their buttons, which contains odds of a selection. This make it difficult to select text for copy/pasting into a script or spreadsheet for finding fair odds. 

This is especially a problem for futures, and for events like golf, where there could be over 100+ participants. Without text selection, one would have to manually enter every single value.

![Scrolling down list of golf winners and odds](/assets/pinnacle-golf-futures-scroll.gif "So many choices").

It is possible to edit the CSS to enable copy/pasting. However this would need to be done every time the page loads and is inconvenient.

## A Chrome Extension as The Solution

I wrote a chrome extension to solve this issue. 

The extension enables user-selection via CSS style change by JavaScript in the content script. It does not require any special permissions or making an external network call.

## Attempts to Publish to the Chrome Store

I attempted to publish the extension to the Chrome store. However it was rejected for "facilitating of gambling". 

![Pinnacle NBA Outright with copy and pasting via right click](/assets/pinnacle-outrights.png "We can select, copy, and paste text now.").

I researched this policy and I found that there currently exists several extensions in the Chrome store for "facilitating of gambling". Here are a few:

https://chrome.google.com/webstore/detail/sharpsports/jbjkingkpiakebpokhndnnmniaafoobe

https://chrome.google.com/webstore/detail/tipico-sports/kodahiemdnchcjekdlmmnfdcpmmncanb?hl=de

https://chrome.google.com/webstore/detail/rotowire/caljiholplhibonecblicbgjkakcpmmm

It is not clear how this particular policy is enforced.

## Final link to the extension

The extension can still be manually installed via the source code here:

https://github.com/carlzoo/copy-odds-chrome