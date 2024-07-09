---
title: "GSoC: Week 6"
date: 2024-07-08
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

Here's a quick overview of the progress in this week,
- [t-oidmap](https://lore.kernel.org/git/20240703062958.23262-2-shyamthakkar001@gmail.com/): merged into 'next'
- [t-hashmap](https://lore.kernel.org/git/20240708161641.10335-2-shyamthakkar001@gmail.com/): sent v2 to the list -> merged into 'seen'
- [t-oid-array](https://lore.kernel.org/git/20240703034638.8019-2-shyamthakkar001@gmail.com/): sent v1 to the list
- [t-oidset](https://github.com/spectre10/git/commit/68edc2eda4b95ab24c1b51eb6c9cd8be09291814): ready in my tree

Other than these there is also [t-urlmatch-normalization](https://lore.kernel.org/git/20240628125632.45603-1-shyamthakkar001@gmail.com/) which I had sent 10 days ago, it is waiting for a review.

Apart from sending these patches, I also participated in a patch series discussion of ['rs/unit-tests-test-run'](https://lore.kernel.org/git/85b6b8a9-ee5f-42ab-bcbc-49976b30ef33@web.de/), which introduces
a new macro called _TEST_RUN()_ for running unit tests. _TEST_RUN()_ negates the need of defining functions to run unit tests, which was
the case with _TEST()_. This macro would be helpful in 't-oid-array' as it uses internal functions _test\_\_run\_*()_. Using _TEST_RUN()_ would
help us in avoiding the use of these internal functions.

However, in the mailing list, there was also scepticism around using callbacks in _setup()_ functions in the tests which use _TEST()_. Personally, I had been 
using that pattern to initialize a data-structure and release them after the callback, which reduces boilerplate. Examples can be found in t-{oidmap, hashmap}.
However, the scepticism was that use of this pattern reduces readability and it makes manual verifying harder. However, there are still not concrete guidelines as to
the most idiomatic way to write unit tests.

## So what is the plan ahead?

The plan is to get feedback on 't-oidset' from the mentors and post it to the list in the coming week. And to also iterate on and respond to the reviews
on the patches already on the list, but not yet in 'next'. Apart from that, hopefully work on 'lib-repo'.

Thanks and see ya' next week!
