---
title: "GSoC: Week 2"
date: 2024-06-09
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

During this week, I sent [`t-oidtree`](https://lore.kernel.org/git/20240608165731.29467-1-shyamthakkar001@gmail.com/) patch to the mailing list. In the review process,
Junio noticed some cases which would not be checked by the new unit tests, which can potentially be
identified by the old helper. Therefore, I reiterated on that patch and sent a v2. The review process
is still ongoing, but hopefully it will be good to go in the next week.

Apart from this, I resurrected one of my old patches, [`t-oid-array`](https://lore.kernel.org/git/20240223193257.9222-1-shyamthakkar001@gmail.com/). With the addition of `lib-oid.h`,
`t-oid-array` migration will become much simpler.

Interestingly enough, when I rebased the old `t-oid-array` patch I linked above on the latest master, it suddenly started segfaulting.
Some crude debugging revealed that _oid_array_lookup()_ was the culprit. And _oid_array_lookup()_ is a wrapper around _oid_pos()_, which is a generic
binary search helper which accepts any table datastructure and an access function to access the objects in that table datastructure. And apparently since [ps/undecided-is-not-necessarily-sha1](https://github.com/git/git/commit/a60c21b7206fff1a6ab561e29ac7312c437d2592),
when the repository is not initialized, _the_hash_algo_ is kept _NULL_, where as before we used to fallback to SHA1. And _the_hash_algo_ is used by _oid_pos()_
to determine the _rawsz_ of the queried _object_id_, but we currently do not initialize a repository for the unit tests to use, so the_hash_algo is NULL and thus comes the segfault.
An example test which demonstrates this behavior can be seen [here](https://github.com/spectre10/git/commit/0cdc2f409f126972fc3f77364566de31671f04e5). And the CI for that
test can be seen [here](https://github.com/spectre10/git/actions/runs/9430757729).

A primitive fix would be to remove the use of _the_hash_algo_:
```diff
diff --git a/hash-lookup.c b/hash-lookup.c
index 9f0f95e2b9..b2cb5cf1bd 100644
--- a/hash-lookup.c
+++ b/hash-lookup.c
@@ -65,7 +65,7 @@ int oid_pos(const struct object_id *oid, const void *table, size_t nr,
 	if (nr != 1) {
 		size_t lov, hiv, miv, ofs;
 
-		for (ofs = 0; ofs < the_hash_algo->rawsz - 2; ofs += 2) {
+		for (ofs = 0; ofs < hash_algos[oid->algo].rawsz - 2; ofs += 2) {
 			lov = take2(fn(0, table), ofs);
 			hiv = take2(fn(nr - 1, table), ofs);
 			miv = take2(oid, ofs);
```
with this fix all the tests pass, but... they are not displaying the full picture! Because what is the guarantee that the algo wouldn't
be _GIT_HASH_UNKNOWN_, in which case the _rawsz_ would be ZERO and we would get an infinite loop if we don't _return_/_break_ from the loop.
It could be that we always _return_/_break_ from the loop, but we wouldn't want to rely on such behavior. Currently in the places where _oid_pos()_
is used in the codebase, many of them don't seem to set the oid->algo field.
This suspicion is confirmed with:
```diff
diff --git a/hash-lookup.c b/hash-lookup.c
index 9f0f95e2b9..a5dedb10e6 100644
--- a/hash-lookup.c
+++ b/hash-lookup.c
@@ -65,7 +65,10 @@ int oid_pos(const struct object_id *oid, const void *table, size_t nr,
 	if (nr != 1) {
 		size_t lov, hiv, miv, ofs;
 
-		for (ofs = 0; ofs < the_hash_algo->rawsz - 2; ofs += 2) {
+		if (oid->algo == GIT_HASH_UNKNOWN)
+			die("algo field not set in object_id");
+
+		for (ofs = 0; ofs < hash_algos[oid->algo].rawsz - 2; ofs += 2) {
 			lov = take2(fn(0, table), ofs);
 			hiv = take2(fn(nr - 1, table), ofs);
 			miv = take2(oid, ofs);
```
With the above diff, more than 50 percent of the CI breaks, which can be seen [here](https://github.com/spectre10/git/actions/runs/9431371989). So this fix does not seem to be viable for now.

This gets even more odd when there are _object_ids_ of different algos in the array. In which case,
_oid_array_lookup()_ sometimes does not even detect the presence of some _object_ids_. Which is
to be expected given how the SHA1 _object_ids_ are placed as a group first and the SHA256 ones after them and sorted relative to each other, and
this fact is not taken into consideration when running the binary search. This is somewhat contrary to what
[this](https://lore.kernel.org/git/20230927195537.1682-2-ebiederm@gmail.com/) series claims which is "With that oid-array should be safe to use with different kinds of
oids simultaneously." And the "somewhat" part is that while we can store multiple oids of different hash algos
in the oid_array, its methods (i.e. _oid_array_lookup()_) are not yet optimized for such a scenario. But we don't have to
test with _object_ids_ of different hash algos simultaneously, so this is not an issue (for now).

## So what is the plan ahead?

With the discovery of the above, a temporary fix can be to manually initialize the_hash_algo:

```c
int cmd_main(int argc UNUSED, const char **argv UNUSED)
{
  the_hash_algo = hash_algos[GIT_HASH_SHA1];
  ...
}
```

So, the plan is to do this unless I encounter some other tangential problem with doing this
(cause I didn't see anywhere in the codebase where _the_hash_algo_ is manually assigned like this.)
And I believe that using a global state like _the_hash_algo_ in oid-array.h is quite odd, as it is supposed to be an independent
data structure library, but since there are many callers of _oid_array_lookup()_/_oid_pos()_ which don't set the _object_id->algo_, this
seems a necessity. Anyhow, after finishing `t-oid-array`, I plan to start working on `lib-repo` and other potential unit test migrations.

Well, enough rambling for now. See ya' next time!

