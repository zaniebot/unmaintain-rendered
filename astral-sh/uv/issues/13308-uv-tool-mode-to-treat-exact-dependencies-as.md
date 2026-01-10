---
number: 13308
title: "uv tool: mode to treat exact dependencies as overridden"
type: issue
state: open
author: tjni
labels:
  - enhancement
assignees: []
created_at: 2025-05-06T02:26:33Z
updated_at: 2025-07-17T14:49:24Z
url: https://github.com/astral-sh/uv/issues/13308
synced_at: 2026-01-10T01:25:31Z
---

# uv tool: mode to treat exact dependencies as overridden

---

_Issue opened by @tjni on 2025-05-06 02:26_

### Summary

Would it be possible to add a flag to `uv tool` that infers exact dependencies in published packages to also be dependency overrides?

**Motivation**

I'm also open to other strategies. My use case is that I am trying to publish a wheel that pins its dependencies for use as a stable CLI via `uv tool`, like what is requested in https://github.com/astral-sh/uv/issues/8729. I have taken inspiration from the thread to add at build time a "pinned" extra with all direct and transitive dependencies pinned using ==.

When one of the dependencies is either (1) yanked or (2) violates a constraint, invoking the package through `uv tool` fails. During development, we can control this using `override-constraints`, but this doesn't seem to be possible yet through `uv tool`.



### Example

_No response_

---

_Label `enhancement` added by @tjni on 2025-05-06 02:26_

---

_Comment by @konstin on 2025-05-06 08:50_

The closest easy option we currently have is exporting constraints and passing them during tool installation:

```
uv export --no-emit-workspace --no-hashes -o requirements-frozen.txt
uv build
uv tool install -c requirements-frozen.txt dist/foo-0.1.0-py3-none-any.whl 
```

---

_Comment by @tjni on 2025-05-06 10:54_

Thank you for responding! I considered this but it requires a separate mechanism to distribute the dependency overrides.

---

_Comment by @EternityForest on 2025-07-12 22:33_

Is there a reliable script to just modify the wheel with those exported requirements?

---

_Comment by @tjni on 2025-07-17 14:49_

What I have done to work around the "overrides" part of the problem is fetching the package metadata using the pypi_simple package, parsing the requirements, and creating an overrides file based on them.

The "yanked" portion of the problem for me was better captured and reported in https://github.com/astral-sh/uv/issues/13569.

---
