```yaml
number: 16607
title: "[ruff-0.10] [`flake8-pyi`] Stabilize preview-mode behaviours for `custom-type-var-for-self`(`PYI019`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: alex/pyi019-stabilize
created_at: 2025-03-10T17:12:17Z
updated_at: 2025-03-11T16:05:28Z
url: https://github.com/astral-sh/ruff/pull/16607
synced_at: 2026-01-12T15:55:55Z
```

# [ruff-0.10] [`flake8-pyi`] Stabilize preview-mode behaviours for `custom-type-var-for-self`(`PYI019`)

---

_@AlexWaygood_

## Summary

This PR stabilizes several preview-only behaviours for `custom-typevar-for-self` (`PYI019`). Namely:
- A new, more accurate technique is now employed for detecting custom TypeVars that are replaceable with `Self`. See https://github.com/astral-sh/ruff/pull/15888 for details.
- The range of the diagnostic is now the full function header rather than just the return annotation. (Previously, the rule only applied to methods with return annotations, but this is no longer true due to the changes in the first bullet point.)
- The fix is now available even when preview mode is not enabled.

## Test Plan

- Existing snapshots that do not have preview mode enabled are updated
- Preview-specific snapshots are removed
- I'll check the ecosystem report on this PR to verify everything's as expected

---

_Label `rule` added by @AlexWaygood on 2025-03-10 17:12_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-03-10 17:12_

---

_Review requested from @ntBre by @AlexWaygood on 2025-03-10 17:12_

---

_Comment by @github-actions[bot] on 2025-03-10 17:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+19 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/644882faffb15f19cacd09850fccecc782411356/superset/sql/parse.py#L167'>superset/sql/parse.py:167:21:</a> PYI019 [*] Use `Self` instead of custom TypeVar `TBaseSQLStatement`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/781182cbb00d5587e29a0cb7a3d27459a0f0a6dc/pandas/_libs/tslibs/timedeltas.pyi#L97'>pandas/_libs/tslibs/timedeltas.pyi:97:16:</a> PYI019 Use `Self` instead of custom TypeVar `_S`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stdlib/_typeshed/__init__.pyi#L331'>stdlib/_typeshed/__init__.pyi:331:16:</a> PYI019 Use `Self` instead of custom TypeVar `Self`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stdlib/_typeshed/__init__.pyi#L333'>stdlib/_typeshed/__init__.pyi:333:24:</a> PYI019 Use `Self` instead of custom TypeVar `Self`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stdlib/typing.pyi#L960'>stdlib/typing.pyi:960:15:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_T`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stdlib/typing_extensions.pyi#L237'>stdlib/typing_extensions.pyi:237:15:</a> PYI019 Use `Self` instead of custom TypeVar `_T`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/PyYAML/yaml/representer.pyi#L28'>stubs/PyYAML/yaml/representer.pyi:28:24:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_R`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/PyYAML/yaml/representer.pyi#L30'>stubs/PyYAML/yaml/representer.pyi:30:30:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_R`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/protobuf/google/protobuf/internal/containers.pyi#L36'>stubs/protobuf/google/protobuf/internal/containers.pyi:36:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/protobuf/google/protobuf/internal/containers.pyi#L52'>stubs/protobuf/google/protobuf/internal/containers.pyi:52:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/protobuf/google/protobuf/internal/containers.pyi#L76'>stubs/protobuf/google/protobuf/internal/containers.pyi:76:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/protobuf/google/protobuf/internal/containers.pyi#L99'>stubs/protobuf/google/protobuf/internal/containers.pyi:99:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/protobuf/google/protobuf/message.pyi#L30'>stubs/protobuf/google/protobuf/message.pyi:30:21:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/protobuf/google/protobuf/message.pyi#L31'>stubs/protobuf/google/protobuf/message.pyi:31:23:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/041580d6d64f03e8dc7b2fc0f9ff5258e91f6ffb/stubs/protobuf/google/protobuf/message.pyi#L34'>stubs/protobuf/google/protobuf/message.pyi:34:19:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/602d0f4914d09182561042f7000cb5cd29771270/zerver/data_import/slack.py#L109'>zerver/data_import/slack.py:109:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `SlackBotEmailT`
+ <a href='https://github.com/zulip/zulip/blob/602d0f4914d09182561042f7000cb5cd29771270/zerver/lib/thumbnail.py#L55'>zerver/lib/thumbnail.py:55:20:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/cosmology/_src/core.py#L485'>astropy/cosmology/_src/core.py:485:26:</a> PYI019 Use `Self` instead of custom TypeVar `_FlatCosmoT`
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/cosmology/_src/flrw/base.py#L1705'>astropy/cosmology/_src/flrw/base.py:1705:16:</a> PYI019 Use `Self` instead of custom TypeVar `_FlatFLRWMixinT`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI019 | 19 | 19 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-03-10 17:22_

The ecosystem hits all LGTM

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:75 on 2025-03-10 18:21_

Should we update both PEP names. It feels somewhat inconsistent to use a `-` here but not for PEP 673 (I don't think we have a standard for this but I suspect that no dash is more commonly used?)

---

_@MichaReiser approved on 2025-03-10 18:22_

Thank you

---

_@ntBre approved on 2025-03-10 22:08_

---

_@ntBre reviewed on 2025-03-10 22:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:75 on 2025-03-10 22:10_

It looks like this `PEP-695` is only used in a hyphenated expression (`[PEP-695]-style`), which probably explains the extra dash in this one.

---

_@AlexWaygood reviewed on 2025-03-11 16:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:75 on 2025-03-11 16:05_

yes, Brent has it exactly right. The unhyphenated form is usually preferred, so that's why "PEP 673" should remain unhyphenated in the docs above this. But PEP-695-style is a hyphenated expression, so it makes sense for the hyphen to remain there between "PEP" and "695"

---

_Merged by @AlexWaygood on 2025-03-11 16:05_

---

_Closed by @AlexWaygood on 2025-03-11 16:05_

---

_Branch deleted on 2025-03-11 16:05_

---
