```yaml
number: 22764
title: More opinionated multiline string formatting
type: issue
state: open
author: RazerM
labels:
  - formatter
  - style
assignees: []
created_at: 2026-01-20T13:57:30Z
updated_at: 2026-01-20T15:07:53Z
url: https://github.com/astral-sh/ruff/issues/22764
synced_at: 2026-01-20T15:43:52Z
```

# More opinionated multiline string formatting

---

_@RazerM_

> Now that black has stabilized multiline_string_handling I'd like to see it in ruff, it's the one thing keeping me from adopting ruff format in some projects.
> 
> To summarise:
> 
> ruff allows this:
> 
> ```python
> op.execute("""
>     CREATE TRIGGER ...
>     ...
> """)
> ```
> 
> but changes:
> 
> ```python
> op.execute(dedent("""
>     CREATE TRIGGER ...
>     ...
> """).strip())
> ```
> 
> to
> 
> ```python
> op.execute(
>     dedent("""
>     CREATE TRIGGER ...
>     ...
>     """).strip()
> )
> ```
> 
> which I can't get behind 

 _Originally posted by @RazerM in [#20482](https://github.com/astral-sh/ruff/issues/20482#issuecomment-3772967812)_

---

_Label `formatter` added by @MichaReiser on 2026-01-20 14:03_

---

_Label `style` added by @MichaReiser on 2026-01-20 14:03_

---

_Comment by @MichaReiser on 2026-01-20 14:04_

Issue where we discussed this preview style before https://github.com/astral-sh/ruff/issues/8896 and the implementation of the current style https://github.com/astral-sh/ruff/pull/9243 and later https://github.com/astral-sh/ruff/pull/9637

---

_Comment by @MichaReiser on 2026-01-20 15:06_

Ruff already implements `multiline_string_handling` but it isn't as opinionated about it as black. The main reason for it was to reduce the diff size between the old and the new formatter style. See https://github.com/astral-sh/ruff/pull/9637

I'm also not sure that I like Black's formatting better:

```py
op.execute(dedent("""
    CREATE TRIGGER ...
    ...
""").strip())

```

gets formatted to (tested on their playground)

```py
op.execute(
    dedent(
        """
    CREATE TRIGGER ...
    ...
"""
    ).strip()
)
```

And you can make Ruff do the same formatting by manually inserting a line break after the `dedent`


```py
op.execute(dedent(
    """
    CREATE TRIGGER ...
    ...
""").strip())
```

becomes 

```py
op.execute(
    dedent(
        """
    CREATE TRIGGER ...
    ...
"""
    ).strip()
)
```

I don't see a strong reason here to change anything in Ruff as it gives you the option to have either formatting.

---

_Renamed from "multiline_string_handling in ruff format" to "More opinionated multiline string formatting" by @MichaReiser on 2026-01-20 15:07_

---
