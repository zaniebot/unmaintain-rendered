```yaml
number: 5936
title: Update the interface for declaring Python download preferences
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: zb/python-downloads
created_at: 2024-08-08T21:29:55Z
updated_at: 2024-08-09T18:10:21Z
url: https://github.com/astral-sh/uv/pull/5936
synced_at: 2026-01-10T13:31:54Z
```

# Update the interface for declaring Python download preferences

---

_Pull request opened by @zanieb on 2024-08-08 21:29_

The loose consensus is that "fetch" doesn't have much meaning and that a boolean flag makes more sense from the command line.

1. Adds `--allow-python-downloads` (hidden, default) and `--no-python-downloads` to the CLI to quickly enable or disable downloads
2. Deprecates `--python-fetch` in favor of the options from (1)
3. Removes  `python-fetch` in favor of a `python-downloads` setting
5. Adds a `never` variant to the enum, allowing even explicit installs to be disabled via the configuration file

## Test plan

I tested this with various `pyproject.toml`-level settings and `uv venv --preview --python 3.12.2` and `uv python install 3.12.2` with and without the new CLI flags.

---

_Label `configuration` added by @zanieb on 2024-08-08 21:29_

---

_Label `preview` added by @zanieb on 2024-08-08 21:29_

---

_Comment by @zanieb on 2024-08-08 21:35_

Notably I did not deprecate the `python-fetch` setting, just removed it. I'm not sure what the pattern is to deprecate a setting yet, nor am I sure it's worth the extra effort.

---

_Comment by @zanieb on 2024-08-08 21:38_

I'm a little uncertain of the "never" value, but I do think it has some utility to require explicit opt-in from the CLI to install versions.

---

_Marked ready for review by @zanieb on 2024-08-08 22:00_

---

_Comment by @zanieb on 2024-08-08 22:06_

Whoops I checked in my `pyproject.toml` change, and look, it works https://github.com/astral-sh/uv/actions/runs/10310109626/job/28541254932?pr=5936

---

_Comment by @charliermarsh on 2024-08-09 02:48_

Thanks. Am I right that `--python-downloads` isn't exposed on the command line? So those settings are only available in a configuration file?

I'm wondering it it should be `--download-python` and `--no-download-python` to match settings like `--compile-bytecode`. Are we consistent with that pattern?

---

_Comment by @zanieb on 2024-08-09 13:38_

I leaned away from `--download-python` because it won't _force_ Python to be downloaded, it just allows it if it would be, e.g. `--python-preference only-system --download-python` would still never download Python.

---

_Comment by @zanieb on 2024-08-09 13:39_

> Am I right that --python-downloads isn't exposed on the command line? So those settings are only available in a configuration file?

Yes, only a boolean option is available from the command line and the "manual" value is only accessible via the configuration file. I find that a little peculiar, but don't think it'll be problematic in practice.

---

_@charliermarsh approved on 2024-08-09 18:08_

---

_Merged by @zanieb on 2024-08-09 18:10_

---

_Closed by @zanieb on 2024-08-09 18:10_

---

_Branch deleted on 2024-08-09 18:10_

---
