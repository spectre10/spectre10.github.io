---
title: "GSoC: Week 11"
date: 2024-08-12
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

I iterated on [t-urlmatch-normalization](https://github.com/spectre10/git/commit/7e56a101ea3added2203602dffbb3b52442fdb9b). The v2 of that is
ready locally and I will send it in this week. I'll also send [t-oidset](https://github.com/spectre10/git/commit/68e8ee1c7aa1ca7d1996498518bd9a0366372f33) after I send
t-urlmatch-normalization. Other than that I worked on t-revision-walking and lib-repo a
little bit.

I was also a bit hesitant to add further things to the test-lib, as there are discussions/patches
to replace it with a new framework called '[clar](https://lore.kernel.org/git/cover.1722415748.git.ps@pks.im/T/#m82aad1140e86984f3bb6472b9446b45f63880b9a)'. I held back on t-urlmatch-normalization due to this,
but I'll send it without the addition to the test-lib, as waiting for the consensus on 'clar' might
become too late.

## What is the plan ahead?

I'll work on getting [t-oidarray](https://lore.kernel.org/git/20240803132206.72166-1-shyamthakkar001@gmail.com/) merged as it is waiting for a review on the list.
And also work on getting [t-hashmap](https://lore.kernel.org/git/20240803133517.73308-2-shyamthakkar001@gmail.com/) merged, if any other work is required on that part (Although Christian has given the final ack, so it should be good to go).
Also send t-urlmatch-normalization and t-oidset and work on t-revision-walking and lib-repo. As this
week was kind of slow, I did not face any particular difficulties.

Thanks and see ya' next week!
