```yaml
number: 8192
title: "Don't recommend `--prerelease=allow` during build requirement resolution errors"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/pre-error
created_at: 2024-10-15T00:46:38Z
updated_at: 2024-10-15T01:06:46Z
url: https://github.com/astral-sh/uv/pull/8192
synced_at: 2026-01-12T16:08:12Z
```

# Don't recommend `--prerelease=allow` during build requirement resolution errors

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/3686.


---

_Renamed from "Don't recommend --prerelease=allow for source dist builds" to "Don't recommend `--prerelease=allow` for source dist builds" by @charliermarsh on 2024-10-15 00:46_

---

_Label `bug` added by @charliermarsh on 2024-10-15 00:46_

---

_Label `error messages` added by @charliermarsh on 2024-10-15 00:46_

---

_Comment by @zanieb on 2024-10-15 00:50_

I don't entirely follow the motivation in the issue. Can you explain a bit?

---

_Comment by @charliermarsh on 2024-10-15 00:51_

We don't pass the `--prerelease` flag to source distribution builds. Recommending `--prerelease=allow` has no effect.

---

_Comment by @zanieb on 2024-10-15 00:56_

Like.. all source distribution builds including those from the registry that are prereleases? Or non-registry ones like local directories or Git repositories?

---

_Comment by @charliermarsh on 2024-10-15 00:59_

I don't fully follow the distinction you're referencing but we never pass the `--prerelease` flag to source distribution builds. Specifically, when we resolve `build-system.requires`, we don't propagate `--prerelease`, `--resolution`, or `--no-deps`. That applies to _all_ source distribution builds (but _only_ to the `build-system.requires`, like `requires = ["setuptools>=40.8.0"]`).

---

_Comment by @charliermarsh on 2024-10-15 00:59_

In the linked issue, we failed to resolve `requires = ["setuptools>=40.8.0"]` when building some package. We then recommended passing `--prerelease=allow`, but doing so has no effect.

---

_Comment by @charliermarsh on 2024-10-15 01:04_

We removed that propagation in https://github.com/astral-sh/uv/pull/1607 if you want to see a few linked issues. I think it's unclear whether `--prerelease` should be propagated (`--resolution` definitely should not, I think that one is much clearer), but it's reasonable for it _not_ to be IMO.

---

_Merged by @charliermarsh on 2024-10-15 01:04_

---

_Closed by @charliermarsh on 2024-10-15 01:04_

---

_Branch deleted on 2024-10-15 01:04_

---

_Comment by @zanieb on 2024-10-15 01:05_

Ah thanks for clarifying. I somehow missed that we're talking about an error when resolving _build requirements_. Hence my extreme confusion and dumb questions. This change makes a ton of sense. Can you rephrase the title to make it a bit more obvious? Like...

> Don't recommend `--prerelease=allow` during build requirement resolution errors

---

_Comment by @charliermarsh on 2024-10-15 01:06_

Yeah for sure.

---

_Renamed from "Don't recommend `--prerelease=allow` for source dist builds" to "Don't recommend `--prerelease=allow` during build requirement resolution errors" by @charliermarsh on 2024-10-15 01:06_

---

_Comment by @charliermarsh on 2024-10-15 01:06_

Thank you!

---
