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

Chris Coyer of CSS Tricks recently wrote a [good overview of CSS Masking](#linkTBD) and generic browser support. That is an excellent place to start learning about CSS Masking, but it is just a high-level introduction. This post complements that one. It offers a more accurate summary of the implementation state of CSS Masking as it stands at the end of 2014.

By the nature of its topic this post is ephemeral; it will become obsolete. But for now it aims to help developers experimenting with CSS Masking understand what is expected to work in each browser, what isn't, which features are still behind flags, what are the known issues and workarounds for them. This is for the explorers.

At the time of this writing (December 2014), CSS Masking is still in active development. The feature is complex and it offers multiple functionalities, like clipping, masking, referencing SVG masks for use on HTML, and so forth, which are at varying states of implementation across browsers.

## Clipping

Clipping defines the visible area of an element as described by a clip path. Anything outside this area will be invisible and inert to pointer events, like mouse clicks or touches. **@Dirk: is this correct?**.


The prefixed `-webkit-clip-path` CSS property is used in Chrome, Opera and WebKit when clipping HTML content. The unprefixed `clip-path` CSS property still only applies to clipping SVG content by referencing an SVG `<clipPath>` element. The functionality of the two properties will merge.

### Clipping with shape functions

Shape functions can be used to define the clip path. They're the same shape functions as described in [CSS Shapes Module](#linkTBD): `inset()`, `circle()`, `ellipse()` and `polygon()`. At the time of this writing, shape functions can be only used for clipping HTML content, not SVG content.

```css
.element{
  // prefixed for clipping HTML content in Chrome, Opera and WebKit
  -webkit-clip-path: circle(50%);
}
```

You can animate shape functions as long as the number of coordinates remains consistent throughout the transition.

When animating `polygon()` shapes, you can't use a different number of vertex pairs at different stages of the animation. The browser can't interpolate between those states. You can, however, overlap vertex pairs to create the illusion of changing the number of edges of clip-path.

```css
.element{
  animation: animate-shape 1s;
}

@keyframes animate-shape{
  from{
    // clip to a square with four vertex pairs
    -webkit-clip-path: polygon(0 0, 0 100px, 100px 100px, 100px 0)
  }

  to{
    // clip to a triangle with two overlapping vertex pairs
    -webkit-clip-path: polygon(0 0, 0 100px, 50px 50px, 50px 50px);
  }
}
```

Unlike usage in the context of CSS Shapes, shape functions for clipping are relative only to the `border-box` reference box of the target element. There is no implementation for the other reference boxes (`margin-box`, `padding-box`, `content-box`) for clipping. **@Dirk: is this correct?**

There is currently no support for clipping with shape functions in Firefox or Internet Explorer, but the feature is under consideration.

### Clipping HTML with SVG

You can use SVG to clip HTML content. Replace the shape function with a reference to an SVG `<clipPath>` element. The combined area of elements within `<clipPath>` is used as the clip path.

There is currently a [bug in Chrome](#linkTBD), Opera and WebKit where the coordinate system of the `<clipPath>` is not relative to the target HTML element. Instead, it remains relative to the parent SVG. A known workaround is to use a neutral transform on the target HTML element. **@Dirk, please add link to bug**

```html
<style>
  .element{
    -webkit-clip-path: url(#path);

    // workaround for bug in coordinate system of <clipPath>
    transform: translate(0, 0);
  }
</style>

<svg>
  <clipPath id="#path">
    <circle r="50" cx="50" cy="50" />
  </clipPath>
</svg>
```

There is currently no support for clipping HTML content by referencing SVG `<clipPath>` in Firefox or Internet Explorer.

### Clipping SVG with SVG

Though not an explicit functionality of CSS Masking, it is worth mentioning here: there is widespread support for clipping SVG content using an SVG `<clipPath>` element. This is where the inspiration originated for bringing the functionality over to HTML.

The unprefixed `clip-path` CSS property is used in this context. All major browsers support this type of clipping.

```html
<style>
.svg-element{
  clip-path: url(#path);
}
</style>

<svg>
  <clipPath id="#path">
    <circle r="50" cx="50" cy="50" />
  </clipPath>

  <rect class="svg-element" width="100" height="100"></rect>
</svg>
```

### Summary of Clipping Support

TODO: add at-a-glance browser support table for clipping


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
