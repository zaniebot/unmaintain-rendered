```yaml
number: 15752
title: Replace fs2 by fs4
type: pull_request
state: closed
author: ponchofiesta
labels: []
assignees: []
draft: true
base: main
head: replace-fs2-with-fs4
created_at: 2025-09-09T14:25:06Z
updated_at: 2025-09-10T07:53:21Z
url: https://github.com/astral-sh/uv/pull/15752
synced_at: 2026-01-12T16:11:55Z
```

# Replace fs2 by fs4

---

_@ponchofiesta_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR replaces the crate fs2 by fs2.

*Why?*

Crate fs2 is unmaintained for a long time now and has unfixed issues. Especially it doesn't build on AIX, which is the reason I started fixing it.

*How?*

I replaced fs2 by fs4. fs4 is fork of fs2. And I adapted fs4 related code having incompatibilities.

## Test Plan

<!-- How was it tested? -->
- I built it on Windows and AIX only.
- I did not test the artifacts.


---

_Comment by @j178 on 2025-09-09 14:27_

Since [File::lock](https://doc.rust-lang.org/stable/std/fs/struct.File.html#method.lock) is stable in Rust 1.89, I think we can wait when we've bumped MSRV to 1.89 and remove fs2.

---

_Comment by @zanieb on 2025-09-09 15:02_

If that's all we're using it for I would generally prefer to wait rather than audit a new dependency. 

---

_Converted to draft by @konstin on 2025-09-09 15:16_

---

_Comment by @ponchofiesta on 2025-09-10 07:24_

@zanieb OK, I already created a Branch removing fs2 and useing std::fs::File methods. But this would update Rust to 1.89. Would a PR be accepted or is this something you do internally with more critical relations?

---

_Comment by @konstin on 2025-09-10 07:30_

That would be great! There's one caveat, our N-2 minimal Rust policy, so we have to wait until the release of 1.91.0 stable to actually merge it.

---

_Comment by @ponchofiesta on 2025-09-10 07:53_

OK, we will see. Closing this in favor of #15764

---

_Closed by @ponchofiesta on 2025-09-10 07:53_

---
