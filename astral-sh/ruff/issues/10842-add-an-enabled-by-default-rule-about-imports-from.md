```yaml
number: 10842
title: Add an enabled-by-default rule about imports from an src package
type: issue
state: open
author: pradyunsg
labels:
  - rule
assignees: []
created_at: 2024-04-09T01:45:49Z
updated_at: 2024-04-10T09:43:47Z
url: https://github.com/astral-sh/ruff/issues/10842
synced_at: 2026-01-12T15:54:50Z
```

# Add an enabled-by-default rule about imports from an src package

---

_@pradyunsg_

This is meant to prevent misused of PEP 420 (namespace packages) with the src/ layout.

https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/ discusses the tradeoffs between the src layout vs flat layout. The src layout is preferable for the reasons outlines there, although it is also feasible to undermine the benefits of the layout by doing `from src import mypackage` and similar misuses.

It would be nice if ruff had an enabled-by-default rule for warning about misuse of an src namespace package.

---

_Comment by @MichaReiser on 2024-04-09 07:02_

> It would be nice if ruff had an enabled-by-default rule for warning about misuse of an src namespace package.

Could you explain what you consider a misuse of a src namespace package and how this would be detected by analysing a project?

---

_Label `rule` added by @MichaReiser on 2024-04-09 07:03_

---

_Comment by @pradyunsg on 2024-04-09 09:36_

Any of the following imports:

```
import src.whatever
from src import something
from src.something import something_else
```

(and their `as` variants)

This is effectively warning when an import starts from the top level namespace of src.

To avoid repeating too much content from the link, the main benefit of the src layout is that it forces you to install the project to be able to import any of the code within it. This helps ensure that the project configuration is correct, that the package is being installed appropriately and that you're only able to import what would be actually installed. Importing from src directly undermines the whole point of using an src layout.





---

_Comment by @carljm on 2024-04-09 14:19_

This rule would just make it an error to have a top-level package named `src` in your project, with the reasoning that the name `src` is most commonly used for the outer container of your source tree (to isolate your Python source tree from the rest of your project), but unfortunately Python will happily treat any directory as a package, and therefore allow you to `from src import whatever`, even though `src` wasn't supposed to itself be a Python package, `whatever` was supposed to be top-level.

It makes sense why this would be useful in many cases, though it feels a little icky to have an enabled-by-default rule that just says "you can't use this otherwise-valid name for a Python package."

---

_Comment by @pradyunsg on 2024-04-09 17:22_

TBH, I share some hesitation around this feeling a bit icky.

That said, I filed this issue since I think this is not substantially different in terms of "otherwise valid Python code flagged by the linter" as compared to other existing rules, like https://docs.astral.sh/ruff/rules/unused-import/ or https://docs.astral.sh/ruff/rules/import-shadowed-by-loop-var/ or https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/.

All of those are valid ways to use import or other Python syntax, and there might even be situations where one or more of those would be ignored manually because doing that is _exactly_ what the user wants (which is why ignores are nice).

It's almost never what you want to/should do though, and having this get flagged to users by default helps users discover their misuse (thanks to an error with a associated documentation that provides specific guidance, like those aforementioned ones) and helps guide them away from doing what they're doing (or telling the linter to shut up, which should be MUCH rarer, which will be the case for this specific rule).



---
