```yaml
number: 10539
title: Add meta titles to documents in guides, excluding integration documents.
type: pull_request
state: merged
author: FishAlchemist
labels:
  - documentation
assignees: []
merged: true
base: main
head: guides_no_integration
created_at: 2025-01-12T13:15:27Z
updated_at: 2025-01-16T02:25:51Z
url: https://github.com/astral-sh/uv/pull/10539
synced_at: 2026-01-12T16:09:21Z
```

# Add meta titles to documents in guides, excluding integration documents.

---

_@FishAlchemist_

## Summary
Add meta titles to documents in guides, excluding integration documents.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
``uvx --with-requirements docs/requirements.txt -- mkdocs build --strict -f mkdocs.public.yml``
<details>
 <summary>Build Result</summary>

* ``guides/install-python``
```html
...
<meta name="description" content="Guide to install specific Python versions, manage existing installations, and automate downloads with uv.">
...
<title>Install and Manage Python | uv</title>
...
```
* ``guides/projects``
```html
...
<meta name="description" content="Guide to create, manage, and build Python projects with uv, including dependencies and distributions.">
...
<title>Working on projects | uv</title>
...
```
* ``guides/publish``
```html
...
<meta name="description" content="Guide to build and publish Python packages using uv">
...
<title>Publishing a package | uv</title>
...
```
* ``guides/scripts``
```html
...
<meta name="description" content="Run Python scripts quickly and manage dependencies efficiently using uv. Learn about inline metadata and more.">
...
<title>Run Scripts | uv</title>
...
```
* ``guides/tools``
```html
...
<meta name="description" content="Guide to run, install, and upgrade Python tools using uv.">
...
<title>Using tools | uv</title>
...
```
</details>

---

_@FishAlchemist reviewed on 2025-01-12 13:17_

---

_Review comment by @FishAlchemist on `docs/guides/install-python.md`:2 on 2025-01-12 13:17_

``Install and Manage Python`` differs from the original ``Installing Python`` .

---

_Review comment by @cthoyt on `docs/guides/install-python.md`:2 on 2025-01-12 15:47_

just a nit, but the other ones all use -ing words, maybe you want to standardize this to Installing and Managing Python?

```suggestion
title: Installing and Managing Python
```

---

_@cthoyt reviewed on 2025-01-12 15:47_

---

_Comment by @zanieb on 2025-01-15 20:12_

Thanks! I updated these for consistency with https://github.com/astral-sh/uv/pull/10450

---

_Label `documentation` added by @zanieb on 2025-01-15 20:12_

---

_@zanieb approved on 2025-01-15 20:12_

---

_Merged by @zanieb on 2025-01-15 20:12_

---

_Closed by @zanieb on 2025-01-15 20:12_

---

_Branch deleted on 2025-01-16 02:25_

---
