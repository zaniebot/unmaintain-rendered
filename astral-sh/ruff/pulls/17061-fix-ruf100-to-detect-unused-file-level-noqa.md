```yaml
number: 17061
title: "Fix RUF100 to detect unused file-level noqa directives with specific codes (#17042)"
type: pull_request
state: merged
author: maxmynter
labels:
  - rule
assignees: []
merged: true
base: main
head: noqa
created_at: 2025-03-30T09:20:57Z
updated_at: 2025-04-07T14:21:52Z
url: https://github.com/astral-sh/ruff/pull/17061
synced_at: 2026-01-10T19:40:36Z
```

# Fix RUF100 to detect unused file-level noqa directives with specific codes (#17042)

---

_Pull request opened by @maxmynter on 2025-03-30 09:20_

Closes #17042

## Summary
This PR fixes the issue outlined in #17042 where RUF100 (unused-noqa) fails to detect unused file-level noqa directives (`# ruff: noqa` or `# ruff: noqa: {code}`).

The issue stems from two underlying causes:

1. For blanket file-level directives (`# ruff: noqa`), there's a circular dependency: the directive exempts all rules including RUF100 itself, which prevents checking for usage. This isn't changed by this PR. I would argue it is intendend behavior - a blanket `# ruff: noqa` directive should exempt all rules including RUF100 itself.

2. For code-specific file-level directives (e.g. `# ruff: noqa: F841`), the handling was missing in the `check_noqa` function. This is added in this PR.

## Notes
- For file-level directives, the `matches` array is pre-populated with the specified codes during parsing, unlike line-level directives which only populate their `matches` array when actually suppressing diagnostics. This difference requires the somewhat clunky handling of both cases. I would appreciate guidance on a cleaner design :) 

- A more fundamental solution would be to change how file-level directives initialize the `matches` array in `FileNoqaDirectives::extract()`, but that requires more substantial changes as it breaks existing functionality. I suspect discussions in #16483 are relevant for this.

## Test Plan
- Local verification
- Added a test case and fixture

---

_Review requested from @dylwil3 by @MichaReiser on 2025-03-30 12:42_

---

_Assigned to @dylwil3 by @MichaReiser on 2025-03-30 12:42_

---

