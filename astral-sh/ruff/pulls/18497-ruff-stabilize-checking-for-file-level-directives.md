```yaml
number: 18497
title: "[`ruff`] Stabilize checking for file-level directives in `unused-noqa` (`RUF100`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-file-level-directives
created_at: 2025-06-06T14:10:07Z
updated_at: 2025-06-06T21:51:52Z
url: https://github.com/astral-sh/ruff/pull/18497
synced_at: 2026-01-12T15:56:20Z
```

# [`ruff`] Stabilize checking for file-level directives in `unused-noqa` (`RUF100`)

---

_@dylwil3_

Note that the preview behavior was not documented (shame on us!) so the documentation was not modified.

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 14:10_

---

_Label `rule` added by @dylwil3 on 2025-06-06 14:10_

---

_Comment by @github-actions[bot] on 2025-06-06 14:35_

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
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/commands/importers/v1/__init__.py#L70'>superset/commands/importers/v1/__init__.py:70:5:</a> RUF100 [*] Unused `noqa` directive (unused: `C901`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/4a935dc02d0385822e6df26b1b2d8804c39a59fe/ibis/backends/tests/signature/typecheck.py#L8'>ibis/backends/tests/signature/typecheck.py:8:1:</a> RUF100 Unused `noqa` directive (unused: `D205`, `D415`, `D400`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/971d6a30f53772e1b03efeca9f8ce0168e803034/stdlib/asyncio/__init__.pyi#L1'>stdlib/asyncio/__init__.pyi:1:1:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLR5501`)
+ <a href='https://github.com/python/typeshed/blob/971d6a30f53772e1b03efeca9f8ce0168e803034/stdlib/typing.pyi#L2'>stdlib/typing.pyi:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/tests/packages/importlib_editable/pkg/pmod_a.py#L1'>tests/packages/importlib_editable/pkg/pmod_a.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/tests/packages/importlib_editable/pkg/sub_a/pmod_b.py#L1'>tests/packages/importlib_editable/pkg/sub_a/pmod_b.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/tests/packages/importlib_editable/pkg/sub_b/pmod_c.py#L1'>tests/packages/importlib_editable/pkg/sub_b/pmod_c.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/tests/packages/importlib_editable/pkg/sub_b/sub_c/pmod_d.py#L1'>tests/packages/importlib_editable/pkg/sub_b/sub_c/pmod_d.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/tests/packages/importlib_editable/pkg/sub_b/sub_d/pmod_e.py#L1'>tests/packages/importlib_editable/pkg/sub_b/sub_d/pmod_e.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/tests/packages/importlib_editable/pmod.py#L1'>tests/packages/importlib_editable/pmod.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/tests/test_skbuild_settings.py#L2'>tests/test_skbuild_settings.py:2:1:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLC1901`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/cosmology/_src/core.py#L2'>astropy/cosmology/_src/core.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/cosmology/_src/flrw/base.py#L2'>astropy/cosmology/_src/flrw/base.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/cosmology/_src/flrw/w0cdm.py#L2'>astropy/cosmology/_src/flrw/w0cdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/cosmology/_src/flrw/w0wacdm.py#L2'>astropy/cosmology/_src/flrw/w0wacdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/cosmology/_src/flrw/w0wzcdm.py#L2'>astropy/cosmology/_src/flrw/w0wzcdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/cosmology/_src/flrw/wpwazpcdm.py#L2'>astropy/cosmology/_src/flrw/wpwazpcdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/units/photometric.py#L14'>astropy/units/photometric.py:14:1:</a> RUF100 [*] Unused `noqa` directive (unused: `F821`)
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/units/tests/test_quantity_annotations.py#L2'>astropy/units/tests/test_quantity_annotations.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `FA100`, `FA102`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 19 | 19 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-06-06 14:49_

A little confused by this one, which looks incorrect: 


> [stdlib/typing.pyi:2:1:](https://github.com/python/typeshed/blob/971d6a30f53772e1b03efeca9f8ce0168e803034/stdlib/typing.pyi#L2) RUF100 [*] Unused `noqa` directive (unused: `F811`)

Will investigate...

Update: Nope, this is correct! Apparently Ruff has been clever enough to notice that `overload` is `typing.overload` here since 0.4.0.

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:124 on 2025-06-06 20:49_

I know it was already like this, but do we need to `collect` here? It looks like we just immediately put this back in a for loop.

---

_@ntBre approved on 2025-06-06 20:50_

LGTM, one minor suggestion while we're here

---

_@dylwil3 reviewed on 2025-06-06 21:41_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/noqa.rs`:124 on 2025-06-06 21:41_

good catch!

---

_Merged by @dylwil3 on 2025-06-06 21:51_

---

_Closed by @dylwil3 on 2025-06-06 21:51_

---

_Branch deleted on 2025-06-06 21:51_

---
