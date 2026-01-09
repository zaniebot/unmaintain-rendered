---
number: 18248
title: Feedback on E116 (preview rule)
type: issue
state: closed
author: dimaqq
labels:
  - question
assignees: []
created_at: 2025-05-22T06:13:24Z
updated_at: 2025-07-24T12:05:01Z
url: https://github.com/astral-sh/ruff/issues/18248
synced_at: 2026-01-07T13:12:16-06:00
---

# Feedback on E116 (preview rule)

---

_Issue opened by @dimaqq on 2025-05-22 06:13_

https://docs.astral.sh/ruff/rules/unexpected-indentation-comment/

On one hand, I do understand that random comment indentation is a bit ugly.

On the other, Python interpreter is fine with it, and it can be handy during development, for example:

```py
            except (NoObserverError, ActionFailed):
                raise  # propagate along
            except Exception as e:
                raise UncaughtCharmError(
                    f'Uncaught exception ({type(e)}) in operator/charm code: {e!r}',
                ) from e
```

⬇ 

```py
            except (NoObserverError, ActionFailed):
                raise  # propagate along
            # FIXME uncommment back; used for debug
            # except Exception as e:
                # raise UncaughtCharmError(
                    # f'Uncaught exception ({type(e)}) in operator/charm code: {e!r}',
                # ) from e
```

Arguably, I should have used some IDE macro to make a block comment instead, but I'm lazy and prepending `# ` and then repeating the command on the next few lines is just easier?

---

_Renamed from "Feedback on E116 preview" to "Feedback on E116 (preview rule)" by @dimaqq on 2025-05-22 06:13_

---

_Label `question` added by @MichaReiser on 2025-05-22 07:25_

---

_Comment by @MichaReiser on 2025-05-22 07:26_

Thanks for your feedback. 

E116 is an opinionated rule for users who want to enforce the comment indentation. It's totally fine if you decide to indent your comments differently. In this case, disable the rule. 

---

_Closed by @MichaReiser on 2025-07-24 12:05_

---
