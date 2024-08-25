---
title: "GSoC: Final Report"
date: 2024-08-25
description: ""
type: post
weight: 25
showTableOfContents: false
---

![GSoC logo](https://developers.google.com/open-source/gsoc/resources/downloads/GSoC-logo-horizontal-800.png)

\
Hey everyone,\
GSoC 2024 is now officially over. This blog presents my final report on my project for
'Git'. Below are some preliminary details:

- Name: Ghanshyam Thakkar
- Organization: [Git](https://github.com/git/git/)
- Project: [Move existing tests to a unit testing framework](https://summerofcode.withgoogle.com/programs/2024/projects/e9C4rhrv)
- Proposal: [View/Download](https://docs.google.com/document/d/1wmosedy-UKd_mhAAjccv1qETO1q00s-npYj2m6g-Hd0/edit?usp=sharing)
- Mentors: [Christian Couder](https://gitlab.com/chriscool) from GitLab, [Kaartic Sivaraam](https://github.com/sivaraam)

***

## Summary:

Back in late last year, a new unit testing framework written in C, e137fe3b29 (unit tests: add TAP unit test framework, 2023-11-09), was merged into
Git. Before such a framework, we used make a CLI interface for every library/API we wanted to test, and use this interface to call for
the output for a given input via shell scripts. This was because end-to-end tests were also written in shell scripts and Git didn't have
any other way for doing testing before this new framework was merged. This unit testing framework is helpful in understanding how each
function call behaves and also makes it easy to have complex custom setup for a particular library or a function of a library, which
was rather difficult when using shell scripts.

Therefore, my project involved converting these existing shell script based unit tests to the new unit testing framework written solely in C. 

## Work

I have posted 9 migrations of the tests to mailing list and 7 of them are currently merged into 'master' or 'next'. 2 of them
are currently under discussion/review on the mailing list. And I've done another experimental migration which I will talk about
in the Future Work section. Also, I have not only migrated the unit test themselves but also have contributed to other areas as
well, like introducing a new library called 'lib-oid' for the unit tests to use, which generates an `object_id` with an arbitrary
hex string based on the hash algo used. And my Pre GSoC work can be seen listed in my proposal.
Below is the list of patches with the commits, mailig list discussion link and their status:

- ### t-strcmp-offset
  **Status**: Merged into Master\
  **Commits**:\
  [4d00d948ff](https://github.com/git/git/commit/4d00d948ff) (t/: port helper/test-strcmp-offset.c to unit-tests/t-strcmp-offset.c, 2024-05-20)\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240820152008.21354-2-shyamthakkar001@gmail.com/)

- ### t-example-decorate
  **Status**: Merged into Master\
  **Commits**:\
  [456b4dce4c](https://github.com/git/git/commit/456b4dce4c) (t/: migrate helper/test-example-decorate to the unit testing framework, 2024-05-28)\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240528125837.31090-1-shyamthakkar001@gmail.com/)
  
- ### t-hash
  **Status**: Merged into Master\
  **Commits**:\
  [a70f8f19ad](https://github.com/git/git/commit/a70f8f19ad) (strbuf: introduce strbuf_addstrings() to repeatedly add a string, 2024-05-29)\
  [2794932548](https://github.com/git/git/commit/2794932548) (t/: migrate helper/test-{sha1, sha256} to unit-tests/t-hash, 2024-05-29)\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240529080030.64410-1-shyamthakkar001@gmail.com/)
  
- ### t-oidtree
  **Status**: Merged into Master\
  **Commits**:\
  [ed54840872](https://github.com/git/git/commit/ed54840872) (t/: migrate helper/test-oidtree.c to unit-tests/t-oidtree.c, 2024-06-08)\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240608165731.29467-1-shyamthakkar001@gmail.com/)
  
- ### t-oidmap
  **Status**: Merged into Master\
  **Commits**:\
  [28c1c07700](https://github.com/git/git/commit/28c1c07700) (t: migrate helper/test-oidmap.c to unit-tests/t-oidmap.c, 2024-07-03)\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240703062958.23262-2-shyamthakkar001@gmail.com/)

- ### t-hashmap
  **Status**: Merged into Master\
  **Commits**:\
  [3469a23659](https://github.com/git/git/commit/3469a23659) (t: port helper/test-hashmap.c to unit-tests/t-hashmap.c, 2024-08-03)\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240803133517.73308-2-shyamthakkar001@gmail.com/)

- ### t-urlmatch-normalization
  **Status**: Merged into Master\
  **Commits**:\
    [05026637f3](https://github.com/git/git/commit/05026637f3) (t: migrate t0110-urlmatch-normalization to the new framework, 2024-08-20)\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240820152008.21354-2-shyamthakkar001@gmail.com/)
  
- ### t-oid-array
  **Status**: Posted to the list, under review\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240824170223.36080-1-shyamthakkar001@gmail.com/)

- ### t-oidset
  **Status**: Posted to the list, under review\
  **Mailing list**: [Thread](https://lore.kernel.org/git/20240824172028.39419-1-shyamthakkar001@gmail.com/)

## Future Work

Currently t-oid-array and t-oidset are not merged into 'seen', however, I am working on
getting them merged. However, earlier in the report I mentioned about an experimental migration
which I had done. That was [t-revision-walking](https://github.com/spectre10/git/commit/b3a594fc70a87d2b011c2f1bdd2c8a477d98c4fe). This unit test uses a repository for making
commits and performing a revision walk over them. The one thing which was a bit of a limiting
factor for the current framework, which prevented more migrations, was the unavailability of
a test repository. Earlier the shell script based tests had a test repository available to them
which can be setup using the `setup_git_directory()` in the 'C' part of that test. To solve for
this, I had created a new library for the unit tests to use called 'lib-repo' which created
a new repository and populated a 'struct repository *' which was passed down to it.

This, however, turned out to be not what I had envisioned. For almost all the tasks (i.e. staging, committing, etc..),
I had to spawn git sub-processes, which defeated the purpose of writing it in C. Only for creating and setting up the
repo, could I actually use library functions (i.e. init_db():setup.h). And there was also the problem of
'the_repository' global variable. So, if you do pass down a custom 'struct repository *' which is not 'the_repository',
many of the libraries which you would be testing could segfault, cause it is used in many places implicitely. This
occured to me when migrating 'helper/test-revision-walking'. So, a good feature to work on in the future
would be to provide a test repository to the unit tests.

##  Final Words

Overall, I would say that it was quite a transformative experience, from not
even having the knowledge of what a mailing list even was, to being on it and
participating on it on a regular basis, and contributing to one of the most
important open-source projects was really a great experience.

I had faced some difficulties when I started to contribute to 'Git'. The most
challanging of them all was finding the work to do. I mostly just searched the
codebase for 'TODO:', 'FIXME:' and other markers and occasionally #leftoverbits
on the list, but those provided very little context of the problem. I was
used to GitHub issues, where you can filter them based on the difficulty, areas
of the codebase affected by the issue, etc... The unorganized nature of 'Git'
was rather difficult at first and I thought that only the people working on 'Git'
as part of their $DAYJOB would have the clear understanding of what long term
technical goal they wanted to achieve while contributing. And I still somewhat
think the same today.

I also wanted to thank my mentors Christian and Kaartic for helping me during
this process and all the others who have reviewed my code or interacted with
me on the list. I have a `$DAYJOB` now, which can be attributed to my participation
in GSoC and working on 'Git'. I still plan to help out beginners who want to
contribute to 'Git', however, I will probably produce less code than I used to
for a month or two while navigating my first $DAYJOB. Although, I will still
get t-oid-array and t-oidset merged and occasionally participate on the list.
I am particularly interested in the Rust's integration in the 'Git' codebase, which is
under discussion now.

Thanks & Regards,\
Ghanshyam.
