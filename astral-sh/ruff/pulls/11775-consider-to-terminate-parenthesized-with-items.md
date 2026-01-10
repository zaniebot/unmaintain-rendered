```yaml
number: 11775
title: "Consider `:` to terminate parenthesized with items"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/with-items
created_at: 2024-06-06T11:44:39Z
updated_at: 2024-06-06T13:10:45Z
url: https://github.com/astral-sh/ruff/pull/11775
synced_at: 2026-01-10T21:56:00Z
```

# Consider `:` to terminate parenthesized with items

---

_Pull request opened by @dhruvmanila on 2024-06-06 11:44_

## Summary

This PR is a follow-up to this discussion (https://github.com/astral-sh/ruff/pull/11770#discussion_r1628917209) which adds the `:` token in the terminator set for parenthesized with items.

The main motivation is to avoid parsing too much in speculative mode. This is evident with the following _before_ and _after_ parsed with items list for the following code:

```py
with (item1, item2:
    foo
```

<table>
  <tr>
    <th>Before (3 items)</th>
    <th>After (2 items)</th>
  </tr>
  <tr>
    <td>
<pre>
parsed_with_items: [
    ParsedWithItem {
        item: WithItem {
            range: 6..11,
            context_expr: Name(
                ExprName {
                    range: 6..11,
                    id: "item1",
                    ctx: Load,
                },
            ),
            optional_vars: None,
        },
        is_parenthesized: false,
    },
    ParsedWithItem {
        item: WithItem {
            range: 13..18,
            context_expr: Name(
                ExprName {
                    range: 13..18,
                    id: "item2",
                    ctx: Load,
                },
            ),
            optional_vars: None,
        },
        is_parenthesized: false,
    },
    ParsedWithItem {
        item: WithItem {
            range: 24..27,
            context_expr: Name(
                ExprName {
                    range: 24..27,
                    id: "foo",
                    ctx: Load,
                },
            ),
            optional_vars: None,
        },
        is_parenthesized: false,
    },
]
</pre>
	</td>
    <td>
<pre>
parsed_with_items: [
    ParsedWithItem {
        item: WithItem {
            range: 6..11,
            context_expr: Name(
                ExprName {
                    range: 6..11,
                    id: "item1",
                    ctx: Load,
                },
            ),
            optional_vars: None,
        },
        is_parenthesized: false,
    },
    ParsedWithItem {
        item: WithItem {
            range: 13..18,
            context_expr: Name(
                ExprName {
                    range: 13..18,
                    id: "item2",
                    ctx: Load,
                },
            ),
            optional_vars: None,
        },
        is_parenthesized: false,
    },
]
</pre>
	</td>
  </tr>
</table>

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-06 11:44_

---

_Label `parser` added by @dhruvmanila on 2024-06-06 11:44_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-06 11:44_

---

_Comment by @github-actions[bot] on 2024-06-06 12:04_

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

_@MichaReiser approved on 2024-06-06 12:39_

---

_Merged by @dhruvmanila on 2024-06-06 13:10_

---

_Closed by @dhruvmanila on 2024-06-06 13:10_

---

_Branch deleted on 2024-06-06 13:10_

---
