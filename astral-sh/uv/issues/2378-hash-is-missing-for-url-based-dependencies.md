```yaml
number: 2378
title: Hash is missing for url based dependencies
type: issue
state: closed
author: 9128305
labels:
  - enhancement
assignees: []
created_at: 2024-03-12T07:57:57Z
updated_at: 2024-04-10T20:02:46Z
url: https://github.com/astral-sh/uv/issues/2378
synced_at: 2026-01-12T15:58:37Z
```

# Hash is missing for url based dependencies

---

_@9128305_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv version: uv 0.1.17 (ec8315166 2024-03-10)
```shell
echo "astpretty @ https://github.com/asottile/astpretty/archive/refs/heads/main.zip" > requirements.in
pip-compile --allow-unsafe --generate-hashes --resolver=backtracking --quiet --strip-extras --no-annotate
uv pip compile --generate-hashes --no-annotate requirements.in
```

---

_Label `enhancement` added by @charliermarsh on 2024-03-12 14:21_

---

_Comment by @eth3lbert on 2024-03-16 11:10_

This would be quite interesting. Currently, uv downloads and builds the source but doesn't store the source archive. In this case, I'm not sure how to recompute the hash if a cache entry exists.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-07 00:12_

---

_Closed by @charliermarsh on 2024-04-10 20:02_

---
