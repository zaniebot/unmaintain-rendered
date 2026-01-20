```yaml
number: 22764
title: multiline_string_handling in ruff format
type: issue
state: open
author: RazerM
labels:
  - formatter
  - style
assignees: []
created_at: 2026-01-20T13:57:30Z
updated_at: 2026-01-20T14:06:21Z
url: https://github.com/astral-sh/ruff/issues/22764
synced_at: 2026-01-20T14:40:32Z
```

# multiline_string_handling in ruff format

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
