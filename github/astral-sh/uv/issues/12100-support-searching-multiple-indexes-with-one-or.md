---
number: 12100
title: Support searching multiple indexes with one or more offline
type: issue
state: open
author: kimotorc
labels:
  - enhancement
assignees: []
created_at: 2025-03-10T17:31:40Z
updated_at: 2025-09-02T16:31:02Z
url: https://github.com/astral-sh/uv/issues/12100
synced_at: 2026-01-07T13:12:18-06:00
---

# Support searching multiple indexes with one or more offline

---

_Issue opened by @kimotorc on 2025-03-10 17:31_

### Summary

With multiple indexes, it seems that if an index is offline before the dependency is found, there will be a 'client error (Connect)' failure.  It would be nice if the search continued to the following indexes.  

The context is that I'd like uv to manage my project that can run in 3 different environments each with a different pypi index.  From each, all dependencies can be found. Of the 3 specified in the pyproject.toml, only one will be online.  

I know I can pin each package to the right index with environment markers.  However, two of the 3 environments are very similar and I'm not sure I can disambiguate between them with the available PEP508 variables.  Also, it seems like replicating the pinning for all 3 environments to all dependencies is a lot.

### Example

If I had two indexes like below, the current behavior is for uv to try to connect until the timeout is elapsed, then raise a network error.  It would be nice if instead of an error, the search continues on to the online index.

```
[[tool.uv.index]]
url = "https://example.offline.pypi.com/simple"

[[tool.uv.index]]
url = "https://pypi.org/simple"
```

---

_Label `enhancement` added by @kimotorc on 2025-03-10 17:31_

---

_Comment by @seifertm on 2025-08-12 07:02_

I would like to second the use case of having multiple environments, each of which with their own package index. 

Uv currently supports handling HTTP status codes for responses from indexes, but it completely stops the resolution process if any of the indexes are offline or DNS resolution fails. It would be great if the failure handling was extended beyond HTTP errors.

I think this is related to or a duplicate of #10462.


---

_Comment by @seifertm on 2025-08-14 13:41_

Depending on the effort, I'm cautiously optimistic that I could contribute a PR for this functionality. That is, if the maintainers consider the proposed idea valuable.

---

_Comment by @larstalian on 2025-09-02 16:31_

This feature would be very helpful as i am trying to cache dependencies in a local devpi server, and when it is not available on my network, i want to fallback to other indexes.

---
