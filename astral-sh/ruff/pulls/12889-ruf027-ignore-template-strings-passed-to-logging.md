```yaml
number: 12889
title: "RUF027: Ignore template strings passed to logging calls and `builtins._()` calls"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: ruf027-gettext-logging
created_at: 2024-08-14T09:53:46Z
updated_at: 2024-08-14T10:27:37Z
url: https://github.com/astral-sh/ruff/pull/12889
synced_at: 2026-01-12T15:55:42Z
```

# RUF027: Ignore template strings passed to logging calls and `builtins._()` calls

---

_@AlexWaygood_

## Summary

#12869 initially proposed stabilising RUF027, but the ecosystem report showed that there were too many false positives. Some of those false positives stem from the fact that we currently emit errors on calls like this:

```py
import logging

foo = 42
logging.error("{foo}")
```

But you're not meant to pass f-strings to logging calls, as the `logging` module lazily evaluates template strings like this to improve performance. The default style of string interpolation used by the `logging` module is `%`-style interpolation, but this can be configured to use `{}`-style instead: https://docs.python.org/3/howto/logging-cookbook.html#formatting-styles. As such, this PR adjusts the rule's logic to ignore any strings that are inside logging calls.

I'm also making another small tweak to the logic used to detect `gettext` calls. Specifically, we weren't recognising a fully qualified call to `builtins._()` as a `gettext` call. But running `gettext.install()` literally monkeypatches the `builtins` module, so even this fully qualified access should be recognised by us as a `gettext` call.

## Test Plan

I added some new fixtures that Ruff will emit errors on currently, but doesn't with this PR branch.


---

_Label `rule` added by @AlexWaygood on 2024-08-14 09:53_

---

_Label `preview` added by @AlexWaygood on 2024-08-14 09:55_

---

_Comment by @github-actions[bot] on 2024-08-14 10:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-08-14 10:11_

> ### Linter (preview)
> 
> ✅ ecosystem check detected no linter changes.

I suspected that this might be the case. The false positives highlighted in the ecosystem check in https://github.com/astral-sh/ruff/pull/12869 looked like this, which we'll still currently emit a false positive error on, even with this PR (because the string is assigned to a variable _before_ being passed to a `logging` call):

```py
import logging
foo = 42
logging_string = "{foo}"
logging.error(logging_string)
```

I have ideas for how to get rid of this false positive, but it's more involved, so I'd prefer to do it as a followup to this PR.

---

_@MichaReiser approved on 2024-08-14 10:25_

---

_Comment by @MichaReiser on 2024-08-14 10:26_

Nice! And thanks for the very detailed summary. It made reviewing a piece of cake 

---

_Merged by @AlexWaygood on 2024-08-14 10:27_

---

_Closed by @AlexWaygood on 2024-08-14 10:27_

---

_Branch deleted on 2024-08-14 10:27_

---
