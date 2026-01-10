```yaml
number: 4651
title: "`pip tree --prune` output looks incorrect when the pruned package is the last in the group"
type: issue
state: closed
author: ChannyClaus
labels: []
assignees: []
created_at: 2024-06-29T20:39:15Z
updated_at: 2024-06-29T22:45:36Z
url: https://github.com/astral-sh/uv/issues/4651
synced_at: 2026-01-10T05:31:37Z
```

# `pip tree --prune` output looks incorrect when the pruned package is the last in the group

---

_Issue opened by @ChannyClaus on 2024-06-29 20:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
can be reproduced via:
```
$ rm -rf .venv && cargo run -q venv && cargo run -q pip install requests && cargo run -q pip tree --prune certifi
Using Python 3.12.2 interpreter at: /Users/chankang/.pyenv/versions/3.12.2/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
Resolved 5 packages in 17ms
Installed 5 packages in 13ms
 + certifi==2024.6.2
 + charset-normalizer==3.3.2
 + idna==3.7
 + requests==2.32.3
 + urllib3==2.2.2
requests v2.32.3
├── charset-normalizer v3.3.2
├── idna v3.7
├── urllib3 v2.2.2
```
where
```
$ git rev-parse HEAD
13b0beb56fdc607c1f38d820dbf8c95c8fd0ce84
```
i'll look into this one as well... 😭 

---

_Closed by @zanieb on 2024-06-29 22:45_

---
