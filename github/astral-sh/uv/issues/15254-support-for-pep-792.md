---
number: 15254
title: Support for PEP 792
type: issue
state: open
author: woodruffw
labels:
  - enhancement
assignees: []
created_at: 2025-08-13T16:56:06Z
updated_at: 2026-01-05T16:40:58Z
url: https://github.com/astral-sh/uv/issues/15254
synced_at: 2026-01-07T13:12:19-06:00
---

# Support for PEP 792

---

_Issue opened by @woodruffw on 2025-08-13 16:56_

### Summary

[PEP 792](https://peps.python.org/pep-0792/) is now final and has a [living PyPA spec](https://packaging.python.org/en/latest/specifications/project-status-markers/#project-status-markers).

TL;DR: PEP 792 defines "project status markers," which are project-wide states that get presented in the standard JSON and HTML indices.

For example, here's a project that's been marked as `archived`, meaning that it won't receive new releases:

```sh
% curl -s -H "Accept: application/vnd.pypi.simple.v1+json" https://pypi.org/simple/pepy/ |  jq '."project-status"'
{
  "status": "archived"
}
```

These markers are optional in the index API v1.4 and above; in earlier (<= 1.3) responses, the client should assume that all projects have the `active` status. Any 1.4+ response that doesn't have a status also defaults to `active`.

### Example

PEP 792 suggests that installers produce appropriate warnings on the following statuses:

* `archived`: the installer should warn that the project does not expect to be updated in the future
* `deprecated`: the installer should warn that the project is considered obsolete by its maintainers
* `quarantined`: the installer should warn that the project is considered unsafe for any use (e.g. malicious). the index will prevent resolution of quarantined projects anyways, but the warning will help users understand why the resolution subsequently fails 🙂 

In terms of _where_ this should happen: I _think_ it probably makes the most sense for these to be propagated on any environment-changing command, e.g. `uv add`, `uv sync`, `uv pip install`.

Subtasks:

- [ ] Data modeling + extract status markers from the detail responses (#17311)
- [ ] Plumb status markers into the internal representation
- [ ] Propagate relevant statuses as warnings (e.g. warn users when they request a quarantined / archived / deprecated project)

---

_Label `enhancement` added by @woodruffw on 2025-08-13 16:56_

---

_Referenced in [pypa/pip#13543](../../pypa/pip/issues/13543.md) on 2025-08-13 17:16_

---

_Referenced in [astral-sh/uv#17311](../../astral-sh/uv/pulls/17311.md) on 2026-01-02 22:36_

---
