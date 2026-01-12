```yaml
number: 15567
title: Retry streaming Python and bin download errors
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/retry-on-python-download-stream-error
created_at: 2025-08-28T09:08:44Z
updated_at: 2025-08-31T15:07:23Z
url: https://github.com/astral-sh/uv/pull/15567
synced_at: 2026-01-12T16:11:49Z
```

# Retry streaming Python and bin download errors

---

_@konstin_

When there is an error during the streaming download and unpack for Python interpreter and bin installs, we would previously fail, causing a lot of CI flakes on GitHub Actions.

The problem was that the error is not one of the extended IO errors we were previously handling, but a regular reqwest error, nested below layers of errors of other crates processing the stream, including some IO errors. We now handle nested reqwest errors, too.

This surfaced another problem: Our manual retry loop couldn't inform the retry middleware that it already performed the limit of retries, and that the middleware should not retry anymore. While too many retries are more a problem for debugging than for the user, this causes confusing error output. To work around this, we disable the retries in the client and handle all retry errors in our loop.

Fixes https://github.com/astral-sh/uv/issues/14171

---

_Label `enhancement` added by @konstin on 2025-08-28 09:08_

---

_Marked ready for review by @konstin on 2025-08-28 09:22_

---

_@zanieb approved on 2025-08-29 18:35_

---

_Merged by @charliermarsh on 2025-08-31 15:07_

---

_Closed by @charliermarsh on 2025-08-31 15:07_

---

_Branch deleted on 2025-08-31 15:07_

---
