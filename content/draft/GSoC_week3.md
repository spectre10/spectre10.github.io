---
title: "GSoC: Week 3"
date: 2024-06-17
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

During this week, I reiterated on `t-oid-array` which I mentioned in my last blog.
In my last blog, I also mentioned that without the_repository->hash_algo being initialized,
it will cause segfault when using _oid_array_lookup()_. To make reviewing easier and to track the removal
of _the_repository_, [Patrick](https://gitlab.com/pks-gitlab) introduced a macro USE_THE_REPOSITORY_VARIABLE, and without defining
it, we cannot access _the_repository_ global variable. In the same series, Patrick also ameneded
`helper/test-oid-array.c` and `t0064-oid-array.sh` to work without having an actual repository and
manually initializing _the_hash_algo_ via _repo_set_hash()_. I am waiting for this series ([ps/use-the-repository](https://lore.kernel.org/git/cover.1718347699.git.ps@pks.im/)) to get
merged to next, and after that I will rebase on top of it to sent `t-oid-array` migration to the list.

Apart from this, I also migrated `helper/test-oidmap.c` to `unit-tests/t-oidmap.c`, and will send to the list
in the coming week. Both `t-oid-array` and `t-oidmap` rely on `lib-oid` introduced with `t-oidtree`.

In sum, it was a quite week and I did not face any difficulties.

## So what is the plan ahead?

After sending `t-oid-array` and `t-oidmap` to the list, I will start working on `lib-repo` (probably).
Apart from that `oidset.h` can also take advantage of the new `lib-oid`, but currently there are no existing
tests for it. If I have some spare time, I'll write some from scratch as the API is similar to `oid-array.h` and `oidmap.h`, so
it would not take much time. But I'll discuss with mentors first before doing so.

Thanks and see ya' next time!
