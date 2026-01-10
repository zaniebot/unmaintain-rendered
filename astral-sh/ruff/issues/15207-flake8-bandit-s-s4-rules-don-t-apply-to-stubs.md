```yaml
number: 15207
title: "`flake8-bandit`'s `S4` rules don't apply to stubs"
type: issue
state: closed
author: Avasam
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-12-30T22:09:57Z
updated_at: 2025-01-30T05:42:57Z
url: https://github.com/astral-sh/ruff/issues/15207
synced_at: 2026-01-10T11:09:56Z
```

# `flake8-bandit`'s `S4` rules don't apply to stubs

---

_Issue opened by @Avasam on 2024-12-30 22:09_

When enabling the `flake8-bandit (S)` category (with preview mode), `S4` rules will trigger on imports in stubs of a project. I believe these will *always* be false-positives and there's no need no need to warn about importing insecure and vulnerable libraries in stubs. Since those are not runtime files. Nothing gets executed.

Ruff: 0.8.4 (rules are currently in preview)

(this report is extracted from https://github.com/astral-sh/ruff/issues/14535#issuecomment-2561461038 for ease of tracking and discussion)

---

_Label `rule` added by @dhruvmanila on 2024-12-31 05:23_

---

_Comment by @dhruvmanila on 2024-12-31 05:34_

Thank you for the report!

Thinking from a user perspective, I think this _might_ still be useful when removing the import from both the Python and the respective stub files. Without this rule highlighting the fact that the import exists in the stub file as well, the user might forget to remove them. Your point is correct that this wouldn't affect anyone as stub files aren't executed but it might still be useful.

I'd prefer to get opinions from @AlexWaygood, he's currently on vacation this week so not expecting any reply this week.

---

_Comment by @AlexWaygood on 2025-01-03 14:59_

Heh, interesting question! I think I'm inclined to side with @Avasam on this one, because of the fact that stubs are so often written by people who aren't the authors of the corresponding runtime package. In that scenario, it's out of the control of the stub author whether or not the runtime package uses the insecure API; the stub author's responsibility is just to copy what happens at runtime.

> Without this rule highlighting the fact that the import exists in the stub file as well, the user might forget to remove them.

If you remove the problematic import from the file at runtime, it's true that you might forget to update the stub file at the same time. However, there are excellent tools such as [stubtest](https://mypy.readthedocs.io/en/stable/stubtest.html) that will error if there are significant discrepancies between the runtime module and the stub. There are also tools such as Ruff that will emit errors if you have unused imports in a stub file! So in practice, I don't think this is a major issue. Disabling this rule for stubs also feels like it follows the precedent we set when we disabled [`E741`](https://docs.astral.sh/ruff/rules/ambiguous-variable-name/) for stubs.

---

_Label `help wanted` added by @AlexWaygood on 2025-01-03 15:59_

---

_Label `accepted` added by @AlexWaygood on 2025-01-03 15:59_

---

_Closed by @dylwil3 on 2025-01-30 05:42_

---

_Closed by @dylwil3 on 2025-01-30 05:42_

---
