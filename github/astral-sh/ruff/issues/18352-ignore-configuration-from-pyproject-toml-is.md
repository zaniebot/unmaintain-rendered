---
number: 18352
title: ignore configuration from pyproject.toml is ignored when using --select from the CLI
type: issue
state: closed
author: YoniPrv
labels:
  - question
assignees: []
created_at: 2025-05-28T12:05:51Z
updated_at: 2025-06-24T11:34:48Z
url: https://github.com/astral-sh/ruff/issues/18352
synced_at: 2026-01-07T13:12:16-06:00
---

# ignore configuration from pyproject.toml is ignored when using --select from the CLI

---

_Issue opened by @YoniPrv on 2025-05-28 12:05_

### Summary

Hi all,

I don't know if this is a bug or indented behavior but when i use the select statement it tends to ignore the ignore configuration in my toml.
Example toml:
```
[tool.ruff.lint]
select = ["D", "PL", "F", "E", "I", "S", "DTZ"]
ignore = ["S101"]
```
with `ruff check | grep S101` i get no results and the S101 gets ignored

But when I execute `ruff check --select=S | grep S101` I get results back
```
$ ruff check --select=S | grep S101
abc.py:34:9: S101 Use of `assert` detected
   |         ^^^^^^ S101
```

Is it normal behavior that ignore is ignored when using the select statement?

### Version

ruff 0.11.11

---

_Comment by @MichaReiser on 2025-05-28 12:08_

That's intended. you have to use `--extend-select` to extend the selection from your configuration

---

_Label `question` added by @MichaReiser on 2025-05-28 12:08_

---

_Comment by @YoniPrv on 2025-05-28 12:12_

```
$ ruff check --extend-select=S | grep S101
abc.py:34:9: S101 Use of `assert` detected
   |         ^^^^^^ S101
```

this also returns S101 errors?

---

_Comment by @MichaReiser on 2025-05-28 12:18_

Oh sorry, I was a bit too quick with replying. 

This is still intentional. `--select` takes precedence over the `ignore` in the configuration. You'd have to write `--select S --ignore S101` or ignore all rules except `S` `--ignore D --ignore PL --ignore F --ignore E --ignore I --ignore DTZ`

---

_Comment by @YoniPrv on 2025-05-28 13:00_

Oh. Thats a bummer.
We are using a pre-commit configuration and splitted it in multiple sections (fix/exit-zero) for our ci/cd pipeline to make it not crash on errors we don't want. 
e.g.

```
      - id: ruff
        name: ruff-bandit
        verbose: true
        args: ["--select=S", "--exit-zero"]

      - id: ruff
        name: ruff-datetime
        args: ["--select=DTZ", "--fix"] 
        types_or: [python, pyi]

```
At the moment we have multiple ignores in the .toml file so that vscode can use them. 
This means we have to duplicate our ignores in our precommit configuration as well as in our toml?

---

_Comment by @charliermarsh on 2025-05-28 13:07_

Yes. It would only matter for rules that overlap, though. Like, I don't think you need to do `--select S --ignore D`. You'd just need `--select S --ignore S101`.

---

_Closed by @YoniPrv on 2025-06-24 11:34_

---
