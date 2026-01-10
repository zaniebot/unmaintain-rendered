```yaml
number: 17359
title: "Allow `uv version` bumping `post` and `dev` together at the same time"
type: issue
state: open
author: jvacek
labels:
  - enhancement
assignees: []
created_at: 2026-01-08T13:34:25Z
updated_at: 2026-01-09T12:37:25Z
url: https://github.com/astral-sh/uv/issues/17359
synced_at: 2026-01-10T03:11:36Z
```

# Allow `uv version` bumping `post` and `dev` together at the same time

---

_Issue opened by @jvacek on 2026-01-08 13:34_

### Summary

`uv version --bump post --bump dev=123` is not allowed.

Gankra [mentioned](https://discord.com/channels/1039017663004942429/1207998321562619954/1458491307080159386) that this could be specifically allowed

### Example

_No response_

---

_Label `enhancement` added by @jvacek on 2026-01-08 13:34_

---

_Comment by @zanieb on 2026-01-08 14:39_

Hm I think the test case at https://github.com/astral-sh/uv/pull/17361/files#diff-72580d6331315274e60910416c0770d25ad03c437e39ad6571ddafc046d7e759R1754-R1779 is a good reason why this maybe shouldn't be allowed? I'm not sure if that bump is correct?

cc @Gankra 

---

_Comment by @konstin on 2026-01-08 14:58_

The combination is semantically odd: Post releases exist mainly fixup an existing release, they shouldn't be used for new releases, only to fix e.g. a build problem in an other production release. dev and other prereleases on the other hand exist for testing new releases that aren't production ready.

---

_Comment by @Gankra on 2026-01-08 15:23_

[This section and the next of the versioning spec](https://packaging.python.org/en/latest/specifications/version-specifiers/#post-releases) are at least pretty clear that post-releases are intended to work with beta and dev. We can draw a line and say "this is a silly-antipattern" but also it's not like it's a maintenance burden or footgun (especially since we still validate Number Go Up after applying all the operations).

---

_Comment by @Gankra on 2026-01-08 15:24_

I suppose we can appeal to:

> While they may be useful for continuous integration purposes, publishing developmental releases of pre-releases to general purpose public index servers is strongly discouraged, as it makes the version identifier difficult to parse for human readers. If such a release needs to be published, it is substantially clearer to instead create a new pre-release by incrementing the numeric component.
> 
> Developmental releases of post-releases are also strongly discouraged, but they may be appropriate for projects which use the post-release notation for full maintenance releases which may include code changes.

---

_Comment by @zanieb on 2026-01-08 15:24_

Yeah "strongly discouraged" is a little discouraging :)

---

_Comment by @zanieb on 2026-01-08 15:25_

@jvacek can you explain more about the use-case again?

---

_Comment by @jvacek on 2026-01-08 16:13_

My usecase is having a workflow for (on-demand) pushing dev versions to our private index from the feature branch's CI.

I'd like to essentially make a "dev version" of the current release, but just taking `X.Y.dev1` is technically a downgrade from `X.Y` so I cannot really do a `uv version --bump dev`.

I can't make the assumption about whether the branch is a maj/min/patch bump, or whether or not it's been bumped yet, so my thought process is that doing a post+dev bump is "a dev version of the upcoming release".

As mentioned in DC, if you have a better suggestion for this workflow, I'm all ears

---

_Comment by @zanieb on 2026-01-08 16:58_

I think the problem is that combining post and pre means it's a post version of the prerelease not the other way around?

---

_Comment by @notatallshaw on 2026-01-08 17:09_

> I think the problem is that combining post and pre means it's a post version of the prerelease not the other way around?

Yes, I think combining post and pre is unlikely to make sense for bump mechanism, but combining post and dev as OP requests is probably fine? 

The strongly discouraged  statement is interesting... I'm not sure the intent of it. If a project uses dev versions in source control to represent everything prior to a release, and they publish post releases, there must be dev release of a post releases prior to publishing said post release. I don't see how you can get around that. 

---

_Comment by @Gankra on 2026-01-08 17:21_

The document is suggesting that a post-release is expected to be so trivial that such a dev release is pointless fluff (their canonical example is "fixing the changelog"). Like, if you're doing non-trivial work that's not a post-release, that's, just a release.

I think if you want to represent "development of the next release" you should be doing `--bump patch --bump dev` (no one should ever care if you actually go from `1.0.0` to `1.0.1-dev1` to `1.1.0`. You didn't like, gaslight someone by doing that).

---

_Comment by @notatallshaw on 2026-01-08 17:30_

> The document is suggesting that a post-release is expected to be so trivial that such a dev release is pointless fluff (their canonical example is "fixing the changelog"). Like, if you're doing non-trivial work that's not a post-release, that's, just a release.

That makes opinionated assumptions about how a users release processes work which I would have been -1 if I was involved when PEP 440 was written .

For example pip's release process works like:

1. Always use a dev release
2. Run automated step that:
a. Removes the dev and commits
b. Publishes the release
c. Bumps the release number, makes dev, and commits 

So there is only ever a single commit that isn't dev and it controlled by automation, not manual, so it doesn't matter how trivial the fix is if we wanted to make it automated we'd have to have a dev of a post release.

That's my two cents anyway, this bump mechanism is ultimately uv's call. 

---

_Comment by @zanieb on 2026-01-08 17:46_

Oh sorry, I didn't realize a `dev` release was a non-prerelease segment — i.e., that it was distinct. That makes sense then.

---

_Comment by @zanieb on 2026-01-08 18:01_

See https://github.com/astral-sh/uv/pull/17361

---

_Comment by @konstin on 2026-01-08 18:05_

> Oh sorry, I didn't realize a `dev` release was a non-prerelease segment — i.e., that it was distinct. That makes sense then.

To make this even more complicated, dev release are prereleases, but they are still separate from alpha/beta/rc.

> I can't make the assumption about whether the branch is a maj/min/patch bump, or whether or not it's been bumped yet, so my thought process is that doing a post+dev bump is "a dev version of the upcoming release".

That's a kind of unsupported use case, PEP 440 assumes there is always a target version the dev version is a prerelease of (similar to the pip case), i.e. there's an intentional decision what the next version is. If you don't know the bump yet, imo updating the patch version works semantically better than a post version. I'm trying to nudge people away from post versions as they seem to be the same version, but we have to treat them as entirely distinct versions in the resolver. That makes them usually the worse compromise than just bumping the release version proper (There's also some projects who used post versions for uploading extract builds for additional platforms, which works with pip, but fails as soon as you have an universal lockfile).

---

_Comment by @notatallshaw on 2026-01-08 18:13_

I think if uv wants to discourage users from using post releases, which seems fine to me, it would help to document that somewhere and/or warn when bumping to a post release. 

Post releases do have special rules, like pre release, that are annoying when doing comparison and resolving. 

---

_Comment by @jvacek on 2026-01-09 12:37_

> I think if you want to represent "development of the next release" you should be doing --bump patch --bump dev (no one should ever care if you actually go from 1.0.0 to 1.0.1-dev1 to 1.1.0. You didn't like, gaslight someone by doing that).

My issue with that proposal is the following:

- Master is on 0.0.1
- Feature branch gets made
- Dev bumps feature branch to 0.0.2
- Dev runs CI to push dev version to index, which auto-bumps the version before build and creates 0.0.3.dev1
- Dev makes further changes on feature branch but does not push new version to CI
- Dev merges feature branch (0.0.2) to master
- CI pushes 0.0.2 to index

This means that at the end, the index has the following versions:
- 0.0.1
- 0.0.2
- 0.0.3.dev1

For me this is an issue as 0.0.3.dev1 is actually _behind_ 0.0.2. 

----

However, I suppose if the CI makes a 0.0.2.post1.dev1 instead of a 0.0.3.dev1, it is kinda just as incorrect, just in a smaller magnitude?


---
