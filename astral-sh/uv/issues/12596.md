```yaml
number: 12596
title: "Fails to parse `channels` in `environment.yml`"
type: issue
state: closed
author: lnicola
labels:
  - question
assignees: []
created_at: 2025-04-01T08:32:53Z
updated_at: 2025-04-01T12:58:06Z
url: https://github.com/astral-sh/uv/issues/12596
synced_at: 2026-01-10T03:41:47Z
```

# Fails to parse `channels` in `environment.yml`

---

_Issue opened by @lnicola on 2025-04-01 08:32_

### Summary

I have a project with a `requirements.txt` that starts like this:

```yaml
name: pzero
channels:
  - loop3d
  - conda-forge
  - defaults
dependencies:
  - _libavif_api=1.2.0=h57928b3_0
  - _openmp_mutex=4.5=2_gnu
  - affine=2.4.0=pyhd8ed1ab_1
```

`uv add -r` and `uv pip install -r` fail with: `error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement at envs/pzero-env2.yml:3:3`, presumably because of the `channels` key.

### Platform

Linux 6.14.0-63.fc42.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.11

### Python version

_No response_

PS: I've never really used `uv` or Conda. Not sure if you're planning to support channels.

---

_Label `bug` added by @lnicola on 2025-04-01 08:32_

---

_Comment by @charliermarsh on 2025-04-01 12:55_

That looks like a Conda environment file, not a `requirements.txt` file. A `requirements.txt` file is a flat list of dependencies, like:

```
flask
requests
```

---

_Label `bug` removed by @charliermarsh on 2025-04-01 12:55_

---

_Label `question` added by @charliermarsh on 2025-04-01 12:55_

---

_Renamed from "Fails to parse `channels` in `requirements.txt`" to "Fails to parse `channels` in `environment.yml`" by @lnicola on 2025-04-01 12:55_

---

_Comment by @lnicola on 2025-04-01 12:56_

Yes, it's a Conda YAML, sorry for the mistake in the issue title. It's fine if you're not planning to support those, although the error message is not ideal.

---

_Closed by @lnicola on 2025-04-01 12:58_

---