_Comment by @github-actions[bot] on 2025-03-30 14:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+17 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/8e813b0a73c6898273ffc688dc1eebfe56029a6d/ibis/backends/tests/signature/typecheck.py#L8'>ibis/backends/tests/signature/typecheck.py:8:1:</a> RUF100 Unused `noqa` directive (unused: `D205`, `D415`, `D400`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/1c17cd429c2f91b0066547deb99c537af3e54d39/stdlib/asyncio/__init__.pyi#L1'>stdlib/asyncio/__init__.pyi:1:1:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLR5501`)
+ <a href='https://github.com/python/typeshed/blob/1c17cd429c2f91b0066547deb99c537af3e54d39/stdlib/typing.pyi#L2'>stdlib/typing.pyi:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/155776370261210d1d8d3e69b7f4823f4a8a4183/tests/packages/importlib_editable/pkg/pmod_a.py#L1'>tests/packages/importlib_editable/pkg/pmod_a.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/155776370261210d1d8d3e69b7f4823f4a8a4183/tests/packages/importlib_editable/pkg/sub_a/pmod_b.py#L1'>tests/packages/importlib_editable/pkg/sub_a/pmod_b.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/155776370261210d1d8d3e69b7f4823f4a8a4183/tests/packages/importlib_editable/pkg/sub_b/pmod_c.py#L1'>tests/packages/importlib_editable/pkg/sub_b/pmod_c.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/155776370261210d1d8d3e69b7f4823f4a8a4183/tests/packages/importlib_editable/pkg/sub_b/sub_c/pmod_d.py#L1'>tests/packages/importlib_editable/pkg/sub_b/sub_c/pmod_d.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/155776370261210d1d8d3e69b7f4823f4a8a4183/tests/packages/importlib_editable/pkg/sub_b/sub_d/pmod_e.py#L1'>tests/packages/importlib_editable/pkg/sub_b/sub_d/pmod_e.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/155776370261210d1d8d3e69b7f4823f4a8a4183/tests/packages/importlib_editable/pmod.py#L1'>tests/packages/importlib_editable/pmod.py:1:1:</a> RUF100 [*] Unused `noqa` directive (unused: `I001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/1adbb9e152a5414935f8ab0af5601738ff7becef/indico/legacy/pdfinterface/conference.py#L8'>indico/legacy/pdfinterface/conference.py:8:1:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR1702`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/2f687fc9c25c102f95e0869d49edafd9f2929322/astropy/cosmology/_src/core.py#L2'>astropy/cosmology/_src/core.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/2f687fc9c25c102f95e0869d49edafd9f2929322/astropy/cosmology/_src/flrw/base.py#L2'>astropy/cosmology/_src/flrw/base.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/2f687fc9c25c102f95e0869d49edafd9f2929322/astropy/cosmology/_src/flrw/w0cdm.py#L2'>astropy/cosmology/_src/flrw/w0cdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/2f687fc9c25c102f95e0869d49edafd9f2929322/astropy/cosmology/_src/flrw/w0wacdm.py#L2'>astropy/cosmology/_src/flrw/w0wacdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/2f687fc9c25c102f95e0869d49edafd9f2929322/astropy/cosmology/_src/flrw/w0wzcdm.py#L2'>astropy/cosmology/_src/flrw/w0wzcdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/2f687fc9c25c102f95e0869d49edafd9f2929322/astropy/cosmology/_src/flrw/wpwazpcdm.py#L2'>astropy/cosmology/_src/flrw/wpwazpcdm.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
+ <a href='https://github.com/astropy/astropy/blob/2f687fc9c25c102f95e0869d49edafd9f2929322/astropy/units/tests/test_quantity_annotations.py#L2'>astropy/units/tests/test_quantity_annotations.py:2:1:</a> RUF100 [*] Unused `noqa` directive (unused: `FA100`, `FA102`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 17 | 17 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-03-31 08:43_

I'll leave this to @dylwil3 to review. What's important is that we gate these changes behind preview because it is a "significant" increase of a stable rule's scope.

---

_Label `rule` added by @MichaReiser on 2025-03-31 08:43_

---

_Comment by @ddeepwell on 2025-03-31 17:56_

I'm curious how this PR handles the case with `# ruff: noqa: {code1}, {code2}`, where code2 is a code for a used directive while code1 is for an unused directive which should be removed? This case isn't in the original issue (#17042), but it's basically the same concern, except that the valid code should remain after a fix.

---

_Comment by @dylwil3 on 2025-03-31 18:02_

@maxmynter could you add some test cases along the lines of what @ddeepwell mentioned? and also gate the change behind preview when you get a chance?

I'm gonna try to check out some of the ecosystem reports to see if we're getting any false positives before diving into the implementation

Thank you for your help with this!

---

_Comment by @maxmynter on 2025-03-31 23:15_

@dylwil3 is the implementation of the feature guard in ecbe562 sufficient? 

I am a unsure since I haven't worked with feature guards -- let me know if something is missing.

---

_Comment by @maxmynter on 2025-03-31 23:47_

Thanks for flagging @ddeepwell, ~~the current implementation deletes the whole file level `ruff: noqa:` directive if one code is wrong.~~

Edit: This is not the case. I had misremembered rule [F841](https://docs.astral.sh/ruff/rules/unused-variable/) which does not apply at the file root; deletion in the example that prompted this comment was wanted.

The proposed changes handle (and test) the cases when one code is unused and the other one is used.

---

_Comment by @maxmynter on 2025-04-02 21:01_

@dylwil3 ping :)

No worries if you don't have the bandwidth at the moment; i just wanted to let you know that I'm unsure about the next steps here.

---

_Comment by @dylwil3 on 2025-04-02 21:15_

Thanks for the ping! Hoping to get to this tomorrow morning - sorry for the delay!

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/noqa.rs`:200 on 2025-04-03 13:22_

We also support `flake8: noqa:` and we should probably use the same prefix that the user had originally (even though that will likely be a bit annoying to implement). Luckily, since the directive has already been parsed, you know that it's valid syntax and could probably do something naive to tell the difference between `flake8` and `ruff` prefixes. The alternative option would be to modify the `noqa`-lexing (https://github.com/astral-sh/ruff/blob/755ece0c36ea0f1064b496f2daf4c5fd97565667/crates/ruff_linter/src/noqa.rs) to preserve the prefix. But I would recommend first seeing if there's an ad-hoc way to discern the prefix.

---

_@dylwil3 requested changes on 2025-04-03 13:25_

This looks good, thank you! The gating behind `preview` looks correct. I'm not sure why it's not reflected in the ecosystem check... I can't reproduce it locally. I'll try to investigate it.

In the meantime, I've just left one more issue to address about preserving the prefix used by the user for file-level directives.

---

_@maxmynter reviewed on 2025-04-04 04:19_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/checkers/noqa.rs`:200 on 2025-04-04 04:19_

I went with an ad hoc solution in [f8947c4](https://github.com/astral-sh/ruff/pull/17061/commits/f8947c48ef2730f0a428378ffa332fa028ee56ba) that seems to work fine. 

---

_Review requested from @dylwil3 by @maxmynter on 2025-04-04 04:25_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/noqa.rs`:201 on 2025-04-04 18:13_

We allow whitespace before the colon during `noqa` parsing, so you should just search for `flake8` here

```suggestion
                                if original_text.contains("flake8") {
```

---

_@dylwil3 requested changes on 2025-04-04 18:14_

Looks good! One small change and then looks like there are some merge conflicts to resolve as well

---

_Review requested from @dylwil3 by @maxmynter on 2025-04-04 21:53_

---

_Comment by @maxmynter on 2025-04-04 22:08_

Merge conflicts were with test cases that i added in #17138. They're resolved now. 

---

_@dylwil3 approved on 2025-04-07 14:21_

Wonderful, thank you!

---

_Merged by @dylwil3 on 2025-04-07 14:21_

---

_Closed by @dylwil3 on 2025-04-07 14:21_

---
