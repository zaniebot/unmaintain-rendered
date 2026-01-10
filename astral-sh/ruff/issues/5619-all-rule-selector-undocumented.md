---
number: 5619
title: "\"ALL\" rule selector undocumented"
type: issue
state: closed
author: benjamin-kirkbride
labels:
  - documentation
assignees: []
created_at: 2023-07-08T17:12:53Z
updated_at: 2024-02-26T08:50:09Z
url: https://github.com/astral-sh/ruff/issues/5619
synced_at: 2026-01-10T01:22:44Z
---

# "ALL" rule selector undocumented

---

_Issue opened by @benjamin-kirkbride on 2023-07-08 17:12_

I looked for a while to figure out how to do this, and eventually found it in https://github.com/astral-sh/ruff/discussions/4237

I think it should be documented:
- at the top of https://beta.ruff.rs/docs/rules/
-  in https://beta.ruff.rs/docs/settings/#select

---

_Comment by @dhruvmanila on 2023-07-09 16:57_

Hey, thanks for opening an issue. It's present in the documentation in this section: https://beta.ruff.rs/docs/configuration/#using-pyprojecttoml:

> As a special-case, Ruff also supports the `ALL` code, which enables all rules. Note that some pydocstyle rules conflict (e.g., `D203` and `D211`) as they represent alternative docstring formats. Ruff will automatically disable any conflicting rules when `ALL` is enabled.

Do you think it would be useful to add it elsewhere?

---

_Label `documentation` added by @dhruvmanila on 2023-07-09 16:57_

---

_Comment by @ssweber on 2023-07-09 17:55_

Is there somewhere that has all the rule categories in list format? I made one myself a few months back, and am slowly uncommenting rules as I have time to fix a codebase with them.

---

_Comment by @benjamin-kirkbride on 2023-07-09 19:36_

@dhruvmanila ah, sorry I missed that.

IMO, I think the whole
> As a special-case, Ruff also supports the ALL code, which enables all rules. Note that some pydocstyle rules conflict (e.g., D203 and D211) as they represent alternative docstring formats. Ruff will automatically disable any conflicting rules when ALL is enabled.
>
>If you're wondering how to configure Ruff, here are some recommended guidelines:
>
>Prefer select and ignore over extend-select and extend-ignore, to make your rule set explicit.
Use ALL with discretion. Enabling ALL will implicitly enable new rules whenever you upgrade.
Start with a small set of rules (select = ["E", "F"]) and add a category at-a-time. For example, you might consider expanding to select = ["E", "F", "B"] to enable the popular flake8-bugbear extension.
By default, Ruff's autofix is aggressive. If you find that it's too aggressive for your liking, consider turning off autofix for specific rules or categories (see [FAQ](https://beta.ruff.rs/docs/faq/#ruff-tried-to-fix-something--but-it-broke-my-code)).

section should be much higher up, possibly at the top or even in it's own section.

---

_Comment by @charliermarsh on 2023-07-09 20:00_

Hmm yeah, `ALL` is intentionally under-documented, I view it as a little bit of an anti-pattern and don't want to encourage its use. I'm hoping to replace it in the future with semantically meaningful categories and better defaults.

---

_Comment by @hmvp on 2023-07-26 09:40_

We use `ALL` in combination with a pinned version of ruff. This works great because on upgrade we can always ignore new rules if we don't want them (or don't want to fix them yet).


---

_Comment by @doolio on 2023-11-08 15:30_

> Hmm yeah, `ALL` is intentionally under-documented, I view it as a little bit of an anti-pattern and don't want to encourage its use. I'm hoping to replace it in the future with semantically meaningful categories and better defaults.

Counter argument. I'm a beginner programmer and using `ALL` as a teaching aid to write better code! Just the other day it taught me about the built-in function `all` via `SIM110`.

---

_Comment by @zanieb on 2023-11-08 15:41_

We'd like to resolve this by re-categorizing the rules and having sensible defaults as noted by Charlie — then Ruff will still teach you to write better code and you won't get a bunch of pedantic or rules without common consensus like you do when you turn on ALL. I worry that learning with ALL is too noisy of an experience.

---

_Comment by @sanxiyn on 2024-02-26 08:33_

Current status: `ALL` is documented at https://docs.astral.sh/ruff/linter/#rule-selection.

---

_Closed by @MichaReiser on 2024-02-26 08:50_

---
