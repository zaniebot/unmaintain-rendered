```yaml
number: 5089
title: "Cache downloaded wheel when range requests aren't supported"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/stream
created_at: 2024-07-16T01:56:06Z
updated_at: 2024-07-16T13:21:48Z
url: https://github.com/astral-sh/uv/pull/5089
synced_at: 2026-01-12T16:06:38Z
```

# Cache downloaded wheel when range requests aren't supported

---

_@charliermarsh_

## Summary

When range requests aren't supported, we fall back to streaming the wheel, stopping as soon as we hit a `METADATA` file. This is a small optimization, but the downside is that we don't get to cache the resulting wheel...

We don't know whether `METADATA` will be at the beginning or end of the wheel, but it _seems_ like a better tradeoff to download and cache the entire wheel?

Closes: https://github.com/astral-sh/uv/issues/5088.

Sort of a revert of: https://github.com/astral-sh/uv/pull/1792.


---

_Review requested from @konstin by @charliermarsh on 2024-07-16 02:19_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-16 02:21_

---

_Label `performance` added by @charliermarsh on 2024-07-16 02:21_

---

_Marked ready for review by @charliermarsh on 2024-07-16 02:21_

---

_Comment by @charliermarsh on 2024-07-16 02:24_

I'm sort of torn on this.

---

_Comment by @zanieb on 2024-07-16 04:21_

Would it be a pain to use a heuristic like... only finish downloading if it's greater than some percentage into the file? If we find a metadata entry 10% into the file it seems excessive to unconditionally download the whole wheel when we're trying many versions.

---

_Comment by @morotti on 2024-07-16 09:27_

> We don't know whether `METADATA` will be at the beginning or end of the wheel, but it _seems_ like a better tradeoff to download and cache the entire wheel?

METADATA are at end of the wheel, as per PEP 407. https://peps.python.org/pep-0427/#recommended-archiver-features

I vaguely recall there was a similar discussion in pip to try to only download metadata (I can't find the thread anymore). They checked many packages and only found one exception to confirm the rule, some google packages had the metadata at the start (around tensorflow if I recall well), because google used their own build system that did its own thing. I think they fixed it a while back.



---

_@konstin approved on 2024-07-16 10:44_

---

_Comment by @charliermarsh on 2024-07-16 13:07_

@zanieb - Perhaps... It would be harder to implement for sure. Given that this is not the happy path and PEP 407 recommends putting the archive at the end anyway, I'm inclined to just move forward with it.


---

_Merged by @charliermarsh on 2024-07-16 13:21_

---

_Closed by @charliermarsh on 2024-07-16 13:21_

---

_Branch deleted on 2024-07-16 13:21_

---
