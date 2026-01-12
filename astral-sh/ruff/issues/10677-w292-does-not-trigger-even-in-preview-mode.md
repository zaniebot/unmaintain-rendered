```yaml
number: 10677
title: W292 does not trigger even in preview mode
type: issue
state: closed
author: yrahul3910
labels:
  - question
assignees: []
created_at: 2024-03-30T23:25:49Z
updated_at: 2024-04-01T03:44:48Z
url: https://github.com/astral-sh/ruff/issues/10677
synced_at: 2026-01-12T15:54:50Z
```

# W292 does not trigger even in preview mode

---

_@yrahul3910_

I want to configure Ruff so that it ensures one empty newline at the end of each file in Python. This should be `W292` (`missing-newline-at-end-of-file`), and is implemented in preview mode (so my understanding goes from #2402). My current `pyproject.toml` is below.

```toml
[tool.ruff]
exclude = [
    ".DS_Store",
    ".git",
    "__pycache__",
    ".mypy_cache",
    ".ruff_cache"
]

line-length = 120
target-version = "py312"

[tool.ruff.lint]
preview = true
select = ["E", "W", "F", "I001", "RUF002", "RUF100", "RUF013", "RUF010", "RUF200"]
extend-select = ["W292"]

[tool.ruff.lint.isort]
lines-after-imports = 2
lines-between-types = 1
section-order = ["future", "standard-library", "first-party", "local-folder", "third-party"]

[tool.ruff.lint.flake8-quotes]
inline-quotes = "single"

[tool.ruff.lint.pycodestyle]
max-doc-length = 120
```

A MWE is fairly straightforward:

```sh
echo "print('Hello World!')" >> main.py
```

And then running `ruff check .` (even with `--isolated`) yields, "All checks passed!". I double-checked, and `wc -l` gives me 1 line in `main.py`. I am using Ruff 0.3.4, installed via pip, on macOS Sonoma 14.4.1.

---

_Comment by @charliermarsh on 2024-04-01 02:40_

I believe `echo` does write a newline by default. With `echo -n`, the rule is working as intended in my testing:

```
ruff on  main [$] is 📦 v0.3.4 via 🐍 v3.11.8 via 🦀 v1.77.0
❯ echo -n "print('Hello World!')" >> main.py
ruff on  main [$?] is 📦 v0.3.4 via 🐍 v3.11.8 via 🦀 v1.77.0
❯ wc -l main.py
       0 main.py
ruff on  main [$?] is 📦 v0.3.4 via 🐍 v3.11.8 via 🦀 v1.77.0
❯ cargo run check main.py --select W --preview -n
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff check main.py --select W --preview -n`
main.py:1:22: W292 [*] No newline at end of file
  |
1 | print('Hello World!')
  |                       W292
  |
  = help: Add trailing newline

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Do you mind trying again?


---

_Label `question` added by @charliermarsh on 2024-04-01 02:41_

---

_Comment by @yrahul3910 on 2024-04-01 02:45_

Oh god, apparently I'm just dumb. I didn't realize `wc -l` counted newline characters (and in your case, `wc -l` prints 0). In my actual project, it turns out neovim just didn't show line numbers for the empty newline at the end, so I was incredibly confused. Sorry about that, I'll close this issue.

---

_Closed by @yrahul3910 on 2024-04-01 02:45_

---

_Comment by @charliermarsh on 2024-04-01 03:44_

I didn't realize either, I learned it while looking into this :) All good!

---
