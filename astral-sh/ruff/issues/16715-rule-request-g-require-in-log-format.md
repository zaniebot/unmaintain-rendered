```yaml
number: 16715
title: "Rule request: G: Require {} in log format"
type: issue
state: open
author: colindean
labels:
  - rule
assignees: []
created_at: 2025-03-13T19:13:16Z
updated_at: 2025-09-16T15:54:51Z
url: https://github.com/astral-sh/ruff/issues/16715
synced_at: 2026-01-10T11:09:57Z
```

# Rule request: G: Require {} in log format

---

_Issue opened by @colindean on 2025-03-13 19:13_

### Summary

My team is migrating to [loguru](https://github.com/delgan/loguru) and trying to catch our well-trained reflexes to use curly brackets for placeholders:

```python
logger.info("Thing was {}", thing.category)
```

instead of the preferred `%` placeholder for stdlib logging

```python
logger.info("Thing was %s", thing.category)
```

I'd love a rule for log messages that either (or both):

1. could enforce use of `{}` (and allow `{\w*}` so `{thing}` is allowed)
2. prohibit use of `%` (e.g. `%s`, `%d`, `%f`, and all sub-subspecifiers thereof, e.g. `%0.3f`.)

In https://github.com/astral-sh/ruff/issues/13673, it's established that Loguru is not explicitly supported, as it has not met the popularity threshold for a community standard yet. That's understandable. However, I'm not necessarily asking for "Loguru support" here. The [Python Logging Cookbook section on alterative formatting styles](https://docs.python.org/3/howto/logging-cookbook.html#use-of-alternative-formatting-styles) suggests using `{}` if you want to use `str.format(values)` under the hood instead of `template % values` formatting.

In https://github.com/astral-sh/ruff/issues/13390, [PLE1205](https://docs.astral.sh/ruff/rules/logging-too-many-args/) is triggered when using `{}` in logging messages, but the advice is to disable that check. Perhaps the check that I'm suggesting here can conflict with that and require only one of them to be enabled.

---

_Comment by @MichaReiser on 2025-03-13 19:34_

Would the https://docs.astral.sh/ruff/rules/#flake8-logging-log rules work for you in combination with adding `loguru` to https://docs.astral.sh/ruff/settings/#lint_logger-objects

---

_Label `question` added by @MichaReiser on 2025-03-13 19:34_

---

_Comment by @colindean on 2025-03-13 20:25_

I don't think so, because neither flake8-logging-log (LOG) nor flake8-logging-format (G) have rules that enforce `{}` or prohibit `%` inside of the log message. Also, it seems like the logger from loguru— `from loguru import logger`— is recognized as a logger and all the rules I've expected trigger [^g004].

Something else that might work— and I'm not sure if this is possible— is a configurable placeholder for [PLE1205 logging-too-many-args](https://docs.astral.sh/ruff/rules/logging-too-many-args/) and [PLE1206 logging-too-few-args](https://docs.astral.sh/ruff/rules/logging-too-few-args/). Warning that the number of `{}` doesn't match the expected positional arguments would get us a lot of the way there, probably about 90% of our migrated log lines are simple `{}`. This wouldn't support the `{thing}` use case, though.

[^g004]: [G004 logging-f-string](https://docs.astral.sh/ruff/rules/logging-f-string/) in particular has been a lifesaver and that works great. A few codebases we've inherited are loaded with f-strings in log messages. Most are the best case scenario for misuse but others, well, not so much 🫠


---

_Comment by @MichaReiser on 2025-03-14 08:34_

I'm sorry. I should have read your initial issue more carefully. I now understand what you're asking for. To summarize what I understand:

* For the std logging: respect the `style` argument 
* For third-party loggers: Allow specifying the "fallback" style if `style` isn't explicitly provided. In the future, derive the default automatically based on known loggers.



---

_Label `question` removed by @MichaReiser on 2025-03-14 08:34_

---

_Label `rule` added by @MichaReiser on 2025-03-14 08:34_

---

_Comment by @colindean on 2025-03-17 23:31_

Yes! Those sound good to me. Thank you.

---

_Comment by @spaceone on 2025-09-16 15:54_

> The [Python Logging Cookbook section on alterative formatting styles](https://docs.python.org/3/howto/logging-cookbook.html#use-of-alternative-formatting-styles) suggests using `{}` if you want to use `str.format(values)` under the hood instead of `template % values` formatting.

You are misinterpreting the docs.
Python-logging supports giving `style="{"` to a `logging.Formatter`, which allows one to set the log format (not the log messages) e.g. via `logging.basicConfig(fmt='{asctime} {message}', style='{')`.

but the logger still only supports `log.error('percent %s', 'encoded values')`!

---
