```yaml
number: 16038
title: Retry all h2 errors
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - network
  - ci-flake
assignees: []
merged: true
base: main
head: konsti/h2-err
created_at: 2025-09-26T12:36:37Z
updated_at: 2025-10-09T13:53:15Z
url: https://github.com/astral-sh/uv/pull/16038
synced_at: 2026-01-12T16:12:04Z
```

# Retry all h2 errors

---

_@konstin_

The h2 errors, a specific type nested in reqwest errors, all look like they shouldn't happen in regular operations and should be retried. This covers all `io::Error`s going through h2 (i.e., only HTTP 2 connections).

Fixes https://github.com/astral-sh/uv/issues/15916

---

_Label `bug` added by @konstin on 2025-09-26 12:36_

---

_Label `network` added by @konstin on 2025-09-26 12:36_

---

_Label `ci-flake` added by @konstin on 2025-09-26 12:36_

---

_Comment by @zanieb on 2025-09-29 14:10_

I still am not sure this is appropriate, I think we should enumerate the cases we need to retry on. What are you using as an authoritative list of possible h2 errors?

e.g., I looked at https://docs.rs/h2/latest/h2/struct.Reason.html and we should not retry on [`HTTP_1_1_REQUIRED`](https://docs.rs/h2/latest/h2/struct.Reason.html#associatedconstant.HTTP_1_1_REQUIRED) or [`INADEQUATE_SECURITY`](https://docs.rs/h2/latest/h2/struct.Reason.html#associatedconstant.INADEQUATE_SECURITY).

---

_Comment by @konstin on 2025-09-29 19:06_

I had looked through https://github.com/hyperium/h2/blob/756345ecd8e0c3f890c862c72cc3e3f61a621dce/src/error.rs#L25-L43, but we're lacking good documentation for how these error occur in practice.

reqwest supports HTTP 1.1, but doesn't use h2 for it, so `HTTP_1_1_REQUIRED` should never occur. `INADEQUATE_SECURITY` isn't one of the common security errors such as a wrongly named cert, it occurs if HTTP 2 was implemented wrongly. I consider it very unlikely that some uses an invalid HTTP 2 for their registry and also that this shows up with uv, it's more likely that there was some network error which made it look like an invalid HTTP 2 implementation (like #15916).

It also seems that we are unintentionally retrying bad SSL certs without this PR:

```
$ UV_HTTP_RETRIES=1 uv pip install tqdm --index-url https://expired.badssl.com/
× Failed to fetch: `https://expired.badssl.com/tqdm/`
  ├─▶ Request failed after 1 retries
  ├─▶ error sending request for url (https://expired.badssl.com/tqdm/)
  ├─▶ client error (Connect)
  ╰─▶ invalid peer certificate: certificate expired: verification time 1759172489 (UNIX), but certificate is not valid after 1428883199 (330289290 seconds ago)
  help: Consider enabling use of system TLS certificates with the `--native-tls` command-line flag
```

```
$  UV_HTTP_RETRIES=1 uv pip install tqdm --index-url https://untrusted-root.badssl.com/
× Failed to fetch: `https://untrusted-root.badssl.com/tqdm/`
  ├─▶ Request failed after 1 retries
  ├─▶ error sending request for url (https://untrusted-root.badssl.com/tqdm/)
  ├─▶ client error (Connect)
  ╰─▶ invalid peer certificate: UnknownIssuer
  help: Consider enabling use of system TLS certificates with the `--native-tls` command-line flag
```

---

_@zanieb approved on 2025-10-09 13:39_

---

_Merged by @konstin on 2025-10-09 13:53_

---

_Closed by @konstin on 2025-10-09 13:53_

---

_Branch deleted on 2025-10-09 13:53_

---
