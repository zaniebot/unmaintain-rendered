```yaml
number: 17155
title: "Formatter: keep hanging indent for wrapped string"
type: issue
state: closed
author: vasily-v-ryabov
labels: []
assignees: []
created_at: 2025-04-02T15:42:18Z
updated_at: 2025-04-02T15:46:59Z
url: https://github.com/astral-sh/ruff/issues/17155
synced_at: 2026-01-10T11:09:58Z
```

# Formatter: keep hanging indent for wrapped string

---

_Issue opened by @vasily-v-ryabov on 2025-04-02 15:42_

### Summary

`ruff format` removes hanging indent for a multi-line wrapped string. It makes code less readable, because distinguishing a comma at the end is much harder to visually count a real number of strings in a collection.

Example:
```sh
python -m ruff format --diff
--- str_wrap_hanging_indent.py
+++ str_wrap_hanging_indent.py
@@ -1,6 +1,6 @@
 some_dict = {
-    'This is the first line which well fits into one line of code',
-    'This is the second very long and very annoying line for the '
-        'demonstration of Ruff Formatter issue with hanging indent',
-    'This is the third line which well fits into one line of code',
+    "This is the first line which well fits into one line of code",
+    "This is the second very long and very annoying line for the "
+    "demonstration of Ruff Formatter issue with hanging indent",
+    "This is the third line which well fits into one line of code",
 }

1 file would be reformatted
```
This dict contains 3 strings, but after applying `ruff format` it looks like 4 strings.

This is the most painful issue that stops us from using `ruff format` while `ruff check` is amazing!

I'm not sure if this case may conflict with other cases. Having an `[format]` option like `string-wrap-with-hanging-indent=false` by default is also OK to me.

This case comes from old PyLint 2.x rule W9500 `incorrect-substring-position` which I couldn't find in the newest PyLint docs. It affects hundreds of lines in our project.

BTW, one more example is necessary to highlight the expected result:
```python
some_dict = {
    'This is the first line which well fits into one line of code',
    'This is the second very long and very annoying line for the '
        'demonstration of Ruff Formatter issue with hanging indent '
        'wrapped twice in total',
    'This is the third line which well fits into one line of code',
}
```

Of course, any thoughts and links how such case may be handled other way are welcome.

---

_Comment by @MichaReiser on 2025-04-02 15:46_

Agree, this makes the code less readable. I want to change how we format nested expressions in general, I just haven't gotten to it. This is tracked in https://github.com/astral-sh/ruff/issues/12856

---

_Closed by @MichaReiser on 2025-04-02 15:46_

---
