```yaml
number: 6762
title: "Formatter: Preserve parentheses of yielded value"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-22T09:24:51Z
updated_at: 2023-08-22T14:20:00Z
url: https://github.com/astral-sh/ruff/issues/6762
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: Preserve parentheses of yielded value

---

_@MichaReiser_

Black seems (need confirmation) to preserve parentheses around the yielded value:

## Input / Output
```python
yield ("Cache key will cause errors if used with memcached: %r " "(longer than %s)" % (
        key,
        MEMCACHE_MAX_KEY_LENGTH,
     )
)

yield "Cache key will cause errors if used with memcached: %r " "(longer than %s)" % (
    key,
    MEMCACHE_MAX_KEY_LENGTH,
)
 
 
yield ("Unnecessary")


yield (
    "#   * Make sure each ForeignKey and OneToOneField has `on_delete` set "
    "to the desired behavior"
)
yield (
    "#   * Remove `managed = False` lines if you wish to allow "
    "Django to create, modify, and delete the table"
)
yield (
    "# Feel free to rename the models, but don't rename db_table values or "
    "field names."
)

yield "#   * Make sure each ForeignKey and OneToOneField has `on_delete` set "    "to the desired behavior"
yield "#   * Remove `managed = False` lines if you wish to allow " "Django to create, modify, and delete the table"
yield "# Feel free to rename the models, but don't rename db_table values or "    "field names."
```

## Ruff

Always removes the parentheses.


---

_Label `formatter` added by @MichaReiser on 2023-08-22 09:24_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-08-22 10:07_

---

_Closed by @MichaReiser on 2023-08-22 10:27_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:20_

---
