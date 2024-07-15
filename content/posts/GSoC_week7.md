---
title: "GSoC: Week 7"
date: 2024-07-15
description: ""
type: post
weight: 25
showTableOfContents: false
---

## What did I do in this period?

Here's a quick overview of the progress this week,
- [t-oidset](https://github.com/spectre10/git/commit/68e8ee1c7aa1ca7d1996498518bd9a0366372f33): reiterated based on the feedback from mentors and ready to send to the list
- [t-hashmap](https://lore.kernel.org/git/20240711235159.5320-1-shyamthakkar001@gmail.com/): sent v3 (v2 was already in 'seen')

Other than that, when I introduced 'unit-tests/lib-oid' along with the migration of 't-oidtree',
I was unaware of the fact that Git also had support for configuring the build with _CMake_. There was also no mention
of it in the Documentation about building Git from source. Therefore, when adding 'lib-oid' I updated the Makefile
but forgot to update the 'contrib/buildsystems/CMakeLists.txt'. And the reviewers also couldn't catch it during the
review process. Thus, [Johannes Schindelin](https://github.com/dscho) (alias Dscho, Git for Windows maintainer) sent a [patch](https://lore.kernel.org/git/pull.1761.git.1720816450344.gitgitgadget@gmail.com/)
addressing the issue, which subsequently fixed the building of 'lib-oid' using CMake. 

It seems that this method of building Git only works reliably for Windows. And the CI component which builds using
_CMake_ is only enabled for the 'git-for-windows/git' repository and not for anyone else.

Overall, this was a bit of a slow week in terms of coding, as a couple of my patches are already on the list waiting for a
review or are blocked because of another series, so I want to wait for the reviews of those patches before sending new ones.
Here's a list of above described patches:
- [t-urlmatch-normalization](https://lore.kernel.org/git/20240628125632.45603-1-shyamthakkar001@gmail.com/)
- [t-oid-array](https://lore.kernel.org/git/20240703034638.8019-2-shyamthakkar001@gmail.com/) (blocked by ['rs/unit-tests-test-run'](https://lore.kernel.org/git/85b6b8a9-ee5f-42ab-bcbc-49976b30ef33@web.de/))

## So what is the plan ahead?

In the coming week, I'll hopefully send 't-oidset' to the list and start working on 'lib-repo' and using 'lib-repo',
convert 'helper/test-revision-walking' to the new framework.

Thanks and see ya' next week!

