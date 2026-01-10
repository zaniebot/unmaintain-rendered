```yaml
number: 13347
title: "What, exactly, does `uv lock --U <package>==<version>` do?"
type: issue
state: closed
author: scrooks
labels:
  - question
assignees: []
created_at: 2025-05-08T16:36:00Z
updated_at: 2025-05-08T19:06:23Z
url: https://github.com/astral-sh/uv/issues/13347
synced_at: 2026-01-10T03:41:47Z
```

# What, exactly, does `uv lock --U <package>==<version>` do?

---

_Issue opened by @scrooks on 2025-05-08 16:36_

### Question

I was operating under the assumption that if I did `uv lock -P foo==3.21`, _only_ foo will be updated.  If foo changed its dependency specifications to require bar>=1.34 but my lockfile has bar at 1.32, I was assuming uv would respect the lock and tell me "nope, can't do it, and here's why."  And then I could decide it's okay and add `bar==1.33` to my `uv lock -P` list.

Now a colleague tells me that he sees _foo_ dependencies also updated in the lockfile.  Really!  That's not really "locked" then.  And I started wondering how the update is chosen.  Let's say foo has the bar dependency as `bar>=1.34`, bar is currently at 1.37, and our lockfile says bar is locked to 1.32.  Is uv going to pick bar 1.34, 1.37, or ...?

Can you confirm that if foo has the bar dependency as `bar>=1.32`, bar is currently at 1.37, and our lockfile says bar is locked to 1.32, uv will *not* update bar to 1.37 (since the locked 1.32 version should still work for foo)? It will be really strange if that's not true.

I'm not sure I like that locked doesn't really mean locked for `-P`. But maybe I just need to hear a different point of view to understand.

### Platform

macOS

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

---

_Label `question` added by @scrooks on 2025-05-08 16:36_

---

_Comment by @zanieb on 2025-05-08 16:47_

>  ... was assuming uv would respect the lock and tell me "nope, can't do it, and here's why." And then I could decide it's okay and add bar==1.33 to my uv lock -P list.

During an upgrade, we will _prefer_ the locked version of other packages, but if constraints change such that resolution will _fail_, we will upgrade other packages.

> Can you confirm that if foo has the bar dependency as bar>=1.32, bar is currently at 1.37, and our lockfile says bar is locked to 1.32, uv will not update bar to 1.37 (since the locked 1.32 version should still work for foo)?

Yes, this is true.

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv add --raw-sources anyio
Using CPython 3.13.3
Creating virtual environment at: .venv
Resolved 4 packages in 2ms
Installed 3 packages in 7ms
 + anyio==4.9.0
 + idna==3.10
 + sniffio==1.3.1
❯ uv tree
Resolved 4 packages in 1ms
example v0.1.0
└── anyio v4.9.0
    ├── idna v3.10
    └── sniffio v1.3.1
❯ uv lock --upgrade-package anyio==4.8.0
Resolved 4 packages in 83ms
Updated anyio v4.9.0 -> v4.8.0
❯ uv tree
Resolved 4 packages in 1ms
example v0.1.0
└── anyio v4.8.0
    ├── idna v3.10
    └── sniffio v1.3.1
❯ uv lock --upgrade-package idna==3.9
Resolved 4 packages in 67ms
Updated idna v3.10 -> v3.9
❯ uv tree
Resolved 4 packages in 0.90ms
example v0.1.0
└── anyio v4.8.0
    ├── idna v3.9
    └── sniffio v1.3.1
❯ uv lock --upgrade-package anyio
Resolved 4 packages in 54ms
Updated anyio v4.8.0 -> v4.9.0
❯ uv tree
Resolved 4 packages in 1ms
example v0.1.0
└── anyio v4.9.0
    ├── idna v3.9
    └── sniffio v1.3.1
