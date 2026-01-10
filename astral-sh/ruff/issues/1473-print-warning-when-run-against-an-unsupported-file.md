```yaml
number: 1473
title: Print warning when run against an unsupported file
type: issue
state: closed
author: not-my-profile
labels:
  - bug
  - cli
assignees: []
created_at: 2022-12-30T10:56:29Z
updated_at: 2022-12-31T18:10:58Z
url: https://github.com/astral-sh/ruff/issues/1473
synced_at: 2026-01-10T12:05:29Z
```

# Print warning when run against an unsupported file

---

_Issue opened by @not-my-profile on 2022-12-30 10:56_

```
$ echo 'not json' > foo.json 
$ ruff foo.json 
$ echo $?
0
```

We might also consider returning a non-zero status code since the file isn't supported.

---

_Label `bug` added by @charliermarsh on 2022-12-30 12:22_

---

_Comment by @charliermarsh on 2022-12-30 12:22_

Good idea.

---

_Comment by @charliermarsh on 2022-12-30 12:22_

(And agree -- non-zero.)

---

_Comment by @charliermarsh on 2022-12-30 16:28_

We should probably also warn if we can't find any valid files (but aren't passed an invalid file _directly_).

For example: `ruff foo/` where `foo` is an empty directory, or doesn't contain any `.py` or `.pyi` files.

Here's how Black handles these cases:

```
ruff on  charlie/fix-message:main [$⇡] is 📦 v0.0.202 via 🐍 v3.11.1 (.venv) via 🦀 v1.65.0
❯ black pyproject.toml
error: cannot format pyproject.toml: Cannot parse: 41:8: combine-as-imports = true

Oh no! 💥 💔 💥
1 file failed to reformat.
ruff on  charlie/fix-message:main [$⇡] is 📦 v0.0.202 via 🐍 v3.11.1 (.venv) via 🦀 v1.65.0
❯ black pyproject.toml setup.py
blerror: cannot format pyproject.toml: Cannot parse: 41:8: combine-as-imports = true

Oh no! 💥 💔 💥
1 file left unchanged, 1 file failed to reformat.
ruff on  charlie/fix-message:main [$⇡] is 📦 v0.0.202 via 🐍 v3.11.1 (.venv) via 🦀 v1.65.0
❯ black foo
No Python files are present to be formatted. Nothing to do 😴
ruff on  charlie/fix-message:main [$⇡] is 📦 v0.0.202 via 🐍 v3.11.1 (.venv) via 🦀 v1.65.0
❯ black foo/ setup.py
All done! ✨ 🍰 ✨
1 file left unchanged.
```


---

_Comment by @charliermarsh on 2022-12-30 20:36_

In short:

- (Optional) If we don't find _any_ files, show a warning.
- If we're passed a non `.py` or `.pyi` file, raise an error for that file (but process any valid files).

---

_Closed by @charliermarsh on 2022-12-31 13:12_

---

_Label `cli` added by @charliermarsh on 2022-12-31 18:10_

---
