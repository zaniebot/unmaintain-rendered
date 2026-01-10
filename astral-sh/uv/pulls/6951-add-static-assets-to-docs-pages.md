```yaml
number: 6951
title: "Add `static` assets to docs pages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/seo
created_at: 2024-09-02T23:54:21Z
updated_at: 2024-09-03T00:00:34Z
url: https://github.com/astral-sh/uv/pull/6951
synced_at: 2026-01-10T12:53:37Z
```

# Add `static` assets to docs pages

---

_Pull request opened by @charliermarsh on 2024-09-02 23:54_

## Summary

This PR points the docs towards the static assets that we use for `astral.sh`, so that we can use shared assets for the Ruff and uv docs.


---

_@charliermarsh reviewed on 2024-09-02 23:54_

---

_Review comment by @charliermarsh on `mkdocs.template.yml`:5 on 2024-09-02 23:54_

I guess we have to keep something here? I don't know if this will be used or the links in the override:

```
<link rel="icon" type="image/png" sizes="32x32" href="/static/favicon-32x32.png"/>
<link rel="icon" type="image/png" sizes="16x16" href="/static/favicon-16x16.png"/>
```

---

_Marked ready for review by @charliermarsh on 2024-09-02 23:58_

---

_@charliermarsh reviewed on 2024-09-03 00:00_

---

_Review comment by @charliermarsh on `mkdocs.template.yml`:5 on 2024-09-03 00:00_

It looks like this gets used.

---

_Merged by @charliermarsh on 2024-09-03 00:00_

---

_Closed by @charliermarsh on 2024-09-03 00:00_

---

_Branch deleted on 2024-09-03 00:00_

---

_Label `documentation` added by @charliermarsh on 2024-09-03 00:00_

---
