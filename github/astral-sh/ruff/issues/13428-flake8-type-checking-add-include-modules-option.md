---
number: 13428
title: "[flake8-type-checking] Add include-modules option (Feature Request)"
type: issue
state: open
author: gregbrowndev
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-09-20T19:38:33Z
updated_at: 2024-09-23T09:08:00Z
url: https://github.com/astral-sh/ruff/issues/13428
synced_at: 2026-01-07T13:12:15-06:00
---

# [flake8-type-checking] Add include-modules option (Feature Request)

---

_Issue opened by @gregbrowndev on 2024-09-20 19:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi, thank you for the work on ruff and lint/auto-fix rules!

I've added the flake8-type-checking (TCH) rules to my codebase to automatically move type-only imports to an `if TYPE_CHECKING` block. Super helpful to build lean, production images without superfluous type-shed libraries.

Ruff allows me to disable the `TCH001` and `TCH003` rules to ignore first-party and standard library type-only imports. However, the `TCH002` rule treats all third-party typing-only imports the same. Since some third-party libraries ship with type annotations, these are available in the production build. These types could be imported directly and avoid the additional stringified, forward references throughout the code. 

I feel this would be a smaller migration step for teams to introduce, easing the experience for people less familiar with type hints (since IMO the quotes do add some additional visual clutter).

I've used the [exempt-modules](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_exempt-modules) option to exclude common third-party libraries so they can be imported directly, but it would be simpler to specify specifically which third-party libraries to fix, i.e., the type shed ones. 

Thanks a lot

---

_Comment by @MichaReiser on 2024-09-23 09:07_

Thanks for the kind words.

I'm not sure if I fully understand what you're asking for. 

> avoid the additional stringified, forward references throughout the code.

From reading [`TCH002`'s documentation](https://docs.astral.sh/ruff/rules/typing-only-third-party-import/), I understand that the rule doesn't stringify type annotations. It only does so for a few or when `[lint.flake8-type-checking.quote-annotations](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_quote-annotations)` is enabled. Could you share an example with your configuration where the type annotations get stringified?

> I've used the [exempt-modules](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_exempt-modules) option to exclude common third-party libraries so they can be imported directly, but it would be simpler to specify specifically which third-party libraries to fix, i.e., the type shed ones.

I prefer `excempt-modules`. Yes, it's more annoying to set up initially because it requires listing all modules, but it prevents a new module from accidentally "slipping" through. Maybe you don't want to exclude that new module because you decide to use deferred type annotations for all imports from that new module, there's no existing code after all. 

The other reason I prefer `excempt-modules` is because the main motivation of `TCH002` is to reduce the runtime overhead of importing typing only modules at runtime. Only including few modules defeats the purpose of the rule, IMO. 

---

_Label `rule` added by @MichaReiser on 2024-09-23 09:08_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-23 09:08_

---
