```yaml
number: 15536
title: "F-string formatting: Instable formatting for tuple with trailing comma"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2025-01-16T17:43:52Z
updated_at: 2025-01-17T09:08:11Z
url: https://github.com/astral-sh/ruff/issues/15536
synced_at: 2026-01-12T15:54:54Z
```

# F-string formatting: Instable formatting for tuple with trailing comma

---

_@MichaReiser_

```py
print(f"{ {}, 1, }")
```

Gets formatted to 

```py
print(
    f"{ {}, 1 }"
)

```

notice that the trailing comma is missing but `print` expands because of the trailing comma's `expand_parent`.

Which then get formatted to 

```py
print(f"{ {}, 1 }")
```

---

_Label `bug` added by @MichaReiser on 2025-01-16 17:43_

---

_Label `formatter` added by @MichaReiser on 2025-01-16 17:43_

---

_Comment by @MichaReiser on 2025-01-16 17:55_

This might be rather annoying to fix. I introduced this regression when I changed `RemoveSoftLines` to remove `if_group_breaks` elements to fix https://github.com/astral-sh/ruff/pull/14489. Maybe that was a bad idea after all, and we should instead have changed the power formatting?

---

_Closed by @MichaReiser on 2025-01-17 09:08_

---