```

> I'm not sure I like that locked doesn't really mean locked for -P.

I mean, they're locked within bounds that can satisfy the constraints given to the resolver. The lockfile is there for review of changes. It'd be really annoying if you had to enumerate all transitive dependencies that need to change versions on each lockfile update. If you want this, you should probably pin exact versions in your `pyproject.toml` instead (see #12946).

---

_Comment by @scrooks on 2025-05-08 16:59_

Okay, thanks for the quick response.

> It'd be really annoying if you had to enumerate all transitive dependencies that need to change versions on each lockfile update.

Yes, but when we cut a branch in preparation for a release, we would really, really like to lock it down and be extremely specific about what's updated. Perhaps there should be another option like `--strict-lock`.  Yeah, we can look at the lockfile diff before and after, but I want non-technical people to be able to see uv spit out a nice explanation: "can't update because of this and this." And then the decision can be purposefully made:  "We really want this update to foo bad enough that we're willing to take the update to bar also, so let's add bar to that `-P` list and try again."  Annoying? Yes. Purposeful and completely informed about updates? Yes. A trade-off I would definitely make. In theory, updates to a release branch will hopefully be minimal so the annoyance won't have to be tolerated too often.

I don't think we should have to update `pyproject.toml` in the release branch for this. I've been promoting the benefits of the lockfile and how we don't have to touch `pyproject.toml` when we cut a release branch.

---

_Comment by @scrooks on 2025-05-08 17:06_

Your example is interesting.  I see a big difference between `uv lock -U anyio` and `uv lock -U "anyio==4.9.0"`.  In the first case, you're saying, "I want to move to the latest version of anyio," and I would expect all the anyio dependencies to be updated, if necessary, despite whatever my lockfile says.  In the second case, that feels very specific and I would not expect literally anything but anyio to be updated.  But that's probably because I'm currently living in a "cutting a release" world. 😄 

---

_Comment by @zanieb on 2025-05-08 17:17_

> Yes, but when we cut a branch in preparation for a release, we would really, really like to lock it down and be extremely specific about what's updated.

Can you share more about this use-case? I don't fully understand, i.e., why are you bumping versions? Are you talking about deploying an application?

> In the first case, you're saying, "I want to move to the latest version of anyio," and I would expect all the anyio dependencies to be updated, if necessary, despite whatever my lockfile says.

I'm not sure I see the difference. In both cases, you're just specifying that `anyio`'s version should be changed. I don't agree that `anyio` without constraints should have a different affect on our preferences for transitive dependencies.

It sounds like you want an `--only-upgrade-package` flag or an `--upgrade-mode strict` option. These are very technically feasible, I'm just not sure we've been asked for them before. cc @konstin who is working on some changes in the package upgrade area.

---

_Comment by @scrooks on 2025-05-08 18:04_

> Can you share more about this use-case? 

Day-to-day we work in dev branches and merge to master when features are ready. Master should always be working and up-to-date.  We have numerous repos for components that are used in our application.  We often do `uv lock -U` in our application repo to ensure we have the latest of everything.

Now it's time to cut a release, meaning create a version that is ready to go to users.  We clone the current master and consider it locked for release. Everyday work continues in master and in all the other component repos.

Extensive testing happens for the release (we are but one part of a big application made of many pieces). Testing reveals that there's a bug here, or we need to modify this feature there, whatever. At that point, _we want to do the minimal necessary to fix the problem_.  Extensive testing has already happened and our stuff is working in conjunction with everyone else's stuff.  Minimal, but necessary, changes are the name of the day.

So we cherry pick that commit over there.  Or we decide one of the packages we use (also written by us) needs to be updated, but we only need the two bug fixes that happened after we cut a release, not the minor update.  So we carefully update to take the version with only the bugfixes.  Ideally, no other package, even a sub-dependency package, is updated because of this so that the blast radius is minimized, and we start testing again.  This repeats until the entire application is acceptable, and it's officially declared a product in client hands.

Sometimes released products have problems too, and require a hotfix. Those are even more precise, where we will likely get the team making the package that needs a hotfix to prepare a version that ensure the changes are very, very specific to the update needed, and give us a hotfix version (which usually means a fourth level in the version number, like 3.21.2.1.) We roll that hotfix in -- and nothing else! -- and hope there are no unexpected side effects.

All changes for releases are as precise and minimal as we can make them to reduce the blast radius.  So taking a package update that also updates 5 other sub-dependency packages raises alarm bells.  We have to very deliberately decide that it's okay to take all 5. Maybe instead (if it's a package we control), we ask the package team to cut a special hotfix that only needs 2 of those sub-dependency packages updated.  This is particularly true if a sub-dependecy package is used by multiple other packages!

That is why I would like to have `--only-upgrade-package` or `--upgrade-mode strict`.

---

_Comment by @zanieb on 2025-05-08 18:09_

Thanks for all the details!

---

_Comment by @zanieb on 2025-05-08 18:12_

Let's track that as a feature request at https://github.com/astral-sh/uv/issues/13352

---

_Closed by @zanieb on 2025-05-08 18:12_

---

_Comment by @konstin on 2025-05-08 19:06_

I can give some background on the resolution process: The resolver gets a set of requirements (the `pyproject.toml` or workspace) and a set of preferences (`uv.lock`). The resolver traverses the dependencies in a predefined order. If you have lockfile compatible with the requirements, the resolver is idempotent: We do a traversal through the dependency tree and assign each packages its preference, we see that there are no conflicts, were done and write out the lockfile exactly the same. (There's some optimizations in here to not actually do that for perf, but the code itself totally supports this).

When a package is set to `--upgrade-package foo`, we remove the preference for foo. Instead, when it comes to picking a version for foo, we select the highest version possible for foo. In the subsequent steps, we still try select the preferences, and usually, we only update foo. It can happen though that the bar preference is incompatible with the new foo, we select another version of bar, other packages aren't compatible with bar, etc., and then you can see bigger lockfile updates.


One option of controlling the updates is the `uv lock -P foo==3.21` you mentioned: By doing the minimal update (that #13352 would figure out automatically) you need, if you know that all other packages are compatible with your update step, uv preserves the other versions.

---
