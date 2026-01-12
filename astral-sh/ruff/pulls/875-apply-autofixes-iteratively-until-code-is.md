```yaml
number: 875
title: Apply autofixes iteratively until code is stabilized
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/iterative-fix
created_at: 2022-11-22T21:03:20Z
updated_at: 2022-11-22T21:37:53Z
url: https://github.com/astral-sh/ruff/pull/875
synced_at: 2026-01-12T05:48:46Z
```

# Apply autofixes iteratively until code is stabilized

---

_Pull request opened by @charliermarsh on 2022-11-22 21:03_

Resolves #868, #866, #660.


---

_Comment by @JonathanPlasse on 2022-11-22 21:09_

How does it impact performance?

---

_Comment by @charliermarsh on 2022-11-22 21:34_

Seems pretty fast! Certainly within the margin of error, possibly even faster because of some other changes in the PR?

The nice thing is that we only need to re-lint files that _changed_, and we don't actually have to write and read from disk every time we re-lint -- we can just keep the changed contents in memory.

Before:

```
∴ hyperfine --warmup 10 --runs 25 -i --cleanup "git reset --hard HEAD" "../../../target/release/ruff --no-cache --fix ."
Benchmark 1: ../../../target/release/ruff --no-cache --fix .
  Time (mean ± σ):     308.2 ms ±   7.5 ms    [User: 2475.2 ms, System: 92.0 ms]
  Range (min … max):   302.5 ms … 342.4 ms    25 runs
```

After:

```
∴ hyperfine --warmup 10 --runs 25 -i --cleanup "git reset --hard HEAD" "../../../target/release/ruff --no-cache --fix ."
Benchmark 1: ../../../target/release/ruff --no-cache --fix .
  Time (mean ± σ):     304.1 ms ±   2.7 ms    [User: 2449.3 ms, System: 87.2 ms]
  Range (min … max):   300.0 ms … 309.7 ms    25 runs
```

---

_Merged by @charliermarsh on 2022-11-22 21:37_

---

_Closed by @charliermarsh on 2022-11-22 21:37_

---

_Branch deleted on 2022-11-22 21:37_

---
