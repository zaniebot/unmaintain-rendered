```yaml
number: 2463
title: Allow non-ruff.toml-named files for --config
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/any-name
created_at: 2023-02-02T02:09:25Z
updated_at: 2023-02-02T02:35:43Z
url: https://github.com/astral-sh/ruff/pull/2463
synced_at: 2026-01-12T04:52:00Z
```

# Allow non-ruff.toml-named files for --config

---

_Pull request opened by @charliermarsh on 2023-02-02 02:09_

Previously, if you passed in a file on the command-line via `--config`, it had to be named either `pyproject.toml` or `ruff.toml` -- otherwise, we errored. I think this is too strict. `pyproject.toml` is a special name in the ecosystem, so we should require _that_; but otherwise, let's just assume it's in `ruff.toml` format.

As an alternative, we could add a `--pyproject` argument for `pyproject.toml`, and assume anything passed to `--config` is in `ruff.toml` format. But that _would_ be a breaking change and is arguably more confusing. (This isn't a breaking change, since it only loosens the CLI.)

Closes #2462.

---

_Review comment by @charliermarsh on `src/settings/configuration.rs`:91 on 2023-02-02 02:09_

(Looks like this is unused.)

---

_@charliermarsh reviewed on 2023-02-02 02:09_

---

_Merged by @charliermarsh on 2023-02-02 02:35_

---

_Closed by @charliermarsh on 2023-02-02 02:35_

---

_Branch deleted on 2023-02-02 02:35_

---
