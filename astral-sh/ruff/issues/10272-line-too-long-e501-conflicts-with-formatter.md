```yaml
number: 10272
title: "`line-too-long` (`E501`) - conflicts with formatter"
type: issue
state: open
author: DetachHead
labels:
  - incompatibility
assignees: []
created_at: 2024-03-07T09:33:27Z
updated_at: 2025-04-03T11:15:30Z
url: https://github.com/astral-sh/ruff/issues/10272
synced_at: 2026-01-12T15:54:50Z
```

# `line-too-long` (`E501`) - conflicts with formatter

---

_@DetachHead_

# before
```py
def foo():
    """aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa aaaaaaa
    """
```
# after
```py
def foo():
    """aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa aaaaaaa""" # E501: Line too long (91 > 88)
```
[playground](https://play.ruff.rs/78361064-f9ba-4914-ae6d-cc7c71a1c369)

related: #6269

---

_Comment by @MichaReiser on 2024-03-07 09:47_

Hy @DetachHead 

Thanks for reporting this. This is an interesting one. Putting the closing quotes on the same line as the opening quotes
is desired for consistency but it can have the undesired side effect that it exceeds the line length. 

There are other cases where the formatter collapses lines even if that means that it increases the line length. For example, the formatter removes the parentheses from:

```python
aaaaaaaaaaaaaaaaaa = (
    bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
)
```

It's less of a problem in this case because the input already exceeds the line length. However, this is different from what you report. I'm not sure what the desired behavior should be because it either leads to reduced consistency OR a violation of E501. From a pure formatting standpoint, I would prefer keeping it as is, but it would mean that E501 can be incompatible with the formatter, so it might be worth fixing.




---

_Label `formatter` added by @MichaReiser on 2024-03-07 09:48_

---

_Label `incompatibility` added by @MichaReiser on 2024-03-07 09:48_

---

_Label `formatter` removed by @MichaReiser on 2024-03-07 09:48_

---

_Comment by @vasily-v-ryabov on 2025-04-03 11:11_

In our project pure `ruff format --config .ruff.toml` produces 3k+ `line-too-long` warnings, but explicit use of `ruff format --config .ruff.toml --line-length 80` produces only 30+ warnings which are easily fixable by hands or ignored by `# fmt: off`. I'm not sure why `.ruff.toml` option `max-line-length = 80` is ignored by the Formatter, but at least we have a workaround which is acceptable for us.

---

_Comment by @MichaReiser on 2025-04-03 11:15_

The formatter uses the global `line-length` setting. The `max-line-length` setting is for when you want to use a different limit between E501 and the formatter

https://docs.astral.sh/ruff/settings/#line-length

---
