```yaml
number: 18884
title: "Clarify documentation for `lint.extend-ignore` and `lint.extend-select` settings"
type: issue
state: open
author: Tedpac
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-06-23T06:34:48Z
updated_at: 2025-07-14T07:55:28Z
url: https://github.com/astral-sh/ruff/issues/18884
synced_at: 2026-01-10T11:09:58Z
```

# Clarify documentation for `lint.extend-ignore` and `lint.extend-select` settings

---

_Issue opened by @Tedpac on 2025-06-23 06:34_

These settings and their usefulness can be (and have been, from what I've seen in multiple issues) easily misunderstood.

For example, the following configurations are equivalent:

```toml
[tool.ruff.lint]
# Configuration 1.
select = ["ALL"] 
extend-select = ["D213"]
extend-ignore = ["D212"]

# Configuration 2.
select = ["ALL", "D213"] 
ignore = ["D212"]

[tool.ruff.lint.pydocstyle]
convention = "google"
```

On the other hand, it seems that `lint.extend-ignore` [was mistakenly deprecated](https://github.com/astral-sh/ruff/pull/12007#issuecomment-2186270791), and it is confusing to see that `lint.extend-ignore` is deprecated while `lint.extend-select` is not.

The documentation should include clear use cases for when these settings are useful. From what I understand, these settings are primarily valuable when:
- Using the top-level `extend` setting to inherit from base configurations.
- Adding rules on top of the default rule set without completely overriding it.

---

_Comment by @MichaReiser on 2025-06-23 06:44_

I agree that we could do better on the `select` documentation. Specifically, it doesn't mention that it overrides the existing defaults (or parent `select`) nor does it hint users towards using `extend-select`. 

> On the other hand, it seems that lint.extend-ignore https://github.com/astral-sh/ruff/pull/12007#issuecomment-2186270791, and it is confusing to see that lint.extend-ignore is deprecated while lint.extend-select is not.

This is covered by https://github.com/astral-sh/ruff/issues/12014

---

_Label `documentation` added by @MichaReiser on 2025-06-23 06:44_

---

_Label `help wanted` added by @MichaReiser on 2025-06-23 06:44_

---

_Comment by @pakal on 2025-07-09 15:15_

I'm confused too by the behaviour of extend-select, with regards to the "extend" inheritance in particular (not just as a complement to default settings).

If I have a root ruff.toml config stating : 

```
[lint]
extend-select = ["F821"]
```

and a nested config stating: 

```
extend = "../ruff.toml"

[lint]
select = ["T201"]
```

then I would believe the final config to merge these settings and have BOTH rules F821 and T201 enabled ; 
but only T201 gets applied, the "extend-select" of upper level is not inherited.

Is it supposed to behave like that ?

---

_Comment by @MichaReiser on 2025-07-10 08:56_

> Is it supposed to behave like that ?

Yes, that's the intended behavior. `select` disregards any parent configuration. This can be useful when you want to override the default rules enabled by ruff (or want to diverge from the parent configuration in more significant ways). 

It's only `extend-select` that merges the selection with what the parent configuration selected.

---

_Comment by @VelizarVESSELINOV on 2025-07-11 18:17_

```
[tool.ruff.lint]
select = ["ALL"]
extend-select = ["UP"]
```

ALL is not ALL, there is an extension of ALL?



---
