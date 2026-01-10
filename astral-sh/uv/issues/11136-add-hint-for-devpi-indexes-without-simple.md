```yaml
number: 11136
title: "Add hint for DevPi indexes without `/+simple`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2025-01-31T17:37:24Z
updated_at: 2025-02-17T04:20:37Z
url: https://github.com/astral-sh/uv/issues/11136
synced_at: 2026-01-10T03:50:31Z
```

# Add hint for DevPi indexes without `/+simple`

---

_Issue opened by @zanieb on 2025-01-31 17:37_

### Summary

Without the `/+simple` suffix, we'll report no versions on devpi indexes, e.g.:

- https://github.com/astral-sh/uv/issues/10567
- https://github.com/astral-sh/uv/issues/11135

We may be able to sniff it is a devpi index with `<h1><a href="https://<host>/">devpi</a></h1>` and hint the fix to the user.

This is an alternative to adding support as described in https://github.com/astral-sh/uv/issues/4907

### Example

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of package and you require package, we can conclude that the requirements
      are unsatisfiable.

hint: It looks like you are using a DevPi index but did not suffix the index URL with `/+simple` which is
      required for uv to use the index.
```

---

_Label `enhancement` added by @zanieb on 2025-01-31 17:37_

---

_Label `error messages` added by @zanieb on 2025-01-31 17:38_

---

_Comment by @charliermarsh on 2025-01-31 17:45_

Good idea.

---

_Comment by @zanieb on 2025-01-31 18:04_

Seems kind of painful to thread through, but might not be that bad.

---

_Comment by @charliermarsh on 2025-01-31 21:54_

It's probably hard to thread through the hint, but could we fail entirely when we read from this URL, rather than making it a hint?

---

_Comment by @zanieb on 2025-01-31 21:56_

I'm not sure how much I trust the heuristic I shared, we might need something clearer if we're going to fail hard.

---

_Comment by @charliermarsh on 2025-01-31 21:58_

We can probably use (or misuse) `IndexCapabilities` for this. It's how we show the 401 errors, etc.

---

_Comment by @charliermarsh on 2025-02-01 00:03_

It... turns out that `devpi` sniffs the user agent to determine if a request is coming from something that "looks like" an installer: https://github.com/devpi/devpi/blob/18a53a169865f770defa2932d69b75dbf4f869a4/server/devpi_server/views.py#L75.

So if you have `devpi` running locally per the quickstart, this works:

```
curl http://testuser:123@localhost:3141/testuser/dev/flask -H "Accept: application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html;q=0.01" --user-agent "uv/1.0"
```

But this does not:

```
curl http://testuser:123@localhost:3141/testuser/dev/flask -H "Accept: application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html;q=0.01"
```

uv was [added in July](https://github.com/devpi/devpi/pull/1045). So newer versions should "just work", but older versions do not work automatically.

---

_Comment by @zanieb on 2025-02-16 23:43_

Should we do anything then? Or just leave this as-is?

---

_Comment by @charliermarsh on 2025-02-17 03:29_

I was leaning towards doing nothing and closing out. Wdyt?

---

_Closed by @zanieb on 2025-02-17 04:20_

---
