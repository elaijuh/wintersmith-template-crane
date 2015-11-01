---
title: Brief on z-index
date: 2015-11-01 07:55
template: article.jade
tags: css 
---

Notes on css z-index:

- positioned elements are always on the top of static elements
- z-index is applied to positioned elements (absolute, relative, fixed)
- z-index has no effect on static elements, even if explicited defined
- z-index's effect scope is within siblings. Comparing elements under different parents has no meaning.

[demo](https://codepen.io/elaijuh/pen/avKQeg)
