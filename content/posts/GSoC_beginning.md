---
title: "Beginning of GSoC'24"
date: 2024-05-08
description: ""
type: post
weight: 25
showTableOfContents: false
---

The GSoC projects were announced on May 1st, and I was fortunate enough to be
selected for the project '[Move existing tests to a unit testing framework](https://summerofcode.withgoogle.com/programs/2024/projects/e9C4rhrv)'.
My proposal can be read [here](https://docs.google.com/document/d/1wmosedy-UKd_mhAAjccv1qETO1q00s-npYj2m6g-Hd0/edit?usp=sharing).
All in all, I am really excited to be contributing to Git under the mentorship
of [Christian Couder](https://github.com/chriscool) from [GitLab](https://gitlab.com)
and [Kaartic Sivaraam](https://github.com/sivaraam).

I plan to post about my progress every alternate mondays. The post feed can be
found [here](https://spectre10.github.io/posts/). My Git fork, where I'll
primarily be pushing before sending to the mailing list, can be found
[here](https://github.com/spectre10/git).

## A Synopsis about the project

Git has traditionally used helper binaries which import the library to test a certain functionality
and these binaries take arguments/options for a desired output. These binaries are used by the
shell scripts to test their output against the expected output.

However, due to many reasons outlined in my proposal,
this setup of testing through shell scripts may not be ideal. Therefore, a new [TAP](https://testanything.org/) <span style="color:red">(</span>
Test Anything Protocol, which allows individual tests/TAP producers to communicate
test results to the testing harness in a language-agnostic way<span style="color:red">)</span> implementation of a unit testing framework
was merged into Git late last year, which allows us to write unit tests solely in C. And this project involves
converting some of the existing unit tests (shell script based ones) to C using the new framework.
However, some these t/helper binaries are also used as utility tools for setup purposes elsewhere
in the testing suite, therefore these types of binaries would be low on the priority list of possible
migrations to the new unit testing framework.

## What's next?

The official coding period for GSoC begins on 27th May. Until then I will be
looking at the existing legacy unit tests, and deciding with mentors on which
unit tests are worth converting to the new framework.

This project was also taken up by the recent Outreachy intern [Achu Luma](https://github.com/achluma),
but some of her patches seem to have dropped to the void. I'll also be looking
at how to get them merged during this phase of community bonding.

Ciao!
