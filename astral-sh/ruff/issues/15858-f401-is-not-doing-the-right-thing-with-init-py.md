```yaml
number: 15858
title: "F401 is not doing the \"right\" thing with __init__.py"
type: issue
state: open
author: mlasevich
labels:
  - question
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-31T20:50:53Z
updated_at: 2025-05-29T17:43:43Z
url: https://github.com/astral-sh/ruff/issues/15858
synced_at: 2026-01-10T11:09:57Z
```

# F401 is not doing the "right" thing with __init__.py

---

_Issue opened by @mlasevich on 2025-01-31 20:50_

### Description

Ok, yet another question/request about how F401 treats `__init__.py` . I recognize "right" is a matter of an opinion, but tools should be configurable to match user's needs - and this is not the case here.

Generally speaking, in most libraries, `__init__.py` imports are used to expose internal module items to uses outside the library or package. This simplifies consumption of the library/package and allows to change implementation details without affecting the code depending on it. As such, of course most(if not all) of the imports in `__init__.py`  _look_ like they are unused. Pylint seems to have no issues with this. However ruff still gets confused. It wants you to either add things to `__all__` (which only affects wildcard imports, which we do not want to encourage or support, thus adding unused duplicate code just to make a linter happy, which is silly) or by doing redundant exports (import x as x), a step that is not only redundant (and silly), but also breaks some use cases where you legitimately want to change the name on presenting it to the package's clients.

So, while I recognize that not everyone will agree with me, I would like to have some means to configure this behavior so that i can use ruff without rewriting all my code to add redundant/unused stuff. There is a `ignore-init-module-imports` configuration setting which seemed promising - but it is labeled as deprecated, and, more importantly, just does not seem to do anything.

So, what am I asking for is one of:

1 - restore `ignore-init-module-imports` setting to that it works

or

2 - Just split F401 into two separate rules, one for `__init__.py` files and one for everything else. This will allow disabling one without disabling the other.


Currently the only way to make ruff work is to just disable the rule globally, which is less than ideal.

---

_Label `rule` added by @dylwil3 on 2025-02-01 00:01_

---

_Label `needs-decision` added by @dylwil3 on 2025-02-01 00:01_

---

_Comment by @tjkuson on 2025-02-01 13:14_

> Currently the only way to make ruff work is to just disable the rule globally, which is less than ideal.

Alternatively,

```toml
[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["F401"]
```

should disable the diagnostic in `__init__.py`. It's what I did when adding Ruff to a codebase which didn't follow a re-export convention.

---

_Comment by @MichaReiser on 2025-02-03 09:34_

Thanks for the write up. Our current recommendation is to do what @tjkuson suggested. 

---

_Label `question` added by @MichaReiser on 2025-02-03 09:34_

---

_Comment by @mlasevich on 2025-02-04 15:23_

A bit ugly, but good enough. Thank you :-)

---

_Comment by @stevenh on 2025-05-29 17:43_

I found this report, when investigating why ruff had stopped removing unused imports. At the time I was unaware of any special behaviour for `F401` for `__init__.py`. I went down the hole of `--unsafe-fixes` and `--verbose` in the hope of some info as to why it was reporting but not fixing.

I've now found the `--preview` does allow the fix, which is what I expected to be the default.

I understand the motivation when a module has been released but when it hasn't the default behaviour is frustrating.

I also found that setting [ignore-init-module-imports = false](https://docs.astral.sh/ruff/settings/#lint_ignore-init-module-imports), allowed `--unsafe-fixes` to remove it, however it's deprecated so can't rely on its behaviour long term.
```toml
[tool.ruff.lint]
ignore-init-module-imports = false
```

I think the important thing to address is the unexplained behaviour when the user believes they have been explicit, the special nature adds to the confusion. A simple log message to inform the user of the issue would really help in the case.

The current deprecation warning for `ignore-init-module-imports` is also confusing
> warning: The `ignore-init-module-imports` option is deprecated and will be removed in a future release. Ruff's handling of imports in `__init__.py` files has been improved (in preview) and unused imports will always be flagged.
Found 1 error (1 fixed, 0 remaining).

I don't have preview enabled but F401 is reporting all unused imports in `__init__.py`, so contrary to the above message you don't need preview enabled for it to be reported, you only need `--preview` for `--unsafe-fixes` to fix `F401` in `__init__.py`.

Hope this helps others.

---
