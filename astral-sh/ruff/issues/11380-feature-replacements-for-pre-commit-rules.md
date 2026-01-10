```yaml
number: 11380
title: "Feature: Replacements for pre-commit rules"
type: issue
state: closed
author: glensc
labels: []
assignees: []
created_at: 2024-05-12T15:59:59Z
updated_at: 2024-07-17T06:33:52Z
url: https://github.com/astral-sh/ruff/issues/11380
synced_at: 2026-01-10T11:09:53Z
```

# Feature: Replacements for pre-commit rules

---

_Issue opened by @glensc on 2024-05-12 15:59_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

pre-commit is rather slow, so I'm looking for to replace this all:

```yml
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-json
      - id: check-added-large-files
      - id: fix-byte-order-marker
      - id: check-builtin-literals
      - id: check-case-conflict
      - id: check-executables-have-shebangs
      - id: check-shebang-scripts-are-executable
      - id: check-merge-conflict
      - id: check-vcs-permalinks
      - id: destroyed-symlinks
      - id: mixed-line-ending
```

Source: https://github.com/Taxel/PlexTraktSync/blob/ab773cc0b3dc1c3c7efbf8509e5627d1dba1d3dd/.pre-commit-config.yaml#L5-L21

---

_Comment by @glensc on 2024-05-12 16:03_

Replacements found so far:

<table>
<tr>
 <th>pre-commit
 <th>ruff
<tr>
 <td><a href="https://github.com/pre-commit/pre-commit-hooks#trailing-whitespace">trailing-whitespace</a>
 <td><a href="https://docs.astral.sh/ruff/rules/trailing-whitespace/">trailing-whitespace (W291)</a>
<tr>
 <td><a href="https://github.com/pre-commit/pre-commit-hooks#end-of-file-fixer">end-of-file-fixer</a>
 <td><a href="https://docs.astral.sh/ruff/rules/missing-newline-at-end-of-file/">missing-newline-at-end-of-file (W292)</a>
<tr>
 <td><a href="https://github.com/pre-commit/pre-commit-hooks#check-builtin-literals">check-builtin-literals</a>
 <td><a href="https://docs.astral.sh/ruff/rules/native-literals/">native-literals (UP018)</a>
<tr>
 <td><a href="https://github.com/pre-commit/pre-commit-hooks#check-executables-have-shebangs">check-executables-have-shebangs</a>
 <td><a href="https://docs.astral.sh/ruff/rules/shebang-not-executable/">shebang-not-executable (EXE001)</a>
<tr>
 <td><a href="https://github.com/pre-commit/pre-commit-hooks#check-shebang-scripts-are-executable">check-shebang-scripts-are-executable</a>
 <td><a href="https://docs.astral.sh/ruff/rules/shebang-missing-executable-file/">shebang-missing-executable-file (EXE002)</a>
<tr>
 <td><a href="https://github.com/pre-commit/pre-commit-hooks#mixed-line-ending
">mixed-line-ending</a>
 <td><a href="https://docs.astral.sh/ruff/settings/#format_line-ending"><tt>line-ending = auto</tt></a>
</table>




---

_Comment by @trim21 on 2024-05-13 06:50_

Don't think ruff should support this excpet check-builtin-literals...

---

_Comment by @glensc on 2024-05-15 17:57_

Yes, these checks are specific to git commits, i.e do not belong to python linter:
      - id: check-added-large-files
      - id: check-case-conflict
      - id: check-merge-conflict
      - id: check-vcs-permalinks
      - id: destroyed-symlinks

also, these checks are for other language, so useful in git commit hook, but not useful in python linter
      - id: check-yaml
      - id: check-json

not sure of
      - id: fix-byte-order-marker


---

_Comment by @worldmind on 2024-05-21 17:54_

A bit related ticket https://github.com/astral-sh/ruff/issues/10227

---

_Comment by @Avasam on 2024-07-16 23:39_

Is this a duplicate of #4073 ?

---

_Closed by @MichaReiser on 2024-07-17 06:33_

---
