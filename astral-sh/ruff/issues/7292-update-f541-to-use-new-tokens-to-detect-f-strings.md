```yaml
number: 7292
title: "Update `F541` to use new tokens to detect f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
created_at: 2023-09-12T09:42:55Z
updated_at: 2023-09-18T16:44:47Z
url: https://github.com/astral-sh/ruff/issues/7292
synced_at: 2026-01-12T15:54:47Z
```

# Update `F541` to use new tokens to detect f-strings

---

_@dhruvmanila_

`F541` checks for useless f-strings i.e., f-strings without any placeholders.

The rule needs to be updated to use the new f-string tokens to detect that.

### Possible solution

Once we encounter the `FStringStart` token and if there are no placeholders inside the f-string, then the next token is guaranteed going to a `FStringMiddle` token containing the entire content of the f-string and then we'll get the `FStringEnd` token. For example,

```python
f"we're inside a f-string"
```

The above code will emit the following tokens:
```
0 FStringStart 2
2 FStringMiddle {
    value: "we're inside a f-string",
    is_raw: false,
} 25
25 FStringEnd 26
```

And, for triple-quoted f-strings:
```python
f"""this is a triple
quoted f-string with
multiple lines of text"""
```

We get the following tokens:

```
0 FStringStart 4
4 FStringMiddle {
    value: "this is a triple\nquoted f-string with\nmultiple lines of text",
    is_raw: false,
} 64
64 FStringEnd 67
```

I propose to create two edits:
1. Remove the f-string prefix. This can be extracted using the `FStringStart` token
2. Unescape the f-string using the `FStringMiddle` token i.e., replacing `{{` and `}}` with `{` and `}`

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:46_

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:46_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-13 04:19_

---

_Closed by @dhruvmanila on 2023-09-18 16:44_

---
