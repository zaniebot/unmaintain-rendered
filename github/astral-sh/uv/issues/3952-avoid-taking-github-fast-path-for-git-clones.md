---
number: 3952
title: Avoid taking GitHub fast path for git clones
type: issue
state: open
author: ibraheemdev
labels:
  - performance
assignees: []
created_at: 2024-05-31T22:22:56Z
updated_at: 2024-06-04T00:34:27Z
url: https://github.com/astral-sh/uv/issues/3952
synced_at: 2026-01-07T13:12:17-06:00
---

# Avoid taking GitHub fast path for git clones

---

_Issue opened by @ibraheemdev on 2024-05-31 22:22_

Avoid using [`github_fast_path`](https://github.com/astral-sh/uv/blob/a0652921fcf01ac34dbd6ceb1126ff1250bf15cf/crates/uv-git/src/git.rs#L448) for the initial clone of a repository. This would require using `git clone` instead of `git init` + `git fetch`, and using the `--branch` argument to resolve refspecs.

---

_Label `performance` added by @ibraheemdev on 2024-05-31 22:23_

---

_Comment by @charliermarsh on 2024-06-01 18:19_

What's the motivation?

---

_Comment by @samypr100 on 2024-06-01 22:12_

There were also some discussions about other approaches in #3287

---

_Comment by @ibraheemdev on 2024-06-03 13:26_

That discussion is mostly unrelated to this. The GitHub fast path primarily tells us if the commit we have is up to date, so we shouldn't need to take it for the initial clone.

---

_Comment by @charliermarsh on 2024-06-04 00:16_

Don't we need to figure out the precise commit from (e.g.) the branch / tag though? I thought we skipped this anyway if we had a precise commit already, and that the fast path was mostly for _that_ operation.

---

_Comment by @ibraheemdev on 2024-06-04 00:34_

I don't see an early return in [`github_fast_path`](https://github.com/astral-sh/uv/blob/77e93157fb829dee83ff29582e4852a3731477b9/crates/uv-git/src/git.rs#L652) unless we find the local repo is up to date, and the resulting oid is used [pretty rarely](https://github.com/astral-sh/uv/blob/77e93157fb829dee83ff29582e4852a3731477b9/crates/uv-git/src/git.rs#L494).

---
