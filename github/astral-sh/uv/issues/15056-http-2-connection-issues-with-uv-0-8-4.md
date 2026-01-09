---
number: 15056
title: HTTP/2 Connection Issues with uv 0.8.4
type: issue
state: closed
author: JonneDatanen
labels:
  - bug
assignees: []
created_at: 2025-08-04T12:54:24Z
updated_at: 2025-08-07T13:53:13Z
url: https://github.com/astral-sh/uv/issues/15056
synced_at: 2026-01-07T13:12:19-06:00
---

# HTTP/2 Connection Issues with uv 0.8.4

---

_Issue opened by @JonneDatanen on 2025-08-04 12:54_

### Summary

After upgrading from uv 0.8.3 to 0.8.4, many (maybe 50%) of CI runs fail with HTTP/2 ENHANCE_YOUR_CALM errors when connecting to our private Nexus/PyPI repository. This happens when `uv pip install` ~100 packages, and with 0.8.3 the failure rate was 0%.

If downgraded back to 0.8.3, the issues seem to disappear.

The logs are:
```
TRACE Considering retry of error: Reqwest(reqwest::Error { kind: Request, url: "https://<redacted>/repository/python/simple/markdown-it-py/", source: hyper_util::client::legacy::Error(SendRequest, hyper::Error(Http2, Error { kind: GoAway(b"too_many_internal_resets", ENHANCE_YOUR_CALM, Library) })) })
TRACE Cannot retry error: not an extended IO error
TRACE Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<redacted>")), port: None, path: "/repository/python/simple/markdown-it-py/", query: None, fragment: None }, WrappedReqwestError(Middleware(error sending request for url (https://<redacted>/repository/python/simple/markdown-it-py/)

Caused by:
    0: client error (SendRequest)
    1: http2 error
    2: connection error detected: detected excessive load generating behavior (b"too_many_internal_resets")))) }
```

I also noticed that similar issue is mentioned in: https://github.com/astral-sh/uv/issues/11636#issuecomment-3109289245

### Platform

x86_64 GNU/Linux

### Version

0.8.4

### Python version

3.9 & 3.11

---

_Label `bug` added by @JonneDatanen on 2025-08-04 12:54_

---

_Comment by @konstin on 2025-08-04 13:02_

The status "420 Enhance Your Calm" sent by the registry is a non-standard HTTP code, the correct one would be "429 Too Many Requests". We can add this status code to retrying nevertheless.

I'm surprised this regressed in 0.8.4, I can't identify any commit as a likely culprit.

---

_Comment by @charliermarsh on 2025-08-04 13:09_

@konstin -- I guess we upgraded `h2` and `hyper-util`, that's my only guess, otherwise seems odd.

---

_Comment by @konstin on 2025-08-04 13:16_

This PR seems to be the culprit, I missed it cause we usually have per-dependency update PRs: https://github.com/astral-sh/uv/pull/14899/. I looked through the changelogs of the updated HTTP crates but found nothing that looks like it would particularly cause this.

---

_Comment by @JonneDatanen on 2025-08-04 13:49_

I'm not very fluent in Rust, but this:
https://github.com/hyperium/h2/commit/69294bb21628cb381ff6637399cc728fb03d90e0

Seems to be happening in h2 between "0.4.7" and "0.4.11" (i.e. change of `cargo.lock` in the PR that @konstin was linking)

---

_Comment by @ajunior on 2025-08-05 02:28_

@JonneDatanen I've got the same problem here.

---

_Referenced in [hyperium/h2#856](../../hyperium/h2/issues/856.md) on 2025-08-05 02:44_

---

_Comment by @zanieb on 2025-08-05 02:44_

I filed https://github.com/hyperium/h2/issues/856

---

_Comment by @zanieb on 2025-08-05 02:47_

We should just revert the h2 update for now. 

---

_Referenced in [astral-sh/uv#15079](../../astral-sh/uv/pulls/15079.md) on 2025-08-05 10:26_

---

_Closed by @zanieb on 2025-08-05 11:55_

---

_Reopened by @zanieb on 2025-08-05 11:56_

---

_Comment by @zanieb on 2025-08-05 11:56_

Can someone share logs from `RUST_LOG=h2=debug` so we can see the h2 frames?

---

_Closed by @zanieb on 2025-08-05 11:56_

---

_Reopened by @zanieb on 2025-08-05 11:56_

---

_Comment by @JonneDatanen on 2025-08-05 12:40_

I can try to do it. However this is kind of a Heisenbug, when I add some logging, calls between uv and registry slows down just a bit, and the issues disappear.

---

_Comment by @zanieb on 2025-08-05 16:19_

@ajunior are you also using a Nexus repository?

---

_Comment by @zanieb on 2025-08-05 17:41_

This error is thrown client side when the server is causing a lot of stream errors, so you may also want to check in with Nexus about this.

---

_Comment by @JonneDatanen on 2025-08-06 06:04_

Quickly checking `0.8.5` seemed to fix the issue, thanks!

We will still try to figure out:
1) h2 trace
2) what is wrong with Nexus (and other related component) 

---

_Closed by @charliermarsh on 2025-08-06 11:51_

---

_Comment by @musicinmybrain on 2025-08-06 12:48_

> We should just revert the h2 update for now.

This was a reasonable thing to do, but I can’t really make the downgrade happen in Fedora where we build with system packages for Rust crates, so I’m watching https://github.com/hyperium/h2/issues/856 carefully for a possible solution.

---

_Reopened by @zanieb on 2025-08-06 13:19_

---

_Referenced in [astral-sh/uv#15111](../../astral-sh/uv/pulls/15111.md) on 2025-08-06 21:01_

---

_Closed by @zanieb on 2025-08-07 13:53_

---
