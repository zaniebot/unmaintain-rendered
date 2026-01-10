```yaml
number: 15116
title: Updating benchmarks
type: issue
state: closed
author: InSyncWithFoo
labels:
  - documentation
assignees: []
created_at: 2024-12-23T03:05:40Z
updated_at: 2024-12-23T09:32:47Z
url: https://github.com/astral-sh/ruff/issues/15116
synced_at: 2026-01-10T11:09:56Z
```

# Updating benchmarks

---

_Issue opened by @InSyncWithFoo on 2024-12-23 03:05_

[Almost 2 years ago](https://github.com/astral-sh/ruff/issues/269) Ruff took 0.29 seconds to lint the CPython codebase from scratch on Charlie's setup. How long does it take now, with significantly more rules and logic?

My own results:

```shell
$ time ruff check --select ALL --preview --silent
real    0m0.534s
user    0m0.544s
sys     0m0.319s

$ time ruff check --select ALL --silent
real    0m0.438s
user    0m0.495s
sys     0m0.199s

$ time ruff check --silent
real    0m0.032s
user    0m0.034s
sys     0m0.047s
```

In any case, the `README.md` graph should be updated.


---

_Label `documentation` added by @dhruvmanila on 2024-12-23 04:20_

---

_Comment by @dhruvmanila on 2024-12-23 04:23_

Yeah, makes sense. The benchmark scripts are located under [`./scripts/benchmarks`](https://github.com/astral-sh/ruff/tree/main/scripts/benchmarks), which should still work.

---

_Comment by @MichaReiser on 2024-12-23 09:02_

I took a quick look and I think we don't need to update them:

| Tool           | Duration Chart | Duration now | Ratio old/now        |
|----------------|----------------|--------------|----------------------|
|   Autoflake    |   6.18         |   2.243      |   0.36294498381877   |
|   Flake8       |   12.26        |   4.6        |   0.375203915171289  |
|   Pyflakes     |   15.79        |   9.37       |   0.593413552881571  |
|   Pycodestyle  |   46.92        |   21.13      |   0.450341005967604  |
|   Pylint       |   60           |   ??           |   0                  |
|   Ruff         |   0.29         |   0.08       |   0.275862068965517  |

It seems Ruff improved in performance more than any of the other tools. This might just be because I use a different machine. 




---

_Closed by @MichaReiser on 2024-12-23 09:02_

---
