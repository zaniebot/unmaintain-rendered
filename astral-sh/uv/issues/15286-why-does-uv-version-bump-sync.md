```yaml
number: 15286
title: "Why does `uv version --bump` sync?"
type: issue
state: open
author: fabiopicchi
labels:
  - question
assignees: []
created_at: 2025-08-14T18:36:40Z
updated_at: 2026-01-06T17:07:07Z
url: https://github.com/astral-sh/uv/issues/15286
synced_at: 2026-01-12T16:02:07Z
```

# Why does `uv version --bump` sync?

---

_@fabiopicchi_

### Question

I am trying to create a CD pipeline using `uv version --bump` however, it locks, which is not desirable in that scenario. 

My dependencies are split in groups, and there is a dev dependency which is downloaded directly from a private git repository (not a Pypi repo). However, `uv version` doesn't care about groups and tries to regenerate the lock file. Since that private dependency is specified there, it will try to fetch it and fail the workflow.

I was wondering what is the rationale behind `version` doing a full lock. It makes sense for it to bump the package's version in the lockfile (same as it does to in pyproject.toml), but I would say the lock is not necessary. If I use `--frozen` the lock file isn't updated at all, which is not desirable as any subsequent `uv run` will update the lock file with the new version, forcing the developer to commit it without having made any explicit changes.

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.7.21 (Homebrew 2025-07-14)

---

_Label `question` added by @fabiopicchi on 2025-08-14 18:36_

---

_Comment by @zanieb on 2025-08-14 18:40_

cc @Gankra 

---

_Comment by @Gankra on 2025-08-14 19:31_

(Note that there is also `--no-sync`, which edits the lock but does not sync.)

It's admittedly a pretty narrow corner-case but the concern was that a workspace can have non-trivial cyclic dependencies, such that bumping a version of your own package can actually change what version of third-party packages are installed (think: you are developing some major ecosystem package and your dev-dependencies actually depend on you).

I *think* there was also more narrow concerns of a developer running `uv version bump` and then running their project in a way that doesn't invoke `sync` and being confused why say, `mycoolapp --version` is yielding the wrong version (as whatever tooling they use to implement `--version` reads their yet-unsynced-pyproject.toml).

We could *potentially* add some kind of fast-path that detects there are no such cycles, if it's a serious issue.

---

_Comment by @fabiopicchi on 2025-08-14 19:59_

I see. `--no-sync` unfortunately doesn't work for me as it still tries to solve dependencies, apparently. The difference between `--no-sync` and `--frozen` is a bit subtle to me, but I use `--frozen` since it doesn't try to solve dependencies again. Semantically, they are the same to me.

The workspace scenario is indeed much more sophisticated than what I have. Maybe this could be triggered only if a workspace is detected?

---

_Comment by @zanieb on 2025-08-14 20:11_

`--no-sync` skips the sync portion of locking and syncing :) `--frozen` skips the locking portion.

---

_Comment by @fabiopicchi on 2025-08-14 20:15_

Btw, just to clarify my use case (I was reading the question and noticed my goal was left implicit!!!). I want the lock file to be updated, but I would like it to be updated in a more mechanical way, just be replacing my package's version as in a `str.replace` (major simplification here).

I just don't want the dependencies to be fetched again.

---

_Comment by @tigerhawkvok on 2025-08-15 18:30_

TBH I think the no-lock-no-sync version should be default. If I'm version bumping it's usually the last step before release, the absolutely last thing I want to do is have my dependencies mutate on me.

It's essentially a **tag**, not a directive. I would argue the default case should be "update all version numbers" (for both the lockfile and pyproject.toml) and a flag should be required to ".... and then sync". I understand the reversal of the logic is undesirable for the existing command, but perhaps a slight variant like `--bump-tag` could do as @fabiopicchi and I suggest?

---

_Comment by @zanieb on 2025-08-15 18:52_

> [...] is have my dependencies mutate on me.

This won't happen unless your lockfile is incompatible with the version... which would be important to know. We prefer existing versions in the lockfile. We're not invalidating that constraint.

---

_Comment by @fabiopicchi on 2025-08-15 19:13_

@zanieb I guess my main problem is that the lockfile contains dev and test dependencies which are irrelevant to my build/publish flow. I would be ok with the current behaviour if there as a way to pass `--no-groups` to `uv version`.

---

_Comment by @charlesnicholson on 2025-08-18 01:19_

Related, I hand-authored a tool that rewrites the `version=...` line in our pyproject.toml files because `uv version` started complaining about resolving things and I didn't care enough to figure out what it actually wanted. We copy the entire package to a sandbox directory for parallelism safety and tried to run `uv version` on the sandbox version and it yelled at us.

It would be very nice if there was a way to say "look, I know what I'm doing and I don't care what you think the world looks like; I just want you to update the `version=` field of this `pyproject.toml` file".

---

_Comment by @fabiopicchi on 2025-08-18 02:18_

> Related, I hand-authored a tool that rewrites the `version=...` line in our pyproject.toml files because `uv version` started complaining about resolving things and I didn't care enough to figure out what it actually wanted. We copy the entire package to a sandbox directory for parallelism safety and tried to run `uv version` on the sandbox version and it yelled at us.
> 
> It would be very nice if there was a way to say "look, I know what I'm doing and I don't care what you think the world looks like; I just want you to update the `version=` field of this `pyproject.toml` file".

... and the uv.lock file! I was actually able to achieve the behavior you described with uv version --frozen. That, unfortunately, doesn't update the uv.lock file which is a small inconvenience someone will have to take care of in a future commit.

---

_Comment by @ozancaglayan on 2026-01-06 17:07_

This is so annoying in CI/CD steps where we don't install the package or the deps but we simply want to bump the version.

---
