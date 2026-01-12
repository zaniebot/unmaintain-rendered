```yaml
number: 9980
title: Missing whitespace/newline detection at end of a long string split over multiple lines
type: issue
state: open
author: swaldhoer
labels:
  - rule
  - wish
assignees: []
created_at: 2024-02-14T09:24:02Z
updated_at: 2024-02-14T14:59:27Z
url: https://github.com/astral-sh/ruff/issues/9980
synced_at: 2026-01-12T15:54:49Z
```

# Missing whitespace/newline detection at end of a long string split over multiple lines

---

_@swaldhoer_

Consider this long string:

```python
print("This is a long string. Yeah it's really long. I think I need multiple lines to have a meaningful formatting.")
```

Most times, you will split this into something like this to have a meaningful formatting in an acceptable line length

```python
print(
    "This is a long string. Yeah it's really long."
    "I think I need multiple lines to have a meaningful formatting."
)
```

I missed the trailing whitespace or a newline at the end of the second sentence

```python
print(
    "This is a long string. Yeah it's really long."
                                                # ^ [ ] or \n is missing
    "I think I need multiple lines to have a meaningful formatting."
)
```

and this would create an undesired string:

```diff
-This is a long string. Yeah it's really long. I think I need multiple lines to have a meaningful formatting.
+This is a long string. Yeah it's really long.I think I need multiple lines to have a meaningful formatting.
```

I would suggest what Ruff checks on *split* strings, that all lines except the last line end in `[ ]"` or in `\n"` as the version without whitespace or newline is basically never desired.


---

_Label `rule` added by @MichaReiser on 2024-02-14 14:59_

---

_Label `wish` added by @MichaReiser on 2024-02-14 14:59_

---
