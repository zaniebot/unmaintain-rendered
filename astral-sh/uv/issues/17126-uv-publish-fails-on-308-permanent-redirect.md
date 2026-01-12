```yaml
number: 17126
title: "`uv publish` fails on 308 Permanent Redirect"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-12-13T15:35:00Z
updated_at: 2025-12-15T15:46:18Z
url: https://github.com/astral-sh/uv/issues/17126
synced_at: 2026-01-12T16:02:44Z
```

# `uv publish` fails on 308 Permanent Redirect

---

_@zanieb_

### Summary

e.g.

```
error: Failed to publish `dist/ruff-0.1.0-py3-none-any.whl` to https://test.pypi.org/legacy
  Caused by: Upload failed with status code 308 Permanent Redirect. Server says: 308 An upload was attempted to /legacy but the expected upload URL is /legacy/ (with a trailing slash)
```

Should we just follow the redirect here?

### Platform

macOS

### Version

0.915

### Python version

_No response_

---

_Label `bug` added by @zanieb on 2025-12-13 15:35_

---

_Label `question` added by @zanieb on 2025-12-13 15:35_

---

_Assigned to @konstin by @zanieb on 2025-12-13 15:35_

---

_Comment by @dijor0310 on 2025-12-14 20:38_

Hey! I'd be happy to give this a try if no one else has started yet.

---

_Comment by @konstin on 2025-12-14 20:59_

The problem is that after a POST request, getting a redirect means we should send a GET request (https://en.wikipedia.org/wiki/Post/Redirect/Get). If we do that, we get a 200 OK on the GET request to the correct endpoint, without having actually upload the distribution (`curl -v https://test.pypi.org/legacy/` shows a 200 OK).

https://github.com/astral-sh/uv/blob/7ad441a0bdca9d02e2e7199364e12e0ace9c6f19/crates/uv-publish/src/lib.rs#L1078-L1087

This is notably specified for 301, 302 and 303, while 307 and 308 may be retried. The publish code isn't compatible with `RedirectClientWithMiddleware` as the request isn't cloneable, the uv publish code currently just doesn't retry after redirect.

https://github.com/seanmonstar/reqwest/blob/9edbd2e00b9b752e851cac0374f7aa1034beca85/tests/redirect.rs#L8-L128

@woodruffw may know more about the security implications of those retry behaviors.

---

_Comment by @woodruffw on 2025-12-15 01:44_

There are two separate things here, right?

1. If the index returns an HTTP 308 for the user-specified upload URL, we should probably follow that direct unconditionally and retry the POST. I can't think of a security risk with that, separately from the "normal" security risk with redirects where the origin either does an insecure downgrade or redirects to a malicious different origin. I guess we should probably make sure reqwest doesn't do HTTPS->HTTP redirect downgrades by default 😅 

2. More generally, I think we should probably _always_ retry POST requests that fail, regardless of any intermediating redirects. To do that we might need to stop depending on reqwest's own redirect-handling logic, since the P/R/G pattern is more relevant for user agents (= browsers) than machine-to-machine requests.

---

_Comment by @zanieb on 2025-12-15 14:27_

Regarding (1) I think it'd be fine to check if we're on the same realm and just error if not.

---

_Comment by @konstin on 2025-12-15 15:20_

Is that security item? I could otherwise imagine a registry redirecting let's say from `registry.company.com` to `upload.company.com`.

---

_Comment by @zanieb on 2025-12-15 15:21_

I'd consider it a security item, yeah. If someone redirects to a new realm it seems fine to fail and have the user update the URL if they want it to work.


---

_Label `bug` removed by @konstin on 2025-12-15 15:46_

---

_Label `question` removed by @konstin on 2025-12-15 15:46_

---

_Label `enhancement` added by @konstin on 2025-12-15 15:46_

---
