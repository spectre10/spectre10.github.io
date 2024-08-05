---
title: "GSoC: Week 8, 9 and 10"
date: 2024-08-05
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

Well, to start off, I have been a bit busy with some IRL stuff over the past two weeks. So, I have been a bit on and off on doing programming.
And one of my mentors, Christian, was also on vacation during week 8, so I did not do much during that period also. But, as things have settled
down and the merge window is open again, I am back to my previous working capacity. Here's a quick overview of what I did during this period:

- [t-hashmap](https://lore.kernel.org/git/20240803133517.73308-2-shyamthakkar001@gmail.com/): sent v5 (v4 was already in 'seen')
- [t-oid-array](https://lore.kernel.org/git/20240803132206.72166-1-shyamthakkar001@gmail.com/): sent v2
- [t-urlmatch-normalization](https://lore.kernel.org/git/20240628125632.45603-1-shyamthakkar001@gmail.com/): had some discussions on the list on v1
- [t-oidset](https://github.com/spectre10/git/commit/68e8ee1c7aa1ca7d1996498518bd9a0366372f33): locally ready
- [t-revision-walking](https://github.com/spectre10/git/commit/b3a594fc70a87d2b011c2f1bdd2c8a477d98c4fe): work ongoing

The v5 of 't-hashmap' would be good to merge to 'next' as it addresses all the reviews from the previous versions. The v2 of 't-oid-array' is yet to be
merged to 'seen' and it is waiting on acknowledgements from the reviewers. And I've been meaning to send out 't-oidset', but since my other patches
are still waiting for reviews, I don't want to over-burden the reviewers, so I am still sitting on it for a few weeks.

There was an interesting question which arose from the discussions on 't-urlmatch-normalization', where basically this patch imposes a new restrictions
on running unit-tests where they can only be run from the 't/' and 't/unit-tests/bin' directory ('t/unit-tests/bin' is for running unit tests via the 'test-tool').
This restriction is similar to what we already have for end-to-end tests. This restriction is due to the fact that we have to interact with external test files
during unit testing (i.e. read data from a test file), and it would be hard to construct the path to those test files if we were to allow running unit test binaries from anywhere. The discussion
is still ongoing on how to handle this, and I'll soon send a v2 of the 't-urlmatch-normalization' as well.

Other than that, I've been working on 'lib-repo', which can be used to setup a repository and return a 'struct repository *' populated with
a test repository which can used in the unit tests. In the earlier version of this I used to populate 'the_repository' using setup_git_directory(),
however since the use of 'the_repository' is soon to be deprecated, I switched to using the user supplied 'struct repository *'. However,
in the unit tests which require the use of a repository, many of them use 'the_repository' or 'the_hash_algo' implicitly. By implicitly, I mean
that some function takes a 'struct repository *' as an argument, but it still uses 'the_repository'/'the_hash_algo' in its code path. This recently
happened while writing 't-revision-walking'. Take a look at this snippet:

```c
static void run_revision_walk(struct repository *r)
{
	struct rev_info rev;

	repo_init_revisions(r, &rev, NULL);
	setup_revisions(argc, argv, &rev, NULL);
	...
}
```

Here we populate 'rev' via _repo_init_revisions()_, which subsequently stores the repo we pass (r) in _rev.repo_. But, _setup_revisions()_
still expects _the_repository_ /_the_hash_algo_ to be initialized (otherwise there will be segfault).

## So what is the plan ahead?

My first priority would be to get 't-hashmap', 't-oid-array' and 't-urlmatch-normalization' merged. After that I'll send
't-oidset' and 't-revision-walking' to the list (not in the next week though).

Thanks and see ya' next week!
