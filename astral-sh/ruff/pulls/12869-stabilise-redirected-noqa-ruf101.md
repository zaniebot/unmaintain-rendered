```yaml
number: 12869
title: "Stabilise `redirected-noqa` (`RUF101`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.6
head: ruff-stabilisations
created_at: 2024-08-13T17:58:50Z
updated_at: 2024-08-14T08:55:10Z
url: https://github.com/astral-sh/ruff/pull/12869
synced_at: 2026-01-12T15:55:42Z
```

# Stabilise `redirected-noqa` (`RUF101`)

---

_@AlexWaygood_

## Summary

Stabilise ~~two~~ one `ruff` rule for the 0.6 release:
- ~~`missing-f-string-syntax` (`RUF027`)~~
- `redirected-noqa` (`RUF101`)

These have both been in preview for a while and there are no open issues about either of them. They also both seem like very useful rules.

~~I'm fixing a tiny oversight in the RUF027 logic as part of this PR, but it's never been reported and it's unlikely to come up.~~

## Test Plan

`cargo test -p ruff_linter --lib` + ecosystem


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-13 17:59_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-08-13 17:59_

---

_Comment by @github-actions[bot] on 2024-08-13 18:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/050220b1a10d007c0522697359618ec911d06cf7/disnake/utils.py#L1161'>disnake/utils.py:1161:56:</a> RUF101 [*] `PGH001` is a redirect to `S307`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:72:</a> RUF101 [*] `RUF011` is a redirect to `B035`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/e0868b6c092c80837f86463a54d34379851d43d7/nox/manifest.py#L357'>nox/manifest.py:357:50:</a> RUF101 [*] `PGH001` is a redirect to `S307`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF101 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-08-13 18:18_

Most of the RUF027 hits seem to be true positives that I think projects would very much like to know about (so this shows the value of stabilising the rule)... however, this one is a false positive, that I think wouldn't be too hard for us to fix: https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/main.py#L1055-L1060

---

_Comment by @AlexWaygood on 2024-08-13 18:20_

Okay, this is another false positive: https://github.com/pypa/cibuildwheel/blob/c6dd39bd834ed458bb627c294854832e21971f1a/cibuildwheel/macos.py#L742. The rule should exclude logging calls in the same way that it excludes `gettext()` calls. In that case, I don't think this rule can be stabilised right now, which is a shame :(

---

_Converted to draft by @AlexWaygood on 2024-08-13 18:20_

---

_Renamed from "Stabilise 2 `ruff` rules" to "Stabilise `RUF101`" by @AlexWaygood on 2024-08-13 22:35_

---

_Renamed from "Stabilise `RUF101`" to "Stabilise `redirected-noqa` (`RUF101`)" by @AlexWaygood on 2024-08-13 22:35_

---

_Marked ready for review by @AlexWaygood on 2024-08-13 22:36_

---

_Comment by @AlexWaygood on 2024-08-13 22:36_

I've taken the RUF027 stabilisation out of this PR. I'll put up a separate PR to reduce the false positives after the release is out.

---

_@dhruvmanila approved on 2024-08-14 02:37_

---

_Label `rule` added by @MichaReiser on 2024-08-14 06:53_

---

_Merged by @AlexWaygood on 2024-08-14 08:55_

---

_Closed by @AlexWaygood on 2024-08-14 08:55_

---

_Branch deleted on 2024-08-14 08:55_

---
