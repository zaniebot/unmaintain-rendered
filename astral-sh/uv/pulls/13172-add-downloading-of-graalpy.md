```yaml
number: 13172
title: Add downloading of GraalPy
type: pull_request
state: merged
author: timfel
labels: []
assignees: []
merged: true
base: main
head: timfel/add-graalpy-downloading
created_at: 2025-04-28T09:26:49Z
updated_at: 2025-05-07T10:00:37Z
url: https://github.com/astral-sh/uv/pull/13172
synced_at: 2026-01-12T16:10:35Z
```

# Add downloading of GraalPy

---

_@timfel_

## Summary

This adds GraalPy download metadata so that `uv python install graalpy` works. See https://github.com/astral-sh/uv/issues/13114 

## Test Plan

The existing integration test was changed to test this functionality.


---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:634 on 2025-04-28 09:45_

These checksum requests are actually sequential, right?

---

_@j178 reviewed on 2025-04-28 09:45_

---

_Review comment by @timfel on `crates/uv-python/fetch-download-metadata.py`:634 on 2025-04-28 09:52_

Not sure tbh 😅 I figured httpx is going to sent off the requests immediately so we get `n` requests in flight in parellel and then I handle them in batches of `n` before sending the next `n` requests.

---

_@timfel reviewed on 2025-04-28 09:52_

---

_Review requested from @Gankra by @konstin on 2025-04-28 09:52_

---

_Review requested from @jtfmumm by @konstin on 2025-04-28 09:57_

---

_@jtfmumm reviewed on 2025-04-28 09:58_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:367 on 2025-04-28 09:58_

The overall comment is confusing now. I'm interpreting it to mean that the GraalPy executable is always `graalpy`, and otherwise, if it's Windows, the executable is `python.exe`. Is that correct?

---

_Review comment by @jtfmumm on `.github/workflows/ci.yml`:1252 on 2025-04-28 10:04_

This should be `--managed-python` instead of `--python-preference only-managed` now.

---

_@jtfmumm reviewed on 2025-04-28 10:04_

---

_@timfel reviewed on 2025-04-28 10:51_

---

_Review comment by @timfel on `crates/uv-python/src/managed.rs`:367 on 2025-04-28 10:51_

I see the confusin comment, it's `graalpy.exe`, I'll change the comment

---

_Review comment by @timfel on `.github/workflows/ci.yml`:1252 on 2025-04-28 10:52_

Changed

---

_@timfel reviewed on 2025-04-28 10:52_

---

_@j178 reviewed on 2025-04-28 11:33_

---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:634 on 2025-04-28 11:33_

I think in this loop it's sending one request in one iteration, wait for it to finish, and then send the next one. So that’s sequential.

Instead, we could send requests in parallel using a TaskGroup, just like we did in the CPython example here: 
https://github.com/astral-sh/uv/blob/b33a19689c2d39f9bf2bccb37de91b33420906b3/crates/uv-python/fetch-download-metadata.py#L321-L331.

---

_Review comment by @timfel on `crates/uv-python/fetch-download-metadata.py`:634 on 2025-04-28 13:14_

I went with asyncio.gather because it's much shorter - let me know if this is ok:
https://github.com/astral-sh/uv/pull/13172/files#diff-067e233b291998aa086f6fdd3ea4015249604942bd5a18f11252ddce0e5a7dafR632

---

_@timfel reviewed on 2025-04-28 13:14_

---

_Review comment by @Gankra on `crates/uv-python/src/managed.rs`:365 on 2025-04-28 13:32_

empty string?

---

_Review comment by @Gankra on `crates/uv-python/src/managed.rs`:372 on 2025-04-28 13:33_

more empty string?

---

_Review comment by @Gankra on `.github/workflows/ci.yml`:1182 on 2025-04-28 13:36_

do we want to *stop* testing not-uv-managed graalpy's? (genuine question)

---

_@Gankra approved on 2025-04-28 13:37_

This all *looks* right to me using the pypy implementation for comparison.

---

_@zanieb reviewed on 2025-04-28 13:44_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1182 on 2025-04-28 13:44_

Definitely not.

---

