```yaml
number: 14472
title: "Redundant quotes are kept while `format`ting a string from multiple lines to one"
type: issue
state: closed
author: yury-fedotov
labels: []
assignees: []
created_at: 2024-11-20T01:39:14Z
updated_at: 2024-11-20T07:15:59Z
url: https://github.com/astral-sh/ruff/issues/14472
synced_at: 2026-01-12T15:54:53Z
```

# Redundant quotes are kept while `format`ting a string from multiple lines to one

---

_@yury-fedotov_

## Description

Ruff formatter seems to keep redundant quotes at the edges of lines when collapsing them into one line.

> P.S. Maybe it's me not using multiline strings properly, and current behavior is expected.

## MRE

With `line-length` set at `100`, running `ruff format` gives this diff:

```diff
- st.warning(
-     "This page is not implemented yet,"
-     " and is just a placeholder to test navigation."
- )
+ st.warning("This page is not implemented yet," " and is just a placeholder to test navigation.")
```

With redundant `" "` in the middle.

## References and context

* List of keywords you searched for before creating this issue: `multiline`
* The command you invoked: `ruff format`
* The current Ruff version: `0.7.1`

---

_Comment by @dylwil3 on 2024-11-20 02:20_

Would the lint rule [ISC001](https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/) achieve what you're after here?

---

_Comment by @yury-fedotov on 2024-11-20 02:58_

> Would the lint rule [ISC001](https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/) achieve what you're after here?

Good point! Let me close the issue then - I think the solution against this problem is as you stated, just use `ISC001`.

---

_Closed by @yury-fedotov on 2024-11-20 02:58_

---

_Comment by @MichaReiser on 2024-11-20 07:15_

Ruff's formatter will join the two strings in an upcoming release early next year. See https://github.com/astral-sh/ruff/pull/13663

---
