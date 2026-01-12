```yaml
number: 1877
title: Remove --max-complexity from the CLI
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cli-options
created_at: 2023-01-14T23:27:17Z
updated_at: 2023-03-06T02:35:25Z
url: https://github.com/astral-sh/ruff/pull/1877
synced_at: 2026-01-12T04:39:44Z
```

# Remove --max-complexity from the CLI

---

_Pull request opened by @charliermarsh on 2023-01-14 23:27_

_No description provided._

---

_Merged by @charliermarsh on 2023-01-14 23:27_

---

_Closed by @charliermarsh on 2023-01-14 23:27_

---

_Branch deleted on 2023-01-14 23:27_

---

_Comment by @not-my-profile on 2023-01-15 04:17_

Should this be documented in `BREAKING_CHANGES.md`?

---

_Comment by @charliermarsh on 2023-01-15 04:18_

Yeah, it should. I'll add it.

---

_Comment by @cclauss on 2023-02-24 14:29_

Not a fan of this choice...  It is quite helpful when building up a new config to be able to test on the command line before writing the complete config into `pyproject.toml`.

---

_Comment by @not-my-profile on 2023-02-24 14:39_

I think the change makes sense ... `mccabe.max-complexity` isn't more important than any other rule-specific settings and we certainly don't want to introduce a dedicated command-line option for each of our rule-specific settings.

What we could do is introduce a more generic mechanism such as `--config <KEY>=<VALUE>`.

---

_Comment by @cclauss on 2023-03-05 16:45_

The error message is misleading because (at least on my Mac) I cannot figure out how to make `-- --x` escaping work.

% `ruff --max-complexity=10 .`
```
error: unexpected argument '--max-complexity' found

  note: to pass '--max-complexity' as a value, use '-- --max-complexity'
```

% `ruff --mccabe.max-complexity=10 .`
```
error: unexpected argument '--mccabe.max-complexity' found

  note: to pass '--max-complexity' as a value, use '-- --mccabe.max-complexity'
```

---

_Comment by @charliermarsh on 2023-03-06 02:35_

It's correct though, `--max-complexity` is not a supported flag. IIUC, that's a generic message telling you that if you intended it to be interpreted as a value (e.g., if you had a _file_ named `--max-complexity`), you'd pass it after a double dash.

---
