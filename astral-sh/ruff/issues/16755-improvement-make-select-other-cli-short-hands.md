---
number: 16755
title: "[IMPROVEMENT] Make `--select` other CLI short hands more lenient with whitespace"
type: issue
state: open
author: lenormf
labels:
  - cli
  - wish
assignees: []
created_at: 2025-03-14T16:27:40Z
updated_at: 2025-03-15T08:00:05Z
url: https://github.com/astral-sh/ruff/issues/16755
synced_at: 2026-01-10T01:22:58Z
---

# [IMPROVEMENT] Make `--select` other CLI short hands more lenient with whitespace

---

_Issue opened by @lenormf on 2025-03-14 16:27_

### Summary

Hi!

I would like the `--select`, `--ignore` and other short hand options passed to `ruff check` to be more lenient with whitespace characters, between rule codes.

I found myself writing a PreCommit configuration, like so:

```yaml
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.11.0
    hooks:
       - id: ruff
         args:
           - --fix
           - |-
               --select=
               A,
               B
```

This lead to the hook invoking `ruff check --fix --select=' A, B' <file>`, with the result below:

```
error: invalid value ' A' for '--select <RULE_CODE>'
```

I couldn’t find any syntax in YAML that would allow joining a string within a block sequence item _without_ space characters.

Therefore, I believe it would be a convenient improvement to allow whitespace in the comma-separated list of rules that `ruff check --<shorthand>` accepts.

Thanks!

### Version

0.11.0

---

_Comment by @ntBre on 2025-03-14 17:17_

This may not be a perfect workaround, but it looks like ruff allows multiple `--select` flags, so you could instead do something like

```yaml
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.11.0
    hooks:
       - id: ruff
         args:
           - --fix
           - |-
               --select=A
               --select=B
```

(If my limited YAML syntax understanding is sufficient here)

Another option would be putting these in a `ruff.toml` or other config file, but maybe that doesn't work well with pre-commit.

We could still consider trimming spaces, but these might help in the meantime.

---

_Label `cli` added by @ntBre on 2025-03-14 17:17_

---

_Label `wish` added by @MichaReiser on 2025-03-14 18:00_

---

_Comment by @lenormf on 2025-03-15 08:00_

Thank you for the reply!

Unfortunately, in my case, I cannot rely on a local `ruff.toml` file to simplify things.

I also have many rules selected/ignored, so instead of passing each of them with their individual `--{select,ignore}=` flag, I have long strings `"--{select,ignore}=A,B,C,D…"` in the YAML manifest.

It works©, but I expected my suggestion would be easy enough to implement to have thrown in with the next release.

I’ll wait :)

---
