```yaml
number: 22764
title: Hug method calls on multiline strings
type: issue
state: open
author: RazerM
labels:
  - formatter
  - style
assignees: []
created_at: 2026-01-20T13:57:30Z
updated_at: 2026-01-21T08:16:27Z
url: https://github.com/astral-sh/ruff/issues/22764
synced_at: 2026-01-21T09:02:51Z
```

# Hug method calls on multiline strings

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

_Comment by @MeGaGiGaGon on 2026-01-20 18:57_

The black playground is out of date, so it still has 2025's style (we haven't managed to get in contact with the maintainer to update it v.v). The new 2026 style leaves the formatting as Razer observes:
```powershell
PS ~\Documents\python_projects\black>uvx black -c @'
op.execute(dedent("""
    CREATE TRIGGER ...
    ...
""").strip())
'@
op.execute(dedent("""
    CREATE TRIGGER ...
    ...
""").strip())
```

---

_Renamed from "More opinionated multiline string formatting" to "Hug method calls on multiline strings" by @MichaReiser on 2026-01-21 08:14_

---

_Comment by @MichaReiser on 2026-01-21 08:16_

Thanks @MeGaGiGaGon for the added context. 

This doesn't sound unreasonable and a small iteration on what Ruff already does to day

---
