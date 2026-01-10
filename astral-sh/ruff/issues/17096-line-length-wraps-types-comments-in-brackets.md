---
number: 17096
title: line length wraps types + comments in brackets
type: issue
state: closed
author: hyperknot
labels:
  - needs-info
assignees: []
created_at: 2025-03-31T16:10:07Z
updated_at: 2025-03-31T16:42:53Z
url: https://github.com/astral-sh/ruff/issues/17096
synced_at: 2026-01-10T01:22:58Z
---

# line length wraps types + comments in brackets

---

_Issue opened by @hyperknot on 2025-03-31 16:10_

### Summary

A very interesting thing is going on with types + comments.

If I put line length to 88, it keeps it on a single line.
If I put it to 100, it wraps it within brackets and makes it 3 lines.

My editor is giving a warning that "value must be a type".

Playground:
https://play.ruff.rs/dc35fb3f-3836-41c0-a1fb-3c85e4323926

### Version

ruff 0.11.2 (4773878ee 2025-03-21)

---

_Label `needs-info` added by @MichaReiser on 2025-03-31 16:12_

---

_Comment by @MichaReiser on 2025-03-31 16:12_

Hi @hyperknot 

I'm sorry but I don't understand what isn't working. Is it the formatted output that wraps after 88 characters? What output would you expect?

---

_Comment by @hyperknot on 2025-03-31 16:15_

On the default 88, it does NOT wrap.
On 100 it does wrap.

One of them is wrong.

Is this correct?
```
class ThreadDetailsDict(TypedDict):
    last_message_sent_by_me: (
        bool  # True if last msg sender domain is in my_domains or sender email is in my_emails
    )
```

Based on my editor this is wrong, but my editor might be wrong here.


---

_Comment by @hyperknot on 2025-03-31 16:18_

So the question is: 
1. Can a type + comment be wrapped in a bracket? My editor thinks not.
2. Fix either 88's or 100's behaviour. The line is 117 charaters.

---

_Comment by @MichaReiser on 2025-03-31 16:23_

> On the default 88, it does NOT wrap.
On 100 it does wrap.

This is expected, and I agree it can be somewhat confusing. The formatter only wraps the line if it fits into the configured line length when put in parentheses. It avoids wrapping if the line, even if wrapped, still exceeds the configured line length.


Yes, Python allows wrapping type annotations. This runs fine:

```py
>>> from typing import TypedDict
>>> class ThreadDetailsDict(TypedDict):
...     last_message_sent_by_me: (
...         bool  # True if last msg sender domain is in my_domains or sender email is in my_emails
...     )
...
>>>
```

---

_Comment by @hyperknot on 2025-03-31 16:35_

OK, so both are correct and PyCharms is wrong.

---

_Closed by @hyperknot on 2025-03-31 16:35_

---

_Comment by @hyperknot on 2025-03-31 16:35_

Thanks for the quick help!

---

_Comment by @MichaReiser on 2025-03-31 16:42_

You're welcome. I'm surprised that Pycharm flags this. I'm not sure why. Even mypy thinks this is fine https://mypy-play.net/?mypy=1.8.0&python=3.13&gist=aee4eca148a276577c2567b8fac9f691

It seems to be specific to `TypedDict`.

---
