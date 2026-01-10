```yaml
number: 10030
title: Remove rule S410
type: issue
state: closed
author: ofek
labels:
  - rule
assignees: []
created_at: 2024-02-18T20:03:55Z
updated_at: 2024-02-28T17:38:57Z
url: https://github.com/astral-sh/ruff/issues/10030
synced_at: 2026-01-10T11:09:52Z
```

# Remove rule S410

---

_Issue opened by @ofek on 2024-02-18 20:03_

See this discussion https://discuss.python.org/t/status-of-defusedxml-and-recommendation-in-docs/34762

It _may_ be valid to recommend the `defusedxml` third-party package over the standard library `xml` module but the `lxml` third-party package is the de facto way to work with XML in Python and any security issues which were previously a concern have been [fixed](https://discuss.python.org/t/status-of-defusedxml-and-recommendation-in-docs/34762/5) and the `defusedxml` project even [now documents](https://github.com/tiran/defusedxml/commit/a252917a0740a624ca4078b4348b52da1e14f56a) that it is safe.

---

_Comment by @charliermarsh on 2024-02-20 16:30_

Would we need to augment the ruleset with new checks to ensure that `lxml` is being used with the appropriate defaults?

---

_Label `rule` added by @charliermarsh on 2024-02-20 16:30_

---

_Comment by @ofek on 2024-02-20 18:11_

Based on https://github.com/tiran/defusedxml#defusedxmllxml I was thinking you could check for `etree.XMLParser(...)` without an explicit `resolve_entities=False` but actually based on the documentation https://lxml.de/apidoc/lxml.etree.html#lxml.etree.XMLParser it seems the default has been changed to be safe so I think this rule should simply be removed.

---

_Comment by @charliermarsh on 2024-02-20 18:14_

Cool, I think we can remove it in v0.3.0.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-28 17:24_

---

_Comment by @charliermarsh on 2024-02-28 17:24_

Doing this now.

---

_Closed by @charliermarsh on 2024-02-28 17:38_

---
