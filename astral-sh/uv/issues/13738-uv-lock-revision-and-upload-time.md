```yaml
number: 13738
title: "`uv.lock` revision and upload time"
type: issue
state: closed
author: mattyoungberg
labels:
  - question
assignees: []
created_at: 2025-05-30T17:54:14Z
updated_at: 2025-05-30T18:16:05Z
url: https://github.com/astral-sh/uv/issues/13738
synced_at: 2026-01-12T16:01:36Z
```

# `uv.lock` revision and upload time

---

_@mattyoungberg_

### Question

I'm trying to understand the difference in lock output between mine and a coworker's computer that clutters our VCS whenever we bump dependencies in our project.

When `uv` locks and produces the `uv.lock` file, there are two things my uv will do differently compared to my coworker:

1. The top-level lockfile key `revision` is set to 1.
2. Every node describing an artifact from a PyPI doesn't include the `upload-time` key.

I'm on uv 0.7.8, running on Ubuntu LTS 24.04 x86_64. This seems to be the "normal" thing many of the boxes I'm working with produce.

My coworker will lock the same exact `pyproject.toml`, but his will uniquely do the following:

1. The top-level lockfile key `revision` is set to 2.
2. Every node describing an artifact from a PyPI includes the `upload-time` key.

He's running uv 0.7.2 (481d05d8d 2025-04-30) on MacOS's Darwin 23.2.0 arm64.

We have a project that's highly sensitive to dependency changes, and so we manually review the lockfile. For some reason, my coworker's commits generate a bunch of noise in the diff from having a seemingly later lockfile type despite being on an earlier version of `uv`.

I'm struggling to find a description of the contents of a `uv` lockfile such that I can perhaps figure out where these differences are coming from. Can anyone explain it to me?

### Platform

Darwin 23.2.0 arm64

### Version

uv 0.7.2 (481d05d8d 2025-04-30)

---

_Label `question` added by @mattyoungberg on 2025-05-30 17:54_

---

_Comment by @charliermarsh on 2025-05-30 17:57_

The `upload-time` field was added in v0.6.15. Are you certain that you're on uv v0.7.8?

---

_Comment by @zanieb on 2025-05-30 17:59_

You're on different uv versions, in which a latter version added a new field to the file. The revision number is used to detect such changes at deserialization time. As Charlie said, it sounds like you're not actually on the later version? See https://github.com/astral-sh/uv/pull/12968

---

_Comment by @mattyoungberg on 2025-05-30 18:02_

Yep! User error, as always. I had *just* upgraded to the latest version before making sure this really was a problem; I was on 0.6.2 before. I didn't have anything to bump for my test, though, and so I'm guessing when I ran `uv lock`, it elected to leave the lockfile alone? Or some other user-error thing I'm not accounting for.

I just provided it a library to bump, and all of a sudden I'm getting  the #2 revision and the upload-time keys.

Thank you!

---

_Comment by @zanieb on 2025-05-30 18:05_

Yeah if we don't detect project changes I don't think we'll even attempt to update the lockfile, though maybe we should when the revision changes.

---

_Closed by @mattyoungberg on 2025-05-30 18:16_

---
