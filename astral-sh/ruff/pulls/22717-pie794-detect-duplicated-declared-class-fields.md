```yaml
number: 22717
title: "`PIE794`: Detect duplicated declared class fields"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: micha/fix-PIE794-annotated-assignments
created_at: 2026-01-19T11:12:03Z
updated_at: 2026-01-19T14:36:00Z
url: https://github.com/astral-sh/ruff/pull/22717
synced_at: 2026-01-19T15:25:06Z
```

# `PIE794`: Detect duplicated declared class fields

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/22683

The rule skipped annotated assignments without a value. This seems an unintentional change that was introduced in https://github.com/astral-sh/ruff/pull/8634. This PR re-enables detecting duplicated class fields where a later declaration is an annotated assignment without a value.

We might have to preview gate this change if there are many ecosystem hits. Although I doubt that, given that this is unlikely very common.

## Test Plan

Added regression test. 




---

_Label `bug` added by @MichaReiser on 2026-01-19 11:12_

---

_Label `rule` added by @MichaReiser on 2026-01-19 11:12_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 11:22_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/components/recharts/cartesian.py#L245'>reflex/components/recharts/cartesian.py:245:5:</a> PIE794 Class field `fill` is defined multiple times
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/components/recharts/cartesian.py#L248'>reflex/components/recharts/cartesian.py:248:5:</a> PIE794 Class field `stroke` is defined multiple times
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE794 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/components/recharts/cartesian.py#L245'>reflex/components/recharts/cartesian.py:245:5:</a> PIE794 Class field `fill` is defined multiple times
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/components/recharts/cartesian.py#L248'>reflex/components/recharts/cartesian.py:248:5:</a> PIE794 Class field `stroke` is defined multiple times
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE794 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @MichaReiser on 2026-01-19 11:25_

@AlexWaygood I always get confused about the trillion different ways on how you can define field and class level attributes in Python. Does the usage in the `reflex` project look legit to you (does it define a class and field level `fill` attribute?)

---

_Comment by @AlexWaygood on 2026-01-19 11:33_

The reflex hits look like true positives to me! They're basically doing this:

```py
class Foo:
    x: int = 0
    # 10 lines later...
    x: int
```

and there's no reason to do that instead of just:

```py
class Foo:
    x: int = 0
```

We might want to be careful about redeclarations to a _different_ type, though? ty supports these, although no other type checker does (at least, not by default):

```py
class Foo:
    x: int = 0
    x: str = "foo"
```

---

_Comment by @MichaReiser on 2026-01-19 11:35_

> We might want to be careful about redeclarations to a different type, though? ty supports these, although no other type checker does (at least, not by default):


I thought about that too but this seems like a separate change to the rule

---

_@ntBre approved on 2026-01-19 14:26_

---

_Merged by @MichaReiser on 2026-01-19 14:35_

---

_Closed by @MichaReiser on 2026-01-19 14:35_

---

_Branch deleted on 2026-01-19 14:36_

---
