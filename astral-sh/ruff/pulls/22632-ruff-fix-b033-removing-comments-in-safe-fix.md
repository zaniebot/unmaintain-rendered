```yaml
number: 22632
title: "[`ruff`] Fix B033 removing comments in safe fix"
type: pull_request
state: open
author: leandrobbraga
labels: []
assignees: []
base: main
head: bugfix/b033-unsafe-fix
created_at: 2026-01-16T22:36:30Z
updated_at: 2026-01-16T22:58:13Z
url: https://github.com/astral-sh/ruff/pull/22632
synced_at: 2026-01-16T23:05:55Z
```

# [`ruff`] Fix B033 removing comments in safe fix

---

_@leandrobbraga_

The B033 erroneously removes comments in safe fix.

By running
```console
$ cargo run -p ruff  -- check --select B033 - << 'EOF'
{
  1,
  # comment
  1
}
EOF
```

We get:

# Before

```console
B033 [*] Sets should not contain duplicate item `1`
 --> -:4:3
  |
2 |   1,
3 |   # comment
4 |   1
  |   ^
5 | }
  |
help: Remove duplicate item

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

# After
```console
B033 Sets should not contain duplicate item `1`
 --> -:4:3
  |
2 |   1,
3 |   # comment
4 |   1
  |   ^
5 | }
  |
help: Remove duplicate item

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Closes #22629

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 22:48_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---
