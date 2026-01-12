```yaml
number: 659
title: Prerelease not selected for msgraph-core
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-12-15T11:15:02Z
updated_at: 2023-12-29T02:44:36Z
url: https://github.com/astral-sh/uv/issues/659
synced_at: 2026-01-12T15:58:24Z
```

# Prerelease not selected for msgraph-core

---

_@konstin_

Puffin:
```
No solution found when resolving: msgraph-sdk

Caused by:
    Because there is no version of msgraph-core available matching >=1.0.0a2 and msgraph-sdk==1.0.0 depends on msgraph-core>=1.0.0a2, msgraph-sdk==1.0.0 is forbidden.
```
pip-tools:
```
[...]
msgraph-core==1.0.0a4
    # via msgraph-sdk
msgraph-sdk==1.0.0
    # via -r target/requirements.in
[...]
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-15 16:52_

---

_Label `bug` added by @charliermarsh on 2023-12-15 16:52_

---

_Comment by @charliermarsh on 2023-12-15 16:52_

I'll take a look, this might be intentional based on our pre-release strategy?

---

_Comment by @charliermarsh on 2023-12-15 18:40_

This works correctly with `--prerelease allow` which is as-intended. We only allow pre-releases if you explicitly specify a pre-release version as an initial dependency.

---

_Unassigned @charliermarsh by @charliermarsh on 2023-12-15 18:41_

---

_Comment by @charliermarsh on 2023-12-15 18:46_

Thinking about what a better error message would look like here.

---

_Comment by @charliermarsh on 2023-12-15 18:47_

@konstin -- I think we should change our pre-release semantics such that we allow pre-releases under the "if necessary" policy if there's no non-pre marker that satisfies the version. Right now, we only allow under "if necesary" if _all_ versions of a package are pre-release. But that seems incorrect based on https://peps.python.org/pep-0440/#handling-of-pre-releases.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-15 20:08_

---

_Comment by @charliermarsh on 2023-12-16 20:24_

A few options:

- Just leave it, stick to our current behavior.
- Just leave it, but add a dedicated error message to suggest using `--prerelease allow`.
- Allow pre-releases if it's _ever_ necessary (i.e., if the only versions satisfying a range are pre-releases, even if the user didn't request it). Right now, we allow pre-releases if _all_ versions of a package are pre-releases, but that's a much stricter constraint. The downside of this change is that we'd start to select pre-releases when they weren't explicitly requested.
- Try to allow pre-releases whenever at least one requiring package uses a pre-release marker for the package. Might be impossible...

---

_Closed by @charliermarsh on 2023-12-29 02:44_

---
