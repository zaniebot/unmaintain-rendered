---
number: 152
title: Add support for URL dependencies (at least for wheels)
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - wish
assignees: []
created_at: 2023-10-20T05:27:59Z
updated_at: 2023-11-01T13:19:08Z
url: https://github.com/astral-sh/uv/issues/152
synced_at: 2026-01-10T01:23:03Z
---

# Add support for URL dependencies (at least for wheels)

---

_Issue opened by @charliermarsh on 2023-10-20 05:27_

- VCS:
    - Start with just git+
    - The URL must point to a source distribution
    - If the URL ends with a hash, we can use that as the cache key
    - If the URL ends with anything else, we need to resolve it to a commit, then use that as the cache key (and we need to do this lookup every time)
- URL dependencies can be a source distribution or a wheel
    - For URL dependencies, we should cache based on the HTTP semantics
    - Empirically, it seems that the URL must end in .whl to be considered a wheel; otherwise, it’s considered a source distribution
     - We could even start by not caching anything here


---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-23 04:10_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-24 19:17_

---

_Label `enhancement` added by @charliermarsh on 2023-10-24 19:18_

---

_Comment by @charliermarsh on 2023-10-26 04:50_

I need to look at how some other tools handle this (e.g., Bun, pip, Poetry).

---

_Comment by @charliermarsh on 2023-10-26 05:10_

https://github.com/oven-sh/bun/pull/1875#issuecomment-1402128002

---

_Comment by @charliermarsh on 2023-10-26 05:21_

https://github.com/pypa/pip/issues/10075

---

_Comment by @charliermarsh on 2023-10-26 05:21_

https://github.com/pypa/pip/issues/11164

---

_Comment by @charliermarsh on 2023-10-26 05:25_

Lots of good discussion in here: https://github.com/pypa/pip/pull/10564#issuecomment-1154867561

---

_Comment by @charliermarsh on 2023-10-26 05:35_

Gonna propose something here...

---

_Comment by @konstin on 2023-10-26 07:29_

Unlike pip, we should remember the url contents we installed (e.g. through an etag or another http caching mechanism), check the url every time and reinstall if they changed. For locked requirements.txt, we would need hashes.

---

_Comment by @charliermarsh on 2023-10-26 13:28_

Yeah we need a clear mechanism for this. But the pip issues also have a lot of discussion around what happens when you change from a URL dependency to a version dependency, etc.

---

_Label `wish` added by @charliermarsh on 2023-10-26 13:47_

---

_Comment by @charliermarsh on 2023-10-29 20:26_

Probably the biggest remaining "feature" (with the rest of the milestone being largely focused on testing, performance, and polish).

---

_Comment by @charliermarsh on 2023-10-30 16:56_

Another challenge here is that we need to fetch and build the distribution in order to know the version.

---

_Closed by @charliermarsh on 2023-10-30 16:56_

---

_Reopened by @charliermarsh on 2023-10-30 16:56_

---

_Comment by @charliermarsh on 2023-10-30 16:57_

We could consider attempting to parse the version from the URL, but it won't always be sufficient (and it could even be wrong).

---

_Comment by @charliermarsh on 2023-10-30 16:59_

A few other considerations:

- If there's a URL dependency that satisfies a version, and another package depends on that version in a non-URL capacity, we probably need to allow that. For example, if a user points to a `pytorch` wheel via URL, and some other package depends on `pytorch == 1.0.0`, that should be allowed as long as the version at the URL matches the requested version.
- If there are two conflicting URL dependencies, we need to reject those, even if they resolve to the same version.


---

_Comment by @charliermarsh on 2023-11-01 13:19_

Initial version is closed by https://github.com/astral-sh/puffin/pull/251.

---

_Closed by @charliermarsh on 2023-11-01 13:19_

---
