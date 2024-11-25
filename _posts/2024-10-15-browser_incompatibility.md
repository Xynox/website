---
layout: post
title: "Providing Cross-Browser Compatibility to My Website"
author: "Joel Wittke"
tag: "website"
---

In this post, I'll share how I solved various issues that arose when viewing my website across different browsers.
<!--preview-->

## Background

While designing my pages—especially the blog overview page—I noticed an issue: everything below the last box element would disappear into a "void", where the background color differed from the rest of the site. I tried setting a color for the `body` in my base CSS file, but that didn’t fix it. 

Not knowing how to "reset" the site, I thought testing it on a different browser might help. So, I installed Firefox and loaded my site there.

In hindsight, this was a crucial step, even though it initially felt like a setback.

*(The background issue ended up being caused by a browser extension, but that's a story for another time.)*

## Issues Faced

Once I opened the site in Firefox, I encountered two major categories of problems:

1. Navbar alignment issues
2. Inconsistent fonts

### Navbar Issues

#### Background

I had struggled with aligning the navbar during my "Chromium phase", where the layout kept breaking. The main issues were either:
- Navbar elements were squashed together
- The logo aligned correctly, but the buttons were pushed to the far right

Neither layout was satisfactory.

#### Initial Solution

After some trial and error, I found inspiration on the [Jägermeister International website](https://www.jagermeister.com/){:target="_blank"}, which had a well-aligned navbar. By inspecting their DevTools, I noticed they used a three-column grid instead of a flexbox layout. I implemented a similar grid, which added proper spacing between elements.

The key fix was setting `margin-right: auto;` on the logo. While I didn’t fully understand *why* this worked, it centered the buttons perfectly. Since it worked, I didn't look further into it.

#### Firefox Issues

However, Firefox rendered things differently. It appeared to interpret `margin-right: auto;` differently, as the logo stretched too far right, almost to the middle of the screen. Clicking the logo even triggered a link on the extended invisible box to the right!

#### Simple Fix

The solution was simpler than I anticipated. I realized the issue was due to a missing width constraint on the logo. Once I set a specific width, the alignment issue disappeared.

**TL;DR:** The navbar alignment was off because I hadn’t set a width for the logo, even though I had set its height.

### Font Inconsistencies

I learned that browsers use different default fonts, which caused font size and spacing issues. I decided to integrate Google Fonts to ensure consistency. After selecting fonts (thanks, ChatGPT 😉), I added them via the Google Fonts API and set the styles in my CSS. This resolved all spacing and font issues.

## Want to Help?

I’m still not fully satisfied with the heading font. If you have any font suggestions, check the [issue tracker](https://github.com/Xynox/website/issues){:target="_blank"} to see if it’s already been requested. Or, feel free to [open a new issue](https://github.com/Xynox/website/issues/new/choose){:target="_blank"} with your ideas!

I’m always open to feedback, new feature requests, or bug reports. Thanks for reading!
