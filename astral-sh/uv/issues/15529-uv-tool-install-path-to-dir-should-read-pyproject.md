```yaml
number: 15529
title: "`uv tool install /path/to/dir` should read `pyproject.toml` configuration in `/path/to/dir`"
type: issue
state: open
author: charliermarsh
labels:
  - configuration
assignees: []
created_at: 2025-08-26T13:29:58Z
updated_at: 2025-08-26T15:10:35Z
url: https://github.com/astral-sh/uv/issues/15529
synced_at: 2026-01-12T16:02:12Z
```

# `uv tool install /path/to/dir` should read `pyproject.toml` configuration in `/path/to/dir`

---

_@charliermarsh_

We typically _don't_ do this, e.g., `uv pip install /path/to/dir` will _not_ read configuration from `/path/to/dir/pyproject.toml` -- it only discovers configuration from the current working directory and above. (An exception is for named, pinned indexes in sources -- those are always respected.)

For tools, though, it seems like it might make sense to always respect the project configuration, since you can only provide a single target anyway. (E.g., you can `uv pip install /foo /bar`, in which case it becomes ambiguous whether we should respect any or all of the configuration files; but tools always install a single target.)

---

_Label `configuration` added by @charliermarsh on 2025-08-26 13:30_

---

_Comment by @charliermarsh on 2025-08-26 13:38_

This is actually sort of tricky to implement because the settings resolution happens before we resolve the install target.

---

_Comment by @charliermarsh on 2025-08-26 13:38_

Should we use the same behavior for `uv tool run`? (Probably?)

---

_Comment by @powercoconola on 2025-08-26 14:59_

I'd like if there were a message in the terminal about if we are using the "globally installed" tool version, or a version from a `pyproject.toml`. I run tools in various projects and it would be confusing to me if behavior changed without warning.

Should this be tagged as `breaking`?

---

_Comment by @charliermarsh on 2025-08-26 15:10_

Oh sorry, this wouldn't change the tool _version_. It would just mean that if the tool's `pyproject.toml` defined (e.g.) an `[[index]]`, we would respect that `[[index]]` rather than ignoring it as we do today (we only respect explicit named indexes in that case, which tends to be confusing).

---
