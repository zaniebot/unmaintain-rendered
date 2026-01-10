```yaml
number: 8208
title: "`uv add` with no version bound does not sufficiently update uv.lock"
type: issue
state: closed
author: keysmashes
labels:
  - bug
assignees: []
created_at: 2024-10-15T10:23:11Z
updated_at: 2024-10-15T23:09:57Z
url: https://github.com/astral-sh/uv/issues/8208
synced_at: 2026-01-10T04:45:10Z
```

# `uv add` with no version bound does not sufficiently update uv.lock

---

_Issue opened by @keysmashes on 2024-10-15 10:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When adding a package via `uv add` with no lower bound, a lower bound is added to the entry in pyproject.toml, but this is not included in the `specifier` of the entry added to the root package's `package.metadata.requires-dist` in uv.lock.

Concretely, straight after a `uv add typing-extensions`, `package.metadata.requires-dist` contains an entry like `{ name = "typing-extensions" },`; after a subsequent `uv lock`, this is transformed into `{ name = "typing-extensions", specifier = ">=4.12.2" },`.

Full example:

```console
$ uv --version
uv 0.4.21
$ uv add typing-extensions
warning: Missing version constraint (e.g., a lower bound) for `typing-extensions`
Resolved 32 packages in 71ms
   Built farmer @ file:///nfs/cellgeni/ash/src/farmer
Prepared 1 package in 1.37s
Uninstalled 1 package in 38ms
Installed 1 package in 92ms
 ~ farmer==24.285 (from file:///nfs/cellgeni/ash/src/farmer)
$ uv lock --locked
Resolved 32 packages in 75ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
$ git diff
diff --git a/pyproject.toml b/pyproject.toml
index 15c42e5..6421510 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -12,6 +12,7 @@ dependencies = [
     "pydantic>=2.9.1",
     "python-dotenv>=1.0.1",
     "slack-bolt==1.20.0",
+    "typing-extensions>=4.12.2",
     "uvicorn>=0.30.6",
 ]
 license.text = "AGPL-3.0-or-later"
diff --git a/uv.lock b/uv.lock
index f2ab008..948b86c 100644
--- a/uv.lock
+++ b/uv.lock
@@ -196,6 +196,7 @@ dependencies = [
     { name = "pydantic" },
     { name = "python-dotenv" },
     { name = "slack-bolt" },
+    { name = "typing-extensions" },
     { name = "uvicorn" },
 ]

@@ -208,6 +209,7 @@ requires-dist = [
     { name = "pydantic", specifier = ">=2.9.1" },
     { name = "python-dotenv", specifier = ">=1.0.1" },
     { name = "slack-bolt", specifier = "==1.20.0" },
+    { name = "typing-extensions" },
     { name = "uvicorn", specifier = ">=0.30.6" },
 ]

$ uv lock
Resolved 32 packages in 65ms
$ git diff
diff --git a/pyproject.toml b/pyproject.toml
index 15c42e5..6421510 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -12,6 +12,7 @@ dependencies = [
     "pydantic>=2.9.1",
     "python-dotenv>=1.0.1",
     "slack-bolt==1.20.0",
+    "typing-extensions>=4.12.2",
     "uvicorn>=0.30.6",
 ]
 license.text = "AGPL-3.0-or-later"
diff --git a/uv.lock b/uv.lock
index f2ab008..5d949f5 100644
--- a/uv.lock
+++ b/uv.lock
@@ -196,6 +196,7 @@ dependencies = [
     { name = "pydantic" },
     { name = "python-dotenv" },
     { name = "slack-bolt" },
+    { name = "typing-extensions" },
     { name = "uvicorn" },
 ]

@@ -208,6 +209,7 @@ requires-dist = [
     { name = "pydantic", specifier = ">=2.9.1" },
     { name = "python-dotenv", specifier = ">=1.0.1" },
     { name = "slack-bolt", specifier = "==1.20.0" },
+    { name = "typing-extensions", specifier = ">=4.12.2" },
     { name = "uvicorn", specifier = ">=0.30.6" },
 ]

```

---

_Label `bug` added by @konstin on 2024-10-15 11:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-15 13:51_

---

_Comment by @charliermarsh on 2024-10-15 13:52_

Thanks! There's some code to handle this case but apparently it's not quite right. I'll take a look.

---

_Comment by @charliermarsh on 2024-10-15 22:59_

Oh, I'm so dumb... The relevant code is under a `debug_assert!`, so it only runs in debug builds...

---

_Closed by @charliermarsh on 2024-10-15 23:09_

---

_Closed by @charliermarsh on 2024-10-15 23:09_

---
