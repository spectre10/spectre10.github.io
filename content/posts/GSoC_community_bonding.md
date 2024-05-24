---
title: "GSoC: Community Bonding Period"
date: 2024-05-24
description: ""
type: post
weight: 25
showTableOfContents: false
---
## What did I do in this period?

So, the period of Community Bonding provided by GSoC allows you to get familiar
with your mentors, wider community members and plan for the Coding Period. And
I did just that. I had my first meeting with my mentors and we discussed a bunch
of things, some technical and some general advice.

Also, I reiterated on some of the previous patches which were authored by
Achu Luma (Outreachy intern) to get them merged. Here are the mailing list threads:

- [t-strcmp-offset](https://lore.kernel.org/git/20240519204530.12258-3-shyamthakkar001@gmail.com/)
- [t-hash](https://lore.kernel.org/git/20240523235945.26833-1-shyamthakkar001@gmail.com/)

Another one was t-date, which used _tzset()_ to change the timezones during runtime.
It worked perfectly on unix systems (linux, mac), but the windows tests were failing
due _tzset()_ not changing the timezones. Considering that I have no developmental
experience on windows, I have decided to not pursue this patch.

## Some ideas which have been cooking in my mind

During this time, I have been thinking about which tests to port, and I quickly
realised that if we want to port more tests, we would need something similar to
`t/test-lib-functions.sh` for the unit tests. Suppose, this would be `t/unit-tests/test-lib-functions.c`,
then we can define some commonly used utility functions and test-lib enhancements.
One of those is initializing a repo/object store for the unit tests to use.
This would allow for many of the tests such as `t0062-revision-walking` to be
ported over easily. Ofcourse if we are going to do this, we would also have make
helper functions for creating commits and creating files/folders. I am exploring
what the best way to achieve this would be. Maybe there exists some helper
functions already inside git codebase to partially achieve this, otherwise
we can always spawn commands directly.

Another idea which has been floating around in my mind is having a utility function
in test-lib-functions.c, which creates _object_id_ when given an arbitrary string of
length no more than the max hexsz of the hash algorithm used. Something like:

```c
int get_oid_loc(const char *hex, size_t sz, int hash_algo, struct object_id *oid)
{
	int i, ret;
	struct strbuf buf = STRBUF_INIT;

	if (sz > hash_algos[hash_algo].hexsz + 1) {
		test_msg("hex string bigger than maximum allowed");
		return -1;
	}

	strbuf_add(&buf, hex, sz - 1);
	for (i = sz; i <= hash_algos[hash_algo].hexsz; i++)
		strbuf_addch(&buf, hex[sz - 1]);

	ret = get_oid_hex_algop(buf.buf, oid, &hash_algos[hash_algo]);
	strbuf_release(&buf);

	return ret;
}
```
To give an overview, if we give a string 'abc', it will generate an _object_id_ with the hex
'abcccccccccccccccccccccccccccccccccccccc' for SHA-1. For '1', it will generate '1111111111111111111111111111111111111111'.
I have created a prototype implementation of t-oidtree [here](https://github.com/spectre10/git/commit/16682a964ce94dd6fd68bb355f591fd61dde2fae)
which uses this function to create `object_id`.

I believe these small utility functions will make for a better developer experience when
writing unit tests. To give an example of where these might be used, the function to generate
`object_id`, as I already described above, can be used in `t-oidtree` (for t0069-oidtree) and also in `t-oid-array` (for t0064-oid-array).
And the helper functions for initializing and using a repo can be used in t-oidmap, t-revision-walking,
etc.

## What I plan on doing next?

I have compiled a list of potential tests which can be migrated, and when the coding period
starts (27th June), I will begin migrating with the `test-example-decorate.c` helper. I have
made a implementation just for CI testing, which can be seen [here](https://github.com/spectre10/git/commit/5d7e794134699ae14ab5a2d0ce39e8d0248fb915). And if you want to follow
the development, the branches have a number suffixed to their name, which tells the
iteration number (latest being the one with the highest number).

Other than that I will discuss with the mentors on the test-lib enhancement ideas and what the
best strategy would be to implement them.

Besides all the technical stuff, a virtual meet with the wider community has also been
organized on 7th of June by my mentor, Christian. I hope to see and interact with the new
faces and other experienced developers working on Git.

If you'd like to give input on some of these ideas or discuss other ideas, my email is `shyamthakkar001@gmail.com`
or you can join the shared slack channel with me, other GSoC participants and mentors. The details of which
can be found [here](https://lore.kernel.org/git/CAP8UFD0ogL3v8LtC=DA+UBsVE2BS-ycwOjjg4wS43KsnOV5eFQ@mail.gmail.com/).

Adios!
