```yaml
number: 20846
title: Fix syntax error false positives on parenthesized context managers
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: brent/fix-context-manager-3.8
created_at: 2025-10-13T14:27:05Z
updated_at: 2025-10-13T18:13:29Z
url: https://github.com/astral-sh/ruff/pull/20846
synced_at: 2026-01-12T15:57:11Z
```

# Fix syntax error false positives on parenthesized context managers

---

_@ntBre_

This PR resolves the issue noticed in https://github.com/astral-sh/ruff/pull/20777#discussion_r2417233227. Namely, cases like this were being flagged as syntax errors despite being perfectly valid on Python 3.8:

```pycon
Python 3.8.20 (default, Oct  2 2024, 16:34:12)
[Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> with (open("foo.txt", "w")): ...
...
Ellipsis
>>> with (open("foo.txt", "w")) as f: print(f)
...
<_io.TextIOWrapper name='foo.txt' mode='w' encoding='UTF-8'>
```

The second of these was already allowed but not the first:

```shell
> ruff check --target-version py38 --ignore ALL - <<EOF
with (open("foo.txt", "w")): ...
with (open("foo.txt", "w")) as f: print(f)
EOF
invalid-syntax: Cannot use parentheses within a `with` statement on Python 3.8 (syntax was added in Python 3.9)
 --> -:1:6
  |
1 | with (open("foo.txt", "w")): ...
  |      ^
2 | with (open("foo.txt", "w")) as f: print(f)
  |

Found 1 error.
```

There was some discussion of related cases in https://github.com/astral-sh/ruff/pull/16523#discussion_r1984657793, but it seems I overlooked the single-element case when flagging tuples. As suggested in the other thread, we can just check if there's more than one element or a trailing comma, which will cause the tuple parsing on <=3.8 and avoid  the false positives.


---

_Label `bug` added by @ntBre on 2025-10-13 14:27_

---

_Label `parser` added by @ntBre on 2025-10-13 14:27_

---

_Comment by @github-actions[bot] on 2025-10-13 14:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-10-13 14:47_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-13 14:47_

---

_Review requested from @dhruvmanila by @ntBre on 2025-10-13 14:47_

---

_@MichaReiser approved on 2025-10-13 17:41_

---

_Merged by @ntBre on 2025-10-13 18:13_

---

_Closed by @ntBre on 2025-10-13 18:13_

---

_Branch deleted on 2025-10-13 18:13_

---
