```yaml
number: 8566
title: Add documentation for PEP 735 support
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: tracking/735
head: zb/735-docs
created_at: 2024-10-25T14:44:51Z
updated_at: 2024-10-25T18:15:51Z
url: https://github.com/astral-sh/uv/pull/8566
synced_at: 2026-01-10T12:54:12Z
```

# Add documentation for PEP 735 support

---

_Pull request opened by @zanieb on 2024-10-25 14:44_

This adds the minimal documentation I think we need to release PEP 735 support.

I want to add documentation on `include-group` and such but that can come after.

I also want to restructure some of the project dependency documentation, but that will be easier once this all lands in `main`.

---

_Label `documentation` added by @zanieb on 2024-10-25 14:44_

---

_@MichaReiser reviewed on 2024-10-25 15:58_

---

_Review comment by @MichaReiser on `docs/concepts/dependencies.md`:485 on 2024-10-25 15:58_

Not really relevant for the documentation but what happens if I run `uv add --dev xx` when there's only a `dev-dependency` but no `dev` dependency group?

---

_Review comment by @zanieb on `docs/concepts/dependencies.md`:485 on 2024-10-25 16:01_

That's a great question, Charlie is fixing the behavior there (right now we actually never add to the `dev` group during `uv add --dev`) so I'm not sure what the final behavior is yet. I'll update this to include that once he puts up that PR.

---

_@zanieb reviewed on 2024-10-25 16:01_

---

_Review comment by @carljm on `docs/concepts/dependencies.md`:441 on 2024-10-25 16:42_

It might be useful somewhere in here to state explicitly that `uv add --dev` is equivalent to `uv add -- group dev`, etc. (Unless they aren't entirely equivalent, e.g. maybe something with the handling of `uv.tool.dev-dependencies` as in Micha's comment below, in which case that should be clarified.)

---

_@carljm reviewed on 2024-10-25 16:42_

---

_@zanieb reviewed on 2024-10-25 16:47_

---

_Review comment by @zanieb on `docs/concepts/dependencies.md`:441 on 2024-10-25 16:47_

Will do!

#8567 also adds some context in the CLI about the equivalence of these commands.

---

_@charliermarsh approved on 2024-10-25 18:05_

---

_@charliermarsh reviewed on 2024-10-25 18:06_

---

_Review comment by @charliermarsh on `docs/concepts/dependencies.md`:485 on 2024-10-25 18:06_

(It adds to `dev-dependencies`.)

---

_Merged by @zanieb on 2024-10-25 18:15_

---

_Closed by @zanieb on 2024-10-25 18:15_

---

_Branch deleted on 2024-10-25 18:15_

---
