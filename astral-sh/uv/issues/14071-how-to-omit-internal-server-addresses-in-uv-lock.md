```yaml
number: 14071
title: How to omit internal server addresses in uv.lock
type: issue
state: closed
author: andreas-vester
labels:
  - question
assignees: []
created_at: 2025-06-16T09:37:08Z
updated_at: 2025-06-16T10:22:00Z
url: https://github.com/astral-sh/uv/issues/14071
synced_at: 2026-01-12T16:01:42Z
```

# How to omit internal server addresses in uv.lock

---

_@andreas-vester_

### Question

I am working behind a corporate firewall. We are using an internal PyPI mirror to install third-party Python libraries.

The internal server addresses will be included in ``uv.lock``. However, I don't want to reveal these internal addresses when publishing an open source project on GitHub.

I am wondering what is best practice in this situation?
- Add ``uv.lock`` to my ``.gitignore``?
- Remove the internal addresses manually and perform some kind of sanity check?
- Or is there any ``uv`` related function to hide these addresses?

### Version

uv 0.7.13

---

_Label `question` added by @andreas-vester on 2025-06-16 09:37_

---

_Comment by @konstin on 2025-06-16 10:19_

There's currently no good workaround for this unfortunately, we're working on better support for proxy URLs (https://github.com/astral-sh/uv/pull/11782).

---

_Comment by @konstin on 2025-06-16 10:21_

Tracking in #6349

---

_Closed by @konstin on 2025-06-16 10:22_

---
