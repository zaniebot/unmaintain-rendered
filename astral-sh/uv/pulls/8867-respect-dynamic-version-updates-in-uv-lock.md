```yaml
number: 8867
title: "Respect dynamic version updates in `uv lock`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dyn
created_at: 2024-11-06T16:31:12Z
updated_at: 2024-11-13T06:38:35Z
url: https://github.com/astral-sh/uv/pull/8867
synced_at: 2026-01-10T12:00:00Z
```

# Respect dynamic version updates in `uv lock`

---

_Pull request opened by @charliermarsh on 2024-11-06 16:31_

## Summary

Closes https://github.com/astral-sh/uv/issues/8866.


---

_Label `bug` added by @charliermarsh on 2024-11-06 16:31_

---

_Marked ready for review by @charliermarsh on 2024-11-06 16:31_

---

_Merged by @charliermarsh on 2024-11-06 16:40_

---

_Closed by @charliermarsh on 2024-11-06 16:40_

---

_Branch deleted on 2024-11-06 16:40_

---

_Comment by @rino0601 on 2024-11-13 06:38_

@charliermarsh 

It seems that this change has affected the CI we were using.

I prefer using the following configuration:

```toml
[build-system]
requires = ["pdm-backend"]
build-backend = "pdm.backend"

[tool.pdm.version]
source = "scm"
write_to = "mypkg/_version.py"
write_template = "__version__ = '{}'"
```

There are likely similar backends other than `pdm-backend` as well.

After version 0.5.0, running `uv sync --frozen` still modifies `uv.lock`.

This results in every version including a date, like this:

`mypkg 0.2.2.dev3+g83f1f40.d20241113`

When I add a `git diff` during the CI process, the output looks like this:

```
diff --git a/uv.lock b/uv.lock
index dd84aab..0d49ad8 100644
--- a/uv.lock
+++ b/uv.lock
@@ -6,7 +6,7 @@ constraints = [{ name = "sh", specifier = "==1.14.3" }]

[[package]]
name = "mypkg"
-version = "0.2.2.dev4+gee95dc2.d20241113"
+version = "0.2.2.dev6+g77694c2.d20241113"
source = { editable = "." }
dependencies = [
{ name = "sh" },
```

The commit hash following `+g` differs from the actual commit pushed. This is because, with branch protection settings, the commit for the PR changes with each CI run.

In this situation, if a tag is created, `uv.lock` is always modified, resulting in a version like `0.2.3.d20241113`.

If including `commit7` in the dynamic version is the issue, it could be worked around by adjusting the `pdm-backend` configuration to exclude `commit7` from the version format.
However, it would be ideal if `uv` could provide an option to control this. Having `commit7` in the version has been quite useful for me during development.



---
