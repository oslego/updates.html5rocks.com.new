---
layout: post
title: "State of CSS Masking at the end of 2014"
description: "An in-depth look at the state of implementation of CSS Masking functionality across browsers"
article:
  written_on: 2014-12-12
  updated_on: 2014-12-12
authors:
  - oslego
tags:
  - css
  - masking
  - graphics
  - front-end
permalink: /2014/12/css-masking-state-2014
---

Chris Coyer of CSS Tricks recently wrote a good overview of the CSS Masking and generic browser support. That's an excellent place to start learning about CSS Masking, but it is just a high-level introduction. This post complements that one. It offers a more accurate summary of the implementation state of CSS Masking as it stands at the end of 2014.

By the nature of its topic, this post is ephemeral; it will become obsolete. But, for now, it aims to help developers experimenting with CSS Masking understand what's expected to work in each browser, what isn't, which features are still behind flags, what are the known issues and workarounds for them. This is for the explorers.

At the time of this writing, CSS Masking is still in active development. The feature is complex and it offers multiple functionalities, like clipping, masking, referencing SVG masks for use on HTML, and so forth, which are at varying states of implementation across browsers.


## Outline
- Clipping
  - prefixes
  - builtin shape functions
  - referencing SVG `<clipPath>`, quirks, `transform: translate(0,0)` workaround.

- Mask
  - prefixes
  - alpha-mask
  - luminance-mask, flag
  - mask-type, flag, name
  - referencing SVG `<mask>`
    - attributes
  - mask-repeat
  - mask-size

Table of support
- Chrome, Firefox, Safari, WebKit, IE, Opera
