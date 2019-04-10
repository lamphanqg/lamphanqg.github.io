---
layout: post
title: "Prevent momentum dragging in iOS"
tag: [jquery, javascript]
---

# Problem

In our project, we're using [colorbox library](http://www.jacklmoore.com/colorbox/) to display modals. The problem I had to solve is that our modal is a table, and one cell of that table has a long content which need to be scrollable, while the other parts of the modal have to be fixed. In PC or Android phones, you can just use CSS (`overflow: hidden`) to make that happen. But it does not work in iPhone, because iOS has a default momentum scrolling/dragging thing. Even you use `overflow` to disable the scrolling, the dragging still happens. Because of that, when I drag other parts of the table, the whole table is bouncing for around 1-2 seconds, and in that time, I cannot scroll the special cell. All my touchs are received by the bouncing table, not the cell I want. (Actually, it takes me some hours to realize that, because on screen, the table does not move at all, so I cannot see what happens to the table layer. Only after I connect iPhone to Macbook and debug, then I can highlight the layer and see it moving.)

![scrollable cell problem](/assets/images/iphone_scroll_problem.jpeg)

# Solution

Fortunately, it seems like there are many other people got that problem. Basically, the solution is to prevent the table to receiving `touchmove` event, but allow the cell inside it to receive it (to be scrollable). So the code will look like this (with jQuery):

```javascript
$("#colorbox").on("touchmove", function(e) {
  var target = $(e.target);
  if (!target.hasClass("scrollable")) {
    e.preventDefault();
  }
})
```
