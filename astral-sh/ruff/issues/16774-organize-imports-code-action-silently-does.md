```yaml
number: 16774
title: "Organize Imports code action silently does nothing when `# noqa` is present"
type: issue
state: closed
author: xhrnca00
labels:
  - server
assignees: []
created_at: 2025-03-16T12:35:39Z
updated_at: 2025-03-18T07:13:58Z
url: https://github.com/astral-sh/ruff/issues/16774
synced_at: 2026-01-10T11:09:57Z
```

# Organize Imports code action silently does nothing when `# noqa` is present

---

_Issue opened by @xhrnca00 on 2025-03-16 12:35_

### Summary

As mentioned in other issues, (such as [#9554](https://github.com/astral-sh/ruff/issues/9554)), adding `# noqa: I001` to the *first* line disables import sorting for *all* imports.

I personally think that is very unintuitive on its own, but I understand why it's done this way.

What I find even weirder is that the "Organize Imports" code action, present in Ruff LSP integrations in VSCode, Neovim and others, also does not format the imports when executed.

When I manually execute something, shouldn't that override any "static" rules? Since organizing imports is more of a formatting action, the user should *at least* get a warning that nothing actually happened, or the action should not be present at all if that would be the case.

I would prefer it to ignore all restrictions/disablements, but I assume it just executes the `ruff check --select I <file> --fix` command, so that might be difficult to achieve.

## To "reproduce":

1. copy the following to a file:
```python
# example.py
import os  # noqa: I001
from json import loads, dumps

if __name__ == "__main__":
    print(dumps(loads("file.json")))
```
2. Open in any editor with Ruff LSP integration.
3. Invoke "Organize imports" code action.
4. Nothing happens.

### Version

ruff 0.11.0 (2cd25ef64 2025-03-14) (and prior)

---

_Label `server` added by @dylwil3 on 2025-03-17 10:59_

---

_Comment by @MichaReiser on 2025-03-18 07:13_

> As mentioned in other issues, (such as https://github.com/astral-sh/ruff/issues/9554), adding # noqa: I001 to the first line disables import sorting for all imports.

I see how this is confusing. We should revisit how `noqa` suppressions work for isort but that's a breaking change. Until then, I recommend you to use the [`isort` specific suppression comments](https://docs.astral.sh/ruff/linter/#action-comments) to disable sorting of a specific import.

> When I manually execute something, shouldn't that override any "static" rules? Since organizing imports is more of a formatting action, the user should at least get a warning that nothing actually happened, or the action should not be present at all if that would be the case.

I can see the reasoning for this but the CLI and editor must behave exactly the same:

* Many users enable *organize imports* on save and Ruff shouldn't make any changes to a file that passed all Ruff tests on the CLI if it remained unchanged. However, this wouldn't be the case when implementing this change because the editor would organize the imports that were *fine* when using the CLI because the CLI respects the noqa comment but the editor doesn't
* I would find it very confusing if the editor ignored the noqa comment. I'm explicitly telling Ruff to *leave that line alone* and, IMO, that has higher priority than me running the *organize imports* command manually






---

_Closed by @MichaReiser on 2025-03-18 07:13_

---
