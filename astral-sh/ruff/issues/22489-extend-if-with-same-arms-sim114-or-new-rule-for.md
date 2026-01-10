```yaml
number: 22489
title: Extend if-with-same-arms (SIM114) or new rule for seperate if statements that return
type: issue
state: open
author: GideonBear
labels: []
assignees: []
created_at: 2026-01-10T09:44:43Z
updated_at: 2026-01-10T09:44:43Z
url: https://github.com/astral-sh/ruff/issues/22489
synced_at: 2026-01-10T12:06:57Z
```

# Extend if-with-same-arms (SIM114) or new rule for seperate if statements that return

---

_Issue opened by @GideonBear on 2026-01-10 09:44_

### Summary

Ref #8125, originally raised as https://github.com/astral-sh/ruff/issues/8125#issuecomment-3707046945

[if-with-same-arms (SIM114)](https://docs.astral.sh/ruff/rules/if-with-same-arms/#if-with-same-arms-sim114)

Take the following example: [playground](https://play.ruff.rs/a1efcc7f-af3e-4d6b-a923-c958ea86bf1a)

[superfluous-else-return (RET505)](https://docs.astral.sh/ruff/rules/superfluous-else-return/#superfluous-else-return-ret505) indicates that writing this in the `if-if` style is preferred to writing it in the `if-elif` style. The functions have exactly the same behavior, and in both, SIM114 is valid. But by writing it in the preferred style following RET505, SIM114 doesn't trigger anymore. This means that people that write in the RET505 style by themselves, or fix RET505 before SIM114, don't get the benefit of SIM114.

I propose making SIM114 check, similarly to what RET505 checks in the first block, if the blocks return in all paths. If that is the case, pretend like it is `if-elif`, because the runtime behavior is identical.

This will make the rule more complex, as it will need to run on pairs/sets of if statements. This can be a new RUF rule as well, if you don't want to overload SIM114.

---
