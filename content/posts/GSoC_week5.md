---
title: "GSoC: Week 5"
date: 2024-07-01
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

During this week, I reiterated on and addressed Phillip Wood's review on [t-oidmap](https://lore.kernel.org/git/20240619175036.64291-1-shyamthakkar001@gmail.com/T/#mcfac487a68d2847638308aa4eb4d281444f06f31) patch, which I had mentioned about
in my last blog. In the review Phillip noticed that in the legacy version of the tests, we used to iterate over the _oidmap_,
collect the _object_ids_ and sort them to compare them to the sorted input. So, Phillip suggested to do that in unit tests
as well. The v1 of the patch was only detecting that the obtained _object_id_ was given in the input or not. And in that approach, we would
also pass a duplicate _object_id_ which might have been inserted in the _oidmap_. However, to (partially) check for this,
I also added a check to compare the size of the _oidset_ at the end to make sure it matches the input size. However, Phillip noted that checking
the size to guard against duplicate entries only works if we trust _hashmap_get_size()_, so we might need to be more concrete
than that.

So, in the v2, which I sent during this week, I modified the code to detect duplicates as well. And in the v2, Junio noted that
the v2 makes the tests order dependent. This was because I was modifying the global input array to mark for the entries which
had been seen. So, when an already seen entry was detected we would know that it is a duplicate. This made the global input
array unusable after this test, so new tests would have to placed before this test. Junio suggested a simple fix to keep
a marker array ('_char seen[]_') to note which entries were already seen rather modifying the global input array. And I have
modified the patch to include Junio's suggestion.

Other than that, heres a quick overview of the progress this week,

- [t-oidmap](https://lore.kernel.org/git/20240619175036.64291-1-shyamthakkar001@gmail.com/T/#m52f057b0818d82a586616094e26049446dbfa544): sent v2 to the list -> merged into 'seen'
- [t-hashmap](https://lore.kernel.org/git/20240628124149.43688-1-shyamthakkar001@gmail.com/T/#u): sent v1 to the list
- [t-urlmatch-normalization](https://lore.kernel.org/git/20240628125632.45603-1-shyamthakkar001@gmail.com/T/#u): sent v1 to the list

And there is also [t-oid-array](https://github.com/spectre10/git/commit/79de9f4bb7a0549f5e421e65bc6c7c679aefc795) which is ready in my tree, but is waiting on '[ps/use-the-repository](https://lore.kernel.org/git/cover.1718347699.git.ps@pks.im/)' to be merged into master.

## So what is the plan ahead?

Currently, I am looking at 'helper/test-dir-iterator.c' and 'helper/test-simple-ipc.c' to see if they can
be converted or not. And I have also written new unit tests for 'oidset.h' library, which are currently in my local tree.
Other than that, I will probably look to work on 'lib-repo' as well.

Thanks and see ya' next week!
