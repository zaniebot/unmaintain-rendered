```yaml
number: 3939
title: "Allow for deselection of all fixable errors when `check`ing"
type: issue
state: open
author: thejcannon
labels:
  - cli
assignees: []
created_at: 2023-04-11T20:53:27Z
updated_at: 2023-04-12T04:04:32Z
url: https://github.com/astral-sh/ruff/issues/3939
synced_at: 2026-01-12T15:54:44Z
```

# Allow for deselection of all fixable errors when `check`ing

---

_@thejcannon_

:wave: 

Over in https://www.pantsbuild.org/ we're adding `ruff` support. Only one nit so far. We separate running tools that fix code from ones that only lint. Since `ruff` does both, the user will see `fix`able ruff errors when `lint`ing AND they'll see our "ruff made changes when trying to fix".

So ideally, in `lint` mode, we turn off the fixable errors. Not a dealbreaker, just a confusing message to our users.

---

_Comment by @charliermarsh on 2023-04-11 21:24_

To confirm my understanding: is the desired behavior here such that fixable errors don't appear at all when running `ruff check`? Or is it that we omit the `[*]` decoration and the `[*] 99 potentially fixable with the --fix option.` message?

---

_Comment by @thejcannon on 2023-04-11 21:26_

The latter is nice for error messages. The former is (and I almost hesitate to say this, knowing full well this is _ruff_) for performance and reduces the noise.

---

_Comment by @thejcannon on 2023-04-11 21:26_

... so i guess the former 😅

---

_Comment by @thejcannon on 2023-04-12 01:43_

And to give an example, a file containing only `f"I'm missing placeholders"` leads to:
```
20:40:49.88 [ERROR] Completed: Lint with ruff - ruff failed (exit code 1).
src/python/pants/util/foo.py:1:1: F541 [*] f-string without any placeholders
Found 1 error.
[*] 1 potentially fixable with the --fix option.



20:40:49.90 [WARN] Completed: Fix with ruff - ruff --fix made changes.
  src/python/pants/util/foo.py

✕ ruff failed.
✕ ruff --fix failed.

(One or more fixers failed. Run `pants fix` to fix.)
```

What your seeing is us:
- running `ruff --fix` in one process, and noticing the file changed, and therefore warning+failing
- running `ruff` in another process, and just parroting the stdout/stderr

(We do the `fix` thing so that running the `fix` goal before the `lint` goal means we can re-use the process cache for the invocation. Makes it speedy for those "other" fixers that aren't as speedy as `ruff`)

---

_Label `cli` added by @charliermarsh on 2023-04-12 04:04_

---
