---
number: 21613
title: Show diff for applied fixes
type: issue
state: closed
author: travisdowns
labels:
  - question
assignees: []
created_at: 2025-11-24T13:11:11Z
updated_at: 2025-11-24T15:50:46Z
url: https://github.com/astral-sh/ruff/issues/21613
synced_at: 2026-01-07T13:12:16-06:00
---

# Show diff for applied fixes

---

_Issue opened by @travisdowns on 2025-11-24 13:11_

### Question

There are a lot of questions around --fix and --diff and so on but I didn't find one that matched this condition.

We are trying to run `ruff check` in CI, and also in a git pre-commit hook which checks each commit when you do `git commit`.

The best behavior for the pre-commit hook, I think, is:

1) Fixes are applied if possible
2) Diffs are shown for _all applied fixes_ and _any remaining unfixable issues_
3) Return non-zero if there any remaining issues, and the value in the case everything was fixed isn't too important for our use-case (as the pre-commit hook knows to fail if there are modified files regardless of the exit code)

As far as I can tell, we can't get this behavior curently? `--fix` doens't show diffs for fixed issues. `--fix --diff` doesn't apply any fixes at all.

In the local git hook you want the behavior above since `--fix` lets you immediately `add -u` to stage the fixes, but you also want to see the diff so you know what happened and what the diagnostic was.

When running the same code in CI, you really want the console output off the diff & reason since --fix does not provide a benefit there.


### Version

ruff 0.12.10

---

_Label `question` added by @travisdowns on 2025-11-24 13:11_

---

_Comment by @MichaReiser on 2025-11-24 14:20_

Is it necessary that the shown fixes are diffs?

If not, we changed the `--output-format=full` (the default) to show fixes when using `--preview`. I think running `ruff check --fix --preview` should then be what you're looking for (be mindful, `--preview` also enables the preview rule behavior. There's currently no way of opting in to the changed CLI behavior only)

---

_Comment by @travisdowns on 2025-11-24 14:59_

> Is it necessary that the shown fixes are diffs?

Not really, just some format that shows the error. I should not have said diff really: I guess the ideal format is actually the usual "error is here" format you get when you don't use `--diff`.

> If not, we changed the --output-format=full (the default) to show fixes when using --preview.

Oh, that's great. So if it's `--preview` today, I can anticipate that it may show up as the default in the future (if there isn't a bunch of backlash in the meantime)?

---

_Comment by @MichaReiser on 2025-11-24 15:20_

> Oh, that's great. So if it's --preview today, I can anticipate that it may show up as the default in the future (if there isn't a bunch of backlash in the meantime)?

Yes, we plan on stabilizing this feature as part of our next minor, which we aim to release early January

---

_Comment by @travisdowns on 2025-11-24 15:50_

Thanks! Closing.

---

_Closed by @travisdowns on 2025-11-24 15:50_

---
