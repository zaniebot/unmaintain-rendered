```yaml
number: 6887
title: Make headers title case for backward compatibility
type: pull_request
state: merged
author: leiserfg
labels:
  - compatibility
  - network
assignees: []
merged: true
base: main
head: capital-case-headers
created_at: 2024-08-30T20:02:57Z
updated_at: 2024-09-03T17:28:45Z
url: https://github.com/astral-sh/uv/pull/6887
synced_at: 2026-01-12T16:07:34Z
```

# Make headers title case for backward compatibility

---

_@leiserfg_

## Summary
Http headers are supposed to be case-insensitive (RFC 2616), but there are some implementations that don't normalize them.
I noticed it while migrating to `uv`, calls to an internal registry failed. A man in the middle server helped me to find that `pip` uses Title-Case while `uv pip` uses lowercase.

## Test Plan

I tested `uv` with the same server and now it works fine.


---

_Comment by @charliermarsh on 2024-08-30 20:42_

Interesting. I did some reading and it looks like this is disabled by default both out of spec compliance and due to performance risks. I wish we could enable this selectively so it didn't affect all users, since it seems like a minority of servers need this. Maybe an env var?

---

_Label `compatibility` added by @charliermarsh on 2024-08-30 20:42_

---

_Comment by @leiserfg on 2024-08-30 21:49_

Regarding spec compliance, both are equally valid with the only difference being that correctly implemented servers will work with both but http1 servers will use the old way (Title-Case).
I don't think that changing the case of the header would have any impact on performance (I could be wrong tho). 
What I fear is that if there are other servers broken the same way, the error coming from the server does not reflect it, you will just get a 401 or worse.

---

_Comment by @leiserfg on 2024-09-02 07:53_

An example of a webserver not doing headers normalization: AWS lambda. Therefore local pypi using lambda like https://github.com/khornberg/elasticpypi will only work with uv if they handroll the normalization.

---

_Label `network` added by @zanieb on 2024-09-03 14:33_

---

_Merged by @charliermarsh on 2024-09-03 17:28_

---

_Closed by @charliermarsh on 2024-09-03 17:28_

---
