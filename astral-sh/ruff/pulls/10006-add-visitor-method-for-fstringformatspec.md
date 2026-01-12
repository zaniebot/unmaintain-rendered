```yaml
number: 10006
title: "Add visitor method for `FStringFormatSpec`"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
assignees: []
base: main
head: dhruv/visit-f-string-format-spec
created_at: 2024-02-16T10:05:25Z
updated_at: 2024-02-16T11:12:57Z
url: https://github.com/astral-sh/ruff/pull/10006
synced_at: 2026-01-12T15:55:30Z
```

# Add visitor method for `FStringFormatSpec`

---

_@dhruvmanila_

## Summary

This PR adds visitor methods for the `FStringFormatSpec` node.

Take the following code snippet as an example:

```python
f"foo {
    foo
    :.3f
    # comment
}"
```

The `# comment` is in a valid position. Now, the formatter attaches it to the last `FStringElement` in the format spec which could either be literal or expression (it's literal in the above example). Adding an explicit visitor method removes this ambiguity so that the formatter always attaches the comment to the outer `FStringFormatSpec`.

---

_Label `internal` added by @dhruvmanila on 2024-02-16 10:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-02-16 10:05_

---

_Comment by @dhruvmanila on 2024-02-16 10:07_

@MichaReiser Oh, I just saw your comment in the f-string PR. I'll put this on hold.

---

_Comment by @github-actions[bot] on 2024-02-16 10:23_

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

_Comment by @dhruvmanila on 2024-02-16 11:12_

I'll put this on hold for now. I'll revisit when sorting out the string nodes.

---

_Closed by @dhruvmanila on 2024-02-16 11:12_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-02-16 11:12_

---
