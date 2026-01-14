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
updated_at: 2026-01-14T17:47:29Z
url: https://github.com/astral-sh/uv/pull/17249
synced_at: 2026-01-14T18:48:19Z
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

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1563 on 2025-12-29 13:04_

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

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1563 on 2026-01-09 20:08_

That seems technically correct, though I can't say it's definitely worth it without seeing the code.

---

_@EliteTK reviewed on 2026-01-09 20:46_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1492 on 2026-01-09 20:46_

In this specific context, `downloads_enabled` is passed the combined management preference, download preference, and offline state. So this block won't get ran in the offline case.

---

_@EliteTK reviewed on 2026-01-09 20:47_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1563 on 2026-01-09 20:47_

I think it would just be a matter of shoving https://github.com/astral-sh/uv/pull/17249/changes/BASE..37d8cb6fd77ec752c265060db26b92c7584fa78c#diff-d02a01db770da7d68941d9e67624483d066f95310e5c1835dc0edc23f9af6e5cL1469 in a function and then calling it twice...

---

_@EliteTK reviewed on 2026-01-09 20:48_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1563 on 2026-01-09 20:48_

I don't know why github let me make a link and then produced whatever broken thing that was...

I mean the code above in the `if downloads_enabled` block.

---

_@zanieb reviewed on 2026-01-09 21:31_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1492 on 2026-01-09 21:31_

I guess the concern is like... if you wifi is off?

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1563 on 2026-01-09 21:32_

Yeah, I think it's only the error handling that might be annoying? I think you should try it.

---

_@zanieb reviewed on 2026-01-09 21:32_

---

_@EliteTK reviewed on 2026-01-14 16:04_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1492 on 2026-01-14 16:04_

I've made it so that any errors which come out of fill, find, or fetch are warned about (minimally) as opposed to being propagated, unless the request is for any version or a default version, in which case I feel like it's appropriate to reply with a download error if downloads were enabled.

Let me know what you think.

---

_@EliteTK reviewed on 2026-01-14 16:04_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1563 on 2026-01-14 16:04_

All done, I think the error handling didn't turn out too bad.

---

_@zanieb reviewed on 2026-01-14 16:05_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 16:05_

What kind of error cases are you imagining here?

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 16:06_

In what case should we not just hard-fail?

---

_@zanieb reviewed on 2026-01-14 16:06_

---

_@EliteTK reviewed on 2026-01-14 16:06_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 16:06_

Network errors specifically. See the test for an example.

---

_@EliteTK reviewed on 2026-01-14 16:26_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 16:26_

To expand, various errors from `PythonInstallation::fetch` (which include network errors due to e.g. WiFi being out) and two kinds of errors from `PythonDownloadRequest::fill`:
* we can't figure out the libc variant
* UV_PYTHON_<impl>_BUILD contains invalid UTF-8

I considered if the fill errors should be hard-fail.

Mainly I was trying to avoid the situation you described here https://github.com/astral-sh/uv/pull/17249#discussion_r2677677267 which I specifically tested and is what lead to this path.

I think it's possibly an idea to narrow things down to specifically transient network errors and nothing else.

Or we can stick with the original approach of just failing hard - but it's kind of a breaking change in that sense.

---

_@zanieb reviewed on 2026-01-14 16:39_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 16:39_

I guess my thinking is that if we fail here we also won't succeed on the subsequent attempt with another Python version request

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 16:40_

I agree it's nice for it not to be breaking though, thanks for providing the additional context.

We might want to encode some of that in a comment.

---

_@zanieb reviewed on 2026-01-14 16:40_

---

_@EliteTK reviewed on 2026-01-14 16:45_

---

_Review comment by @EliteTK on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 16:45_

Re failing twice: I would agree it would _probably_ fail but I don't think we can assume it _will_ fail. At least that was my thinking in that regard. But it's also kind of a remote possibility to be really trying to cater for it.

Would you like me to skip re-attempting a download on the without-patch-number path if the verbatim path fails (except if it fails with `NoDownloadFound` in which case it is worth re-trying)?

---

_Comment by @EliteTK on 2026-01-14 16:47_

Heh... I just realised, what about if there's no default python version? We should probably _also_ attempt to download in that final case too...

---

_@zanieb reviewed on 2026-01-14 17:47_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1553 on 2026-01-14 17:47_

I probably would skip subsequent attempts, but I don't feel strongly.

---

_Comment by @zanieb on 2026-01-14 17:47_

Yeah that makes sense

---
