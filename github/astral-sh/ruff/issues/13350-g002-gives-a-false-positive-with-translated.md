---
number: 13350
title: G002 gives a false positive with translated strings
type: issue
state: closed
author: jpmckinney
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-09-14T01:57:50Z
updated_at: 2024-09-16T19:35:12Z
url: https://github.com/astral-sh/ruff/issues/13350
synced_at: 2026-01-07T13:12:15-06:00
---

# G002 gives a false positive with translated strings

---

_Issue opened by @jpmckinney on 2024-09-14 01:57_

For example, when using gettext, it's common to write code like:

```python
_("Loading module: %(module)s") % {"module": mod}
```

or:

```python
_("Loading module: %s") % mod
```

If these messages are logged (for example, in a multilingual command line tool), like:

```python
logger.info(_("Loading module: %s") % mod)
```

Then, G002 is raised, although the `%` operation is on the return value of a function (which is not how people typically misuse the logging module).

---

_Label `type-inference` added by @MichaReiser on 2024-09-16 09:02_

---

_Label `type-inference` removed by @MichaReiser on 2024-09-16 09:08_

---

_Comment by @MichaReiser on 2024-09-16 09:15_

I feel hesitant to allow the format operator when the left side is a call expression. For example:

```python
```
>>> def test(): return "abcd %s"
... 
>>> logger.info(test() % "1")
```

This could be rewritten to 

```python
def logTet(arg):
	logger.info("test %", arg)

log(1)
```

I also think flagging the `gettext` call is somewhat in the spirit of the rule. Ideally, `gettext` only gets called if the `logger`'s level is higher than the called log function to avoid an unnecessary gettext calls and string interpolations. 

I would probably explore adding custom `infoTranslated` helper methods that conditionally call `gettext`. 

 

---

_Label `rule` added by @MichaReiser on 2024-09-16 09:15_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-16 09:15_

---

_Comment by @jpmckinney on 2024-09-16 19:35_

Ahh, good call. I'll close – but if some future user does want something similar, it can of course be reopened.

---

_Closed by @jpmckinney on 2024-09-16 19:35_

---
