```yaml
number: 5703
title: No longer reinstalls editable paths
type: issue
state: closed
author: Nomelas
labels:
  - question
assignees: []
created_at: 2024-08-01T18:43:28Z
updated_at: 2024-09-10T01:44:37Z
url: https://github.com/astral-sh/uv/issues/5703
synced_at: 2026-01-12T15:58:58Z
```

# No longer reinstalls editable paths

---

_@Nomelas_

Looks like starting with version 0.2.28 (0.2.27 works fine), local editable packages are no longer being rebuilt. This is problematic for entry_points as you need to reinstall the editable package in order for the entry script to be created.

It looks like now it maintains a cached version that is even reinstalled if I try to pip install directly first (outside of uv) and then run uv pip commands to isntall it, it reverts back to the old version.

My workflow has been to run `uv pip sync requirements.txt` which contains `-e file:.` to reinstall the package, but that no longer works.

---

_Comment by @charliermarsh on 2024-08-01 18:45_

Thanks -- this is duplicate of #5484 so gonna redirect there.

My advice is that if you want your package to _always_ be rebuilt, you should set `reinstall-package = ["package-name"]` in a `uv.toml` or `pyproject.toml` file.


---

_Comment by @charliermarsh on 2024-08-01 18:45_

Feel free to chime in on #5484.

---

_Closed by @charliermarsh on 2024-08-01 18:45_

---

_Label `question` added by @charliermarsh on 2024-08-01 18:45_

---

_Comment by @charliermarsh on 2024-09-10 01:44_

We have a new API whereby you can add additional files to consider when invalidating the cache. You can also include the current Git commit (i.e., invalidate whenever the SHA changes).

Looks like this:

```toml
[tool.uv]
cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" }, { git = true }]
```

See: https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata.

---
