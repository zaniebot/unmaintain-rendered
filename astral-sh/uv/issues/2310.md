```yaml
number: 2310
title: "Question: How to handle multiple private extra indexes?"
type: issue
state: closed
author: atti92
labels:
  - registry
assignees: []
created_at: 2024-03-08T22:43:10Z
updated_at: 2024-04-05T17:40:29Z
url: https://github.com/astral-sh/uv/issues/2310
synced_at: 2026-01-10T05:40:32Z
```

# Question: How to handle multiple private extra indexes?

---

_Issue opened by @atti92 on 2024-03-08 22:43_

I decided to create a new issue, because I think the currently present issues don't exactly match my problem, and ask for different solutions:
 - https://github.com/astral-sh/uv/pull/2135
 - https://github.com/astral-sh/uv/issues/2205 (also this is marked as duplicate for some reason?)
 - https://github.com/astral-sh/uv/issues/171

Since I couldn't find a workaround, or any suggestion on how to handle this, I am writing down my own pain.

I am mainly interested in, how to handle when you have 3 indexes:
- dev: All dev hash wheels go in there
- staging: All RC versions are published here.
- release: Contains release versions.

This means there should be no exact conflicts, but they complement each other.


requirements/pyproject contains a mix of packages from all of those indexes, usually hard specified by some automatism, that updates the requirements/pyproject files. 
During install we have to add all indexes.

We tried to switch over to uv for performance reasons, but this specific case is not supported yet. Resolution stops at the first index where the package name exists, and just throws an error, saying it's impossible.


After reading through some of the issues and discussion I came to the following 2 possible proposals/questions:
- Do you plan to allow treating all indexes equal with an opt-in flag in the future?
- Or is it possible to ask for a solution where the resolution jumps to the next index when it's not possible to resolve with the first found index? 

This may still be a duplicate, sorry if it is, but I thought it worth just writing this down.

---

_Comment by @BurntSushi on 2024-03-09 01:36_

> Or is it possible to ask for a solution where the resolution jumps to the next index when it's not possible to resolve with the first found index?

I think if this were implemented as written, it would quickly go exponential, right? In the case where a resolution doesn't exist (which isn't totally uncommon), you'd need to every combination of each package in each index.

> Do you plan to allow treating all indexes equal with an opt-in flag in the future?

I don't think we're planning on this at present, principally because this is the thing that can cause dependency confusion problems. I suppose in theory some kind of explicit opt-in flag could mitigate the impact of the dependency confusion problem, but I don't know off-hand how invasive of a change that is. (That is, supporting both priority ordering and "get everything from all indexes and given priority to nothing and provide no guarantees about which index is used." It _might_ not be that bad.)

> During install we have to add all indexes.

Can you say more about this? Why?

---

_Comment by @korhojoa on 2024-03-09 07:05_

>> During install we have to add all indexes.
>
> Can you say more about this? Why?

This is somewhat overlapping in my use case (#2205) so I'll gladly answer for that bit,
Say you have package A, which depends on package B, which depends on package C.
Package A is in development, so it is found in development index.
Package B has a released version, but it is not yet compatible with what is developed in package A, so it exists in both release and staging. Staging has the version compatible with Package A under development.
Package C has versions in release repository.

So we end up with:
- dev
  - Package A 0.1.0.dev1
- staging
  - Package B 2.0.0.a1
- release
  - Package B 1.0.0
  - Package C 1.0.0
 
 This wouldn't be a problem, but if you now have candidates in staging, or more development versions in dev, you can't install the production versions you want to test with your development version.
like:
 
- dev
  - Package A 0.1.0.dev1
  - Package B 4.1.0.dev1
  - Package C 5.2.0.dev1
- staging
  - Package B 3.7.0.a1
  - Package C 5.1.0.a1
- release
  - Package B 4.0
  - Package C 5.0

This isn't my problem, but it's adjacent, so I understand how it would be nice to be able to at least allow a "let me resolve the packages from other repositories, even though I know it is a vulnerability"-flag.

Now consider that you have multiple repositories that you have to install from, where the versions available vary, but the  package(s) you need are only available from a particular repository that includes an arbitrary version of it's dependencies in it, then you have #2205.

The current workaround is to have multiple rounds of installing with different package indexes, but that doesn't always pan out (if the dependencies line up in a bad way, you can end up with older versions of packages unless you manually fix up your requirements files). Unfortunately that workaround is not deployable so this is local testing only so far.

---

_Label `registry` added by @charliermarsh on 2024-03-09 14:44_

---

_Comment by @atti92 on 2024-03-09 23:02_

> Can you say more about this? Why?

@korhojoa Pretty much nailed the answer.


> The current workaround is to have multiple rounds of installing with different package indexes, but that doesn't always pan out

Actually I don't know how do you do multiple rounds of installs. 
We use strict versions for internals (using: https://github.com/renovatebot/renovate), so adding only 1 index fails instantly because:
-  `Package A==0.1.0.dev1` is only in `dev`, 
- while `Package B==3.7.0.a1` is only in `staging`. 

And we even have multiple levels of dependencies, where Package A has it's own sub dependencies coming from the same indexes.


---

_Comment by @korhojoa on 2024-03-10 07:00_

> > Can you say more about this? Why?
> 
> @korhojoa Pretty much nailed the answer.
> 
> > The current workaround is to have multiple rounds of installing with different package indexes, but that doesn't always pan out
> 
> Actually I don't know how do you do multiple rounds of installs.

Looser requirements, that's pretty much the "if the dependencies line up in a bad way" cop-out in my sentence.
In general if you have satiable dependencies for production versions, then install the production "requirements layer" first, then the staging layer and finally the development layer. This would let you have a setup with packages that are compatible, but if strict versioning is in use and they don't have the same version requirements in lower layers, it could be a problem.
Try it, it could work for you.

---

_Comment by @atti92 on 2024-04-05 17:38_

I think the `--index-strategy` flag solves this question.

---

_Closed by @charliermarsh on 2024-04-05 17:40_

---
