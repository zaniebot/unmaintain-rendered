```yaml
number: 10432
title: Fetch concurrently for non-first-match index strategies
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/concurr
created_at: 2025-01-09T14:59:06Z
updated_at: 2025-01-10T10:13:41Z
url: https://github.com/astral-sh/uv/pull/10432
synced_at: 2026-01-12T16:09:17Z
```

# Fetch concurrently for non-first-match index strategies

---

_@charliermarsh_

## Summary

On a basic test, this speeds up cold resolution by about 25%:

```
❯ hyperfine "uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache" "../target/release/uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache" --warmup 10 --runs 30
Benchmark 1: uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache
  Time (mean ± σ):     585.8 ms ±  28.2 ms    [User: 149.7 ms, System: 97.4 ms]
  Range (min … max):   541.5 ms … 654.8 ms    30 runs

Benchmark 2: ../target/release/uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache
  Time (mean ± σ):     468.3 ms ±  52.0 ms    [User: 131.7 ms, System: 76.9 ms]
  Range (min … max):   380.2 ms … 607.0 ms    30 runs

Summary
  ../target/release/uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache ran
    1.25 ± 0.15 times faster than uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache
```

Given:
```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12.0"
dependencies = [
    "black>=24.10.0",
    "django>=5.1.4",
    "flask>=3.1.0",
    "requests>=2.32.3",
]
```

And:

```shell
hyperfine "uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache" "../target/release/uv lock --extra-index-url https://download.pytorch.org/whl/cpu --index-strategy unsafe-best-match --upgrade --no-cache" --warmup 10 --runs 30
```

Closes https://github.com/astral-sh/uv/issues/10429.


---

_Label `performance` added by @charliermarsh on 2025-01-09 14:59_

---

_Review requested from @konstin by @charliermarsh on 2025-01-09 14:59_

---

_Marked ready for review by @charliermarsh on 2025-01-09 14:59_

---

_@zanieb approved on 2025-01-09 17:39_

Nice :)

---

_Merged by @charliermarsh on 2025-01-09 17:45_

---

_Closed by @charliermarsh on 2025-01-09 17:45_

---

_Branch deleted on 2025-01-09 17:45_

---

_Comment by @konstin on 2025-01-09 19:23_

Two review comments:

Nit: We can remove the duplication and drop the manual pinning in favor of a `collect()`: https://gist.github.com/konstin/7072939997a8713a1db8a219065947c3

Actual problem: If I'm reading the call hierarchy correctly, we're foregoing `ManagedClient::control` here, i.e. there can be (number of indexes) times more requests in parallel than the user requested.

---

_Comment by @charliermarsh on 2025-01-09 19:27_

I don't think that's a huge problem, but also not sure how to fix it given the setup.

---

_Comment by @konstin on 2025-01-10 10:13_

We can either check eagerly and acquire more tokens or pass around a reference to the semaphore and acquire more tokens in the stream.

---
