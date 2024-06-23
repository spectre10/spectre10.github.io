---
title: "GSoC: Week 4"
date: 2024-06-23
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

During this week, I sent [`t-oidmap`](https://lore.kernel.org/git/ZnP6G6SSBynlBNUj@google.com/T/#t) migration patch to the mailing list. It is still waiting for a review, but hopefully it will
get reviewed in the next week and be merged to at least 'seen'. Apart from that, I also migrated `helper/test-hashmap.c` to [`unit-tests/t-hashmap.c`](https://github.com/spectre10/git/commit/b2e318ed02e3ca1a4b6472e68d966a5ab1b775c5).
That is ready to be sent to the mailing list, but I'll send it in the coming week to give some time to `t-oidmap` to be reviewed and not spam the list.

Apart from that, I also migrated `helper/test-urlmatch-normalization.c` to [`unit-tests/t-urlmatch-normalization`](https://github.com/spectre10/git/commits/t-urlmatch-normalization-3/), but there are still some minor
things to polish out in that patch. Also you might remember from my previous blog that I am waiting on [`ps/use-the-repository`](https://lore.kernel.org/git/cover.1718347699.git.ps@pks.im/) to be merged to next
so that I can send out [`t-oid-array.c`](https://github.com/spectre10/git/commit/6f71829e7be6b297cf978ed7c3ab1a18d0747c41) which is ready in my tree. And from the latest 'What's cooking', it is to be merged to 'next' soon, so I'll
probably send out that patch as well in the coming week.

There were no particular difficulties that I faced in this week. It was rather smooth as I migrated two legacy tests to the unit tests. To give a bird's
eye view of the progress:

 - [t-strcmp-offset](https://lore.kernel.org/git/20240519204530.12258-3-shyamthakkar001@gmail.com/) (Co-authered-by: [Achu Luma](https://github.com/achluma)) (master)
 - [t-hash](https://lore.kernel.org/git/20240529080030.64410-1-shyamthakkar001@gmail.com/) (Co-authered-by: [Achu Luma](https://github.com/achluma)) (master)
 - [t-example-decorate](https://lore.kernel.org/git/20240528125837.31090-1-shyamthakkar001@gmail.com/) (master)
 - [t-oidtree](https://lore.kernel.org/git/20240608165731.29467-1-shyamthakkar001@gmail.com/) (master)
 - [t-oidmap](https://lore.kernel.org/git/ZnP6G6SSBynlBNUj@google.com/T/#mcfac487a68d2847638308aa4eb4d281444f06f31) (posted, waiting for review on the mailing list)
 - [t-oid-array](https://github.com/spectre10/git/commit/6f71829e7be6b297cf978ed7c3ab1a18d0747c41) (ready locally, waiting on `ps/use-the-repository` to be merged to 'next')
 - [t-hashmap](https://github.com/spectre10/git/commit/b2e318ed02e3ca1a4b6472e68d966a5ab1b775c5) (ready locally)
 - [t-urlmatch-normalization](https://github.com/spectre10/git/commits/t-urlmatch-normalization-3/) (mostly ready locally)

With all said and done, I gave an estimate of five to six tests to migrate in my original proposal before the halfway. And I am probably on track to that fulfill that estimate.

## So what is the plan ahead?

For the next week, I plan to finish writing `t-urlmatch-normalization` and also reiterate, if necessary based on the reviews, on `t-oidmap`, `t-oid-array` and `t-hashmap`.
Apart from that, I have been meaning to work on `lib-repo` but it keeps taking the backseat due to other test migrations. I'll try to see if I can write up a working version of that
in the late next week.

That's it for now. See ya' next week!
