Helcim - UX Engineer Coding Challenge

This README covers a few notes to consider when reviewing my submission.

 * Introduction
 * Considerations


INTRODUCTION
------------

Memo is a social media app where users write memos to share with others. A memo is a widely broadcasted reminder/message to a large group of people, typically in a business or organization. Although this is not important, I thought the name was somewhat clever.

The color scheme of Memo is purple with heavily-contrasting gold accents. The theme is quite arcade-like (minus the pixels) featuring retro-style graphics and a blocky logo.


CONSIDERATIONS
------------

1. I opted to keep the all of the JavaScript in one file, as well as all the SCSS. This was mainly due to time constraints and to keep things simplier.

2. I was originally going to include more hover events for some elements, but realized that mobile users would not be able to interact with them. In turn, the user flow would be compromised which was compensated by simplifying those parts of the implementation.

3. I considered making the implementation friendly to all browsers. However, I figured that it would have been mentioned inside the challenge document, so I stuck with non-conforming code.

4. I used a nodemon script to help compile my SCSS into CSS. However, the code does not require any npm installations or downloads; it was merely for convenience-sake.