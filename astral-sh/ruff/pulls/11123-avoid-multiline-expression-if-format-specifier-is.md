```yaml
number: 11123
title: Avoid multiline expression if format specifier is present
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: dhruv/f-string-format-spec
created_at: 2024-04-24T10:35:53Z
updated_at: 2024-04-26T13:37:53Z
url: https://github.com/astral-sh/ruff/pull/11123
synced_at: 2026-01-12T15:55:34Z
```

# Avoid multiline expression if format specifier is present

---

_@dhruvmanila_

## Summary

This PR fixes the bug where the formatter would format an f-string and could potentially change the AST.

For a triple-quoted f-string, the element can't be formatted into multiline if it has a format specifier because otherwise the newline would be treated as part of the format specifier.

Given the following f-string:
```python
f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {
    variable:.3f} ddddddddddddddd eeeeeeee"""
```

The formatter sees that the f-string is already multiline so it assumes that it can contain line breaks i.e., broken into multiple lines. But, in this specific case we can't format it as:

```python
f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {
    variable:.3f
} ddddddddddddddd eeeeeeee"""
```
                     
Because the format specifier string would become ".3f\n", which is not the original string (`.3f`). 

If the original source code already contained a newline, they'll be preserved. For example:
```python
f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {
    variable:.3f
} ddddddddddddddd eeeeeeee"""
```

The above will be formatted as:
```py
f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {variable:.3f
} ddddddddddddddd eeeeeeee"""
```

Note that the newline after `.3f` is part of the format specifier which needs to be preserved.
                                                                                               
The Python version is irrelevant in this case.

fixes: #10040 

## Test Plan

Add some test cases to verify this behavior.


---

_Label `bug` added by @dhruvmanila on 2024-04-24 10:35_

---

_Label `formatter` added by @dhruvmanila on 2024-04-24 10:35_

---

_Comment by @github-actions[bot] on 2024-04-24 10:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:257 on 2024-04-25 10:07_

This is an additional change which isn't relevant to this PR but should be a follow-up. I've included it in here for simplicity. For example,

```py
x = f"aaaaaaaaaaaaaaa {
    {'aaaaaaaaaaaaaaaaaa', 'dddddddddddddddddd'}!s
}"
```

This should be formatted as:
```py
x = f"aaaaaaaaaaaaaaa { {'aaaaaaaaaaaaaaaaaa', 'dddddddddddddddddd'}!s}"
```

Instead of:
```py
x = f"aaaaaaaaaaaaaaa { {'aaaaaaaaaaaaaaaaaa', 'dddddddddddddddddd'}!s }"
#                                                                     ^ this isn't required if there's format spec or conversion spec
```

---

_@dhruvmanila reviewed on 2024-04-25 10:07_

---

_Marked ready for review by @dhruvmanila on 2024-04-25 10:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-25 10:15_

---

_Comment by @MichaReiser on 2024-04-26 07:36_

I'm confused, you say: 

> Note that the newline after .3f is part of the format specifier which needs to be preserved.

```python
f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {variable:.3f
} ddddddddddddddd eeeeeeee"""
```

But the original example had no newline after `3f`.

```python
f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {
    variable:.3f} ddddddddddddddd eeeeeeee"""
```



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:219 on 2024-04-26 07:37_

```suggestion
# But, if it's triple-quoted then we can't or the format specificer will have a
# trailing newline
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:235 on 2024-04-26 07:38_

Lol what so `# comment` here is not a comment :rofl: 

---

_@MichaReiser reviewed on 2024-04-26 07:40_

---

_Comment by @dhruvmanila on 2024-04-26 08:20_

> I'm confused, you say:
> 
> > Note that the newline after .3f is part of the format specifier which needs to be preserved.
> 
> ```python
> f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {variable:.3f
> } ddddddddddddddd eeeeeeee"""
> ```
> 
> But the original example had no newline after `3f`.
> 
> ```python
> f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {
>     variable:.3f} ddddddddddddddd eeeeeeee"""
> ```

The note is for the two snippets above it. The two code snippets you've provided aren't connected. I meant that the following code

```py
f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {
    variable:.3f
} ddddddddddddddd eeeeeeee"""
```

will be formatted as
```diff
-f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {
-    variable:.3f
+f"""aaaaaaaaaaaaaaaa bbbbbbbbbbbbbbbbbb ccccccccccc {variable:.3f
 } ddddddddddddddd eeeeeeee"""
```

The formatter collapsed the f-string expression element but there's still a newline in the format spec which is why you see that the f-string is still multiline.

---

_@dhruvmanila reviewed on 2024-04-26 08:21_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:235 on 2024-04-26 08:21_

Yeah, it isn't but only if it's a triple-quoted f-string

https://play.ruff.rs/b1d0326b-f8dc-4724-870f-db3770fb09c5

---

_@MichaReiser approved on 2024-04-26 10:58_

---

_Merged by @dhruvmanila on 2024-04-26 13:34_

---

_Closed by @dhruvmanila on 2024-04-26 13:34_

---

_Branch deleted on 2024-04-26 13:34_

---
