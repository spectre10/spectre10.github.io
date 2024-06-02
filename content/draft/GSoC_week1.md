---
title: "GSoC: Week 1"
date: 2024-06-02
description: ""
type: post
weight: 25
showTableOfContents: true
---
## What did I do in this period?

As I talked about in my previous blog, I countinued some of the patches of Achu Luma (Outreachy Intern).
And to give a brief overview of the progress,

 - [t-strcmp-offset](https://lore.kernel.org/git/20240519204530.12258-3-shyamthakkar001@gmail.com/) (Co-authered-by: [Achu Luma](https://github.com/achluma)) (master)
 - [t-hash](https://lore.kernel.org/git/20240529080030.64410-1-shyamthakkar001@gmail.com/) (Co-authered-by: [Achu Luma](https://github.com/achluma)) (seen)
 - [t-example-decorate](https://lore.kernel.org/git/20240528125837.31090-1-shyamthakkar001@gmail.com/) (seen)

The `t-hash` patch has been acked by all the reviewers and will soon be merged into 'next' according to the latest
'What's cooking in git.git'. And there is ongoing discussion/review taking place in `t-example-decorate`, but it
will probably be good to go in about a week.

Apart from these, I worked on porting `helper/test-oidtree.c` along with `t0069-oidtree.sh`. That is not yet posted on the mailing
list, but can be seen in my [repo](https://github.com/spectre10/git/commit/d5bae493a6608265d6a3d876c7cec1373e6844ba).

In my last blog, I talked about introducing a test-lib-functions.c library for utility functions, well if you take a look
at the `t-oidtree` migration I linked above, that introduces something similar called lib-oid. The methods in that library are also similar
to what I talked about in my previous blog. Namely _get_oid_arbitrary_hex()_, which converts arbitrary hex string to _object_id_.
Apart from `t-oidtree`, this will also be useful when we port `helper/test-oid-array.c` and `t0064-oid-tree.sh`.

## What is the plan ahead?

Currently I am planning on hopefully sending `t-oidtree` patches to the mailing list in the next week. Due to the introduction of
lib-oid, I think this will probably take a bit longer to merge because there will be more discussions and iterations. In the meantime, I am planning
on working on two things, first is to migrate `helper/test-oid-array.c` to the unit testing framework. This will be based on `t-oidtree` patch,
as the lib-oid will be needed for `t-oid-array`.

The second one would be to plan for something like `lib-repo`. This will be responsible for setting up a repository for the unit tests
to use. And it would also contain methods similar to the ones we have in `test-lib-functions.sh` like _test_commit()_ etc. Many of the unit tests
currently make use of a repository, so the addition of `lib-repo` will make the transition to the new framework easier.

I experimented locally for this one. Although setting up a repo can be done easily via `init_db():setup.c` with something like:

```c
#include "setup.h"

int test_setup_repo(void)
{
	const char *git_dir = "unit-tests/.git";
	const char *git_dir_parent = strrchr(git_dir, '/');
	char *rel = xstrndup(git_dir, git_dir_parent - git_dir);
	git_work_tree_cfg = real_pathdup(rel, 1);

	set_git_work_tree(git_work_tree_cfg);
	if (access(get_git_work_tree(), X_OK))
		die_errno(_("Cannot access work tree '%s'"),
			  get_git_work_tree());

	free(rel);
	return init_db(git_dir, NULL, "", GIT_HASH_SHA1,
		       REF_STORAGE_FORMAT_FILES, "main", 0, INIT_DB_QUIET);
}
```

But... for methods like `test_commit()`, there is no straight forward interface like `init_db()`. I looked at
`builtin/commit.c`, but there was much more going on than just an interface for the cli. I think there should be
a straight forward interface for creating commits in `commit.h` like there is for _init-db_. Hopefully when
libification efforts progress we can use an api for creating commits. But until then, we may just have to rely on spawning commands
or using a shellscript.

See ya' next week!
