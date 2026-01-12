```yaml
number: 9958
title: "Formatter trims empty lines after `;` "
type: issue
state: open
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-02-12T18:06:50Z
updated_at: 2024-03-27T22:36:45Z
url: https://github.com/astral-sh/ruff/issues/9958
synced_at: 2026-01-12T15:54:49Z
```

# Formatter trims empty lines after `;` 

---

_@MichaReiser_

```Python
b = """
    This looks like a docstring but is not in a notebook because notebooks can't be imported as a module.
    Ruff should leave it as is
""";


a = "another normal string"
```

The formatter should preserve the empty lines between `b` and `a` but it does not today.

[Playground](https://discord.com/channels/1039017663004942429/1082324263199064206/1206662519012458526)

---

_Label `bug` added by @MichaReiser on 2024-02-12 18:06_

---

_Label `formatter` added by @MichaReiser on 2024-02-12 18:06_

---

_Comment by @dhruvmanila on 2024-02-15 05:09_

Giving a quick 10 second look, the issue is probably on this line:

https://github.com/astral-sh/ruff/blob/fe79798c12b4771cee0b0c59964ad7bd751c3779/crates/ruff_python_formatter/src/statement/suite.rs#L419-L420

As `lines_after` doesn't take into account the `;` token which results in the value being 0 and then the hard line break is added instead.

Maybe we should use `lines_after_ignoring_end_of_line_trivia` but that doesn't ignore `;` either.

---

_Comment by @robincaloudis on 2024-03-27 22:32_

@MichaReiser, followed what @dhruvmanila suggested. Worked out nicely. Adapted some tests as well. If you find the time, please review the [fix](https://github.com/astral-sh/ruff/pull/10639). Thanks!

---
