```yaml
number: 7291
title: "Detect `noqa` directives inside f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - suppression
  - python312
assignees: []
created_at: 2023-09-12T09:42:51Z
updated_at: 2023-09-21T02:30:24Z
url: https://github.com/astral-sh/ruff/issues/7291
synced_at: 2026-01-12T15:54:47Z
```

# Detect `noqa` directives inside f-strings

---

_@dhruvmanila_

`noqa` comments for strings goes at the end of it. Similarly, for multi-line strings (triple-quoted), the `noqa` directive is expected on the last line of the string.

Now, with the new f-string specification, multi-line _expressions_ are allowed in a single-quoted f-string. For example, the following code is valid in Python 3.12 but invalid before 3.12:

```python
f"foo {
    a
        *
            b
}"
```

We need to make sure to add the `noqa` directives at the end of f-string regardless if it's single or triple-quoted. For the above example, the outcome would be:

```python
f"foo {
    a
        *
            b
}"  # noqa: F821
```

The [`extract_noqa_line_for`](https://github.com/astral-sh/ruff/blob/a4a4616f0a49c6b1ff7da93a71308d897d27d21f/crates/ruff/src/directives.rs#L89-L89) function needs to be updated to fix this. Currently, it only checks for tripled-quoted strings using the `String` token. Now, we will need to use the `FStringStart` token to (1) check if it's a tripled-quoted f-string, if it is then (2) get the end range of the corresponding `FStringEnd` token. This could be done by either using a stack or using the `Indexer` to get the f-string range.

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:45_

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:46_

---

_Label `rule` removed by @dhruvmanila on 2023-09-12 09:47_

---

_Label `noqa` added by @dhruvmanila on 2023-09-12 09:47_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-13 03:47_

---

_Comment by @dhruvmanila on 2023-09-18 06:49_

***Continuing the discussion started in https://github.com/astral-sh/ruff/pull/7326#discussion_r1326848296***
> 
> I think there are multiple questions here that we haven't talked about
> 
> 1. How should noqa comments in fstrings work in Python 3.12 forward, now that comments are allowed inside f-strings
> 2. Whether we want to implement the new layout now or defer to later
> 3. How to implement the change

## NoQA for f-strings:

There are two options:
* For multi-line strings, NoQA comments can only be added at the end of the last line:
	```python
	"""this is a normal multi-line string
	where NoQA comments can only be added at the
	end of the last line that is here --> """  # noqa: E501
* For multi-line f-strings, we've got two options:
	1. Always at the end of the last line (current version)
    2. Either (i) or on the expression line where the violation needs to be suppressed. Here, we'll have to check the Python version as comments inside f-strings is only valid for 3.12+
	
For (ii), things gets a bit tricky. Take for example the following code snippet:

```python
f"""thiiiiiiiiiiiiiiisssssssssss iiiiiiiiiisssssssssss aaaaaaaaaaaaa verrrrrrrrrrrrrryyyyyyyyyyyy
{
    x
        *
            y
} looooooonnnnnggg string"""
```

Here, the rule `E501` (line too long) can only be disabled at the last line of the multi-line string but the rule `F821` (undefined name) can be disabled on the expression line (`x` and `y`). Taking the above example by adding the NoQA comments:

```python
f"""thiiiiiiiiiiiiiiisssssssssss iiiiiiiiiisssssssssss aaaaaaaaaaaaa verrrrrrrrrrrrrryyyyyyyyyyyy
{
    x  # noqa: F821
        *
            y  # noqa: F821
} looooooonnnnnggg string"""  # noqa: E501
```

The `E501` is added on the last line of the f-string because if it was added on the first line, it would become part of the string itself. But then what if we moved the opening curly brace on the first line:

```python
f"""thiiiiiiiiiiiiiiisssssssssss iiiiiiiiiisssssssssss aaaaaaaaaaaaa verrrrrrrrrrrrrryyyyyyyyyyyy {
    x
        *
            y
} looooooonnnnnggg string"""
```

Now, we can add the `E501` on the first line because that's the line which is longer than the line-length limit. Should it be added on the first line or the last line?

```python
f"""thiiiiiiiiiiiiiiisssssssssss iiiiiiiiiisssssssssss aaaaaaaaaaaaa verrrrrrrrrrrrrryyyyyyyyyyyy {  # noqa: E501
    x  # noqa: F821
        *
            y  # noqa: F821
} looooooonnnnnggg string"""
```

For single-quoted f-strings this problem doesn't come as the expression should start on the same line as the opening quote i.e., the opening curly brace should be on the same line as the opening quote:

```python
# valid
f"first line {
	x * y
}"

# invalid
f"first line
{ 
	x * y
}"
```

### Implementation

Here, it get's a bit tricky. To give some context, we build the `NoqaMapping` that maps the logical line to NoQA line i.e., the line where the violation is to the line where the NoQA comments should be added/detected.

For f-strings, we could use the `FStringMiddle` token. If the token is the only one on the line, then the NoQA comment should be at the end of the last line of the f-string. But, if there are other tokens on the same line (`f"foo {"` where there's `FStringMiddle` then `Rbrace`) then we can add the `NoQA` directive on the same line. But, this requires peeking to the next token and checking if they're on the same line or checking for any newlines in the `FStringMiddle` value.

For example,
```python
# 1
f"""first line
{
	expr
}"""

# 2
f"""first line
and another {
	expr
}"""

# 3
f"""first line {
	expr
}"""
```

Here, the content of `FStringMiddle` in (1) would be `"first line\n"` while in (2) would be `"first line\nand another "` and for (3) `"first line "`.

When I was thinking about the possible implementation, I found another problem w.r.t. (2) in the above code snippet. If the first line is too long, the NoQA directive has to go after the end of f-string but if the second line is too long, it could be added to the same line or at the end of the f-string. This is a confusing behavior:

```python
f"""assume this is a long line
and this is a long line too with an opening curly brace {  # noqa: E501  # should this even be here?
	expr
}"""  # noqa: E501  # for the first line
```

Then, if there's no NoQA comment on the second line but it is present at the end of the f-string, should the second line trigger `E501`?

```python
f"""assume this is a long line
and this is a long line too with an opening curly brace {  # should this trigger E501?
	expr
}"""  # noqa: E501  # for the first line
```

---

\cc @charliermarsh @MichaReiser I would love your thoughts on this but I believe we should be consistent in the NoQA handling and should add it at the end of the f-string

---

_Comment by @MichaReiser on 2023-09-18 16:17_

I'm okay moving forward with our current design but I see inline-noqa comments as a potential improvement because it avoids suppressing a too large range.

---

_Comment by @charliermarsh on 2023-09-18 16:23_

I feel similarly. For now, I'm fine with only respecting `# noqa` at the end of the multiline string, even for f-strings.

In the future, I suppose both of these should work:

```python
f"""thiiiiiiiiiiiiiiisssssssssss iiiiiiiiiisssssssssss aaaaaaaaaaaaa verrrrrrrrrrrrrryyyyyyyyyyyy {  # noqa: E501
    x
        *
            y
} looooooonnnnnggg string"""  # noqa: E501
```

We have precedence for this with, e.g.:

```python
from foo import (  # noqa: F401
  Bar,  # noqa: F401
)
```

In which both of those `noqa` positions are valid.


---

_Comment by @dhruvmanila on 2023-09-21 02:30_

Thanks for the feedback! I've opened #7561 to track that. I'll close this now.

---

_Closed by @dhruvmanila on 2023-09-21 02:30_

---