_Comment by @zanieb on 2025-04-28 13:45_

Thanks for contributing!

I'm a _little_ hesitant to add GraalPy download support. We'd have to maintain it in perpetuity, and haven't really gotten asked for it by a significant number of users.

---

_@timfel reviewed on 2025-04-28 13:53_

---

_Review comment by @timfel on `crates/uv-python/src/managed.rs`:365 on 2025-04-28 13:53_

Yes, GraalPy does not encode a version in the executable.

---

_@timfel reviewed on 2025-04-28 13:54_

---

_Review comment by @timfel on `crates/uv-python/src/managed.rs`:372 on 2025-04-28 13:54_

Yes, GraalPy has neither variants nor a `w` suffix.

---

_Review comment by @timfel on `.github/workflows/ci.yml`:1182 on 2025-04-28 14:00_

ah, ok, I looked at how it's done for PyPy and adapted this test to that one. But I now see there's a `system-test-pypy`, I copied that one to `system-test-graalpy` to check that.

---

_@timfel reviewed on 2025-04-28 14:00_

---

_Comment by @timfel on 2025-04-28 14:01_

> Thanks for contributing!
> 
> I'm a _little_ hesitant to add GraalPy download support. We'd have to maintain it in perpetuity, and haven't really gotten asked for it by a significant number of users.

I only saw https://github.com/astral-sh/uv/issues/13114 this morning and thought it'd be a good project to procrastinate on. No hard feelings if you want to reject it :-)

(Although tbh, and not sure if a pro or a con, but I don't see PyPy having much activity compared to GraalPy either)

---

_Comment by @paugier on 2025-04-28 19:50_

Thanks a lot @timfel!

@zanieb GraalPy is really a great and young project and its usage should increased, especially if it can be easily installed with UV. Moreover, it would increase consistency of UV since PyPy and GraalPy are similar projects in terms of popularity and potential :slightly_smiling_face: 

---

_Comment by @Gankra on 2025-04-28 20:03_

@timfel you're *on* the GraalPy team right?

At least in my books this kind of thing is easier to stomach when the developers of the runtime are explicitly providing the support for it. Like we don't need to worry about y'all breaking this logic for fetching releases.

---

_Comment by @timfel on 2025-04-28 20:38_

> @timfel you're _on_ the GraalPy team right?

Yes, I'm on the GraalPy team. We would make sure that this keeps working (we're running tests for uv, virtualenv, tox and more packages already in our own CI as well to catch any problems early). And if you need us to change things down the line we'd be happy to.

---

_Comment by @henryiii on 2025-04-30 02:35_

cibuildwheel 3.0 will add GraalPy support, by the way. Maybe it will be a bit more common once it's easier to build wheels for. Though that was the argument for pypy, too.

---

_@j178 reviewed on 2025-04-30 03:04_

---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:576 on 2025-04-30 03:04_

By default, this pulls in just one page of releases, which is 30 items. Does it make sense to only keep the latest 30 releases for Graalpy?

---

_@timfel reviewed on 2025-04-30 12:26_

---

_Review comment by @timfel on `crates/uv-python/fetch-download-metadata.py`:576 on 2025-04-30 12:26_

Yes, GraalPy releases 2 major and 4 patch releases a year, and our support window is only 2 years so we will not support many releases back anyway, picking up 30 releases is plenty I'd say.

---

_@konstin reviewed on 2025-04-30 12:56_

---

_Review comment by @konstin on `crates/uv-python/fetch-download-metadata.py`:576 on 2025-04-30 12:56_

Can you add an inline comment about that?

---

_Comment by @Gankra on 2025-04-30 19:55_

(download-metadata-minified.json is now deleted so you can remove the changes made to it in this PR)

---

_Review comment by @timfel on `crates/uv-python/fetch-download-metadata.py`:576 on 2025-05-05 06:29_

Done

---

_@timfel reviewed on 2025-05-05 06:29_

---

_@zanieb approved on 2025-05-06 16:02_

---

_Merged by @zanieb on 2025-05-06 16:02_

---

_Closed by @zanieb on 2025-05-06 16:02_

---

_Branch deleted on 2025-05-07 10:00_

---
