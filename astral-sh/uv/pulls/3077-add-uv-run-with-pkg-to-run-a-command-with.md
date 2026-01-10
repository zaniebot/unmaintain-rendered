```yaml
number: 3077
title: "Add `uv run --with <pkg>` to run a command with ephemeral requirements"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cli
assignees: []
merged: true
base: main
head: zb/uv-run-with
created_at: 2024-04-16T22:56:06Z
updated_at: 2024-04-19T14:23:28Z
url: https://github.com/astral-sh/uv/pull/3077
synced_at: 2026-01-10T14:43:31Z
```

# Add `uv run --with <pkg>` to run a command with ephemeral requirements

---

_Pull request opened by @zanieb on 2024-04-16 22:56_

Holy cow does installation / resolution take a ton of options. We side-step most of them here.

If the current environment satisfies the requirements, it is used. Otherwise, we create a new environment with the requested dependencies.

---

_Label `internal` added by @zanieb on 2024-04-16 22:56_

---

_Label `cli` added by @zanieb on 2024-04-16 22:56_

---

_Comment by @samypr100 on 2024-04-17 01:45_

I wonder if we could leverage parts of [PEP-0723](https://peps.python.org/pep-0723/)

---

_Comment by @zanieb on 2024-04-17 02:27_

We definitely will support PEP 723! Tracking in #3096 

---

_Comment by @zanieb on 2024-04-17 02:27_

Self-reminder to follow-up on https://github.com/astral-sh/uv/pull/3085 here.

---

_Marked ready for review by @zanieb on 2024-04-17 18:58_

---

_@charliermarsh reviewed on 2024-04-18 00:31_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/run.rs`:37 on 2024-04-18 00:31_

Desperate to run IntelliJ's "Optimize imports" on this baby, to break into logical categories.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/run.rs`:627 on 2024-04-18 00:32_

We might want to consider making this `#[source]` so that it has to be specified manually?

---

_@charliermarsh approved on 2024-04-18 00:32_

---

_@zanieb reviewed on 2024-04-18 01:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:37 on 2024-04-18 01:19_

lol I started fixing it then was like yuck merge conflicts will fix later

---

_@zanieb reviewed on 2024-04-18 01:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:627 on 2024-04-18 01:20_

Will add a TODO, this is copied directly from `pip_install`.

---

_Review comment by @konstin on `crates/uv/src/commands/run.rs`:37 on 2024-04-18 08:59_

I like rustfmt's default single block behavior :no_mouth: (with `pub use` in a separate block, cause it's the api definition)

---

_@konstin approved on 2024-04-18 09:03_

---

_Merged by @zanieb on 2024-04-19 14:23_

---

_Closed by @zanieb on 2024-04-19 14:23_

---

_Branch deleted on 2024-04-19 14:23_

---
