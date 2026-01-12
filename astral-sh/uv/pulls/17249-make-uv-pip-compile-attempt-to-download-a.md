```yaml
number: 17249
title: "Make `uv pip compile` attempt to download a specified `--python-version` if it can."
type: pull_request
state: open
author: EliteTK
labels:
  - enhancement
assignees: []
base: main
head: tk/pip-compile-missing-py-4
created_at: 2025-12-29T12:59:39Z
updated_at: 2026-01-09T21:32:15Z
url: https://github.com/astral-sh/uv/pull/17249
synced_at: 2026-01-12T16:12:40Z
```

# Make `uv pip compile` attempt to download a specified `--python-version` if it can.

---

_@EliteTK_

## Summary

I believe this mostly addresses #16709. Now specifying a simple version with `--python` or specifying a version using `--python-version` will result in the specified version getting downloaded with a fallback to the previous behaviour if the download fails for some transient reason or if downloads are disabled.

The behaviour of how `--python` gets treated as `--python-version` if a "simple version" is specified is kept. This means that `--python 3.7` turns into a soft requirement. This seems at odds with how other similar parts of UV work, but there seem to be quite a few tests which test for this specific behaviour and I think this is best saved for a separate issue.

## Test Plan

I added a test case which would previously fall back to the default interpreter and warn about it.

---

_Label `enhancement` added by @EliteTK on 2025-12-29 12:59_

---

_@EliteTK reviewed on 2025-12-29 13:03_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1492 on 2025-12-29 13:03_

There's a question here regarding if we should instead just report these errors and continue instead of propagating them upstream, as this is kind of a breaking change.

---

_@EliteTK reviewed on 2025-12-29 13:04_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1511 on 2025-12-29 13:04_

Should we use the same strategy when downloading?

---

_Review requested from @zanieb by @EliteTK on 2025-12-29 13:05_

---

_@zanieb reviewed on 2026-01-09 20:08_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1492 on 2026-01-09 20:08_

I think it seems reasonable to fail on error as long as we don't fail on things like... offline mode being enabled?

---

_@zanieb reviewed on 2026-01-09 20:08_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1511 on 2026-01-09 20:08_

That seems technically correct, though I can't say it's definitely worth it without seeing the code.

---

_@EliteTK reviewed on 2026-01-09 20:46_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1492 on 2026-01-09 20:46_

In this specific context, `downloads_enabled` is passed the combined management preference, download preference, and offline state. So this block won't get ran in the offline case.

---

_@EliteTK reviewed on 2026-01-09 20:47_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1511 on 2026-01-09 20:47_

I think it would just be a matter of shoving https://github.com/astral-sh/uv/pull/17249/changes/BASE..37d8cb6fd77ec752c265060db26b92c7584fa78c#diff-d02a01db770da7d68941d9e67624483d066f95310e5c1835dc0edc23f9af6e5cL1469 in a function and then calling it twice...

---

_@EliteTK reviewed on 2026-01-09 20:48_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1511 on 2026-01-09 20:48_

I don't know why github let me make a link and then produced whatever broken thing that was...

I mean the code above in the `if downloads_enabled` block.

---

_@zanieb reviewed on 2026-01-09 21:31_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1492 on 2026-01-09 21:31_

I guess the concern is like... if you wifi is off?

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1511 on 2026-01-09 21:32_

Yeah, I think it's only the error handling that might be annoying? I think you should try it.

---

_@zanieb reviewed on 2026-01-09 21:32_

---
