```yaml
number: 3176
title: Intermittent errors when installing from git reference
type: issue
state: closed
author: edgarrmondragon
labels:
  - needs-mre
assignees: []
created_at: 2024-04-22T02:09:31Z
updated_at: 2024-04-23T02:53:34Z
url: https://github.com/astral-sh/uv/issues/3176
synced_at: 2026-01-12T15:58:42Z
```

# Intermittent errors when installing from git reference

---

_@edgarrmondragon_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

* Platform: M2 macOS Sonoma
* uv 0.1.35

[Our project runs](https://github.com/meltano/meltano/blob/068289ea8fb7bdf558b58fd541c71c40c0648367/src/meltano/core/venv_service.py#L664-L673), for example `uv pip install git+https://github.com/jwills/target-duckdb.git@main duckdb==0.9.2
` under the hood and I've confirmed that running this command directly fails occasionally:

```
$ uv pip install git+https://github.com/jwills/target-duckdb.git@main duckdb==0.9.2
Updating https://github.com/jwills/target-duckdb.git (main)
error: Git operation failed
  Caused by: failed to fetch into: /Users/edgarramirez/Library/Caches/uv/git-v0/db/84855f4c2e0a8c94
  Caused by: failed to fetch branch or tag `main`
  Caused by: process didn't exit successfully: `git fetch --force --update-head-ok 'https://github.com/jwills/target-duckdb.git' '+refs/tags/main:refs/remotes/origin/tags/main'` (exit status: 128)
--- stderr
```


---

_Comment by @ChannyClaus on 2024-04-22 02:43_

looks like the subprocess failing while running `git` is failing rather than `uv`? (i.e. is there a reason to suspect this is a `uv` issue?)

---

_Comment by @zanieb on 2024-04-22 03:52_

Does that `git fetch` invocation work outside of `uv`?

Was there any output in that stderr log?

---

_Label `needs-mre` added by @zanieb on 2024-04-23 02:20_

---

_Renamed from "Intermitent errors when installing from git reference" to "Intermittent errors when installing from git reference" by @edgarrmondragon on 2024-04-23 02:52_

---

_Comment by @edgarrmondragon on 2024-04-23 02:53_

I think @ChannyClaus is right and this seems to be a problem in the git client. Still not sure what's going on but I'm seeing similar intermittent errors with pre-commit:

```
stderr:
    fatal: unable to access 'https://github.com/pycqa/flake8/': LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443
Check the log at /Users/edgarramirez/.cache/pre-commit/pre-commit.log
```

---

_Closed by @edgarrmondragon on 2024-04-23 02:53_

---
