```yaml
number: 10704
title: "[`pycodestyle`] Do not trigger `E3` rules on defs following a function/method with a dummy body"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-10211
created_at: 2024-04-01T12:21:55Z
updated_at: 2024-04-15T08:27:14Z
url: https://github.com/astral-sh/ruff/pull/10704
synced_at: 2026-01-10T22:37:01Z
```

# [`pycodestyle`] Do not trigger `E3` rules on defs following a function/method with a dummy body

---

_Pull request opened by @hoel-bagard on 2024-04-01 12:21_

## Summary

Closes #10211

This PR keeps track of functions with a dummy body, and does not trigger `E301`, `E302` or `E306` on defs following functions with a dummy body.

## Test Plan

The code snippets from the issue have been added to the fixture.
On that note, so far the test cases for the `E3` rules have been grouped by rule, since it made things easier when checking the results by hand. However it now makes it harder to review the snapshots (here nothing really changed, only line numbers), so I can add the new tests at the end of the fixture file if that makes things easier.

---

_Comment by @github-actions[bot] on 2024-04-01 12:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -5 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e7a19aca757820fe53f896ab15ad4128bf94838a/scripts/lib/zulip_tools.py#L601'>scripts/lib/zulip_tools.py:601:1:</a> E302 [*] Expected 2 blank lines, found 0
- <a href='https://github.com/zulip/zulip/blob/e7a19aca757820fe53f896ab15ad4128bf94838a/zerver/decorator.py#L556'>zerver/decorator.py:556:1:</a> E302 [*] Expected 2 blank lines, found 0
- <a href='https://github.com/zulip/zulip/blob/e7a19aca757820fe53f896ab15ad4128bf94838a/zerver/lib/validator.py#L268'>zerver/lib/validator.py:268:1:</a> E302 [*] Expected 2 blank lines, found 0
- <a href='https://github.com/zulip/zulip/blob/e7a19aca757820fe53f896ab15ad4128bf94838a/zproject/config.py#L33'>zproject/config.py:33:1:</a> E302 [*] Expected 2 blank lines, found 0
- <a href='https://github.com/zulip/zulip/blob/e7a19aca757820fe53f896ab15ad4128bf94838a/zproject/config.py#L56'>zproject/config.py:56:1:</a> E302 [*] Expected 2 blank lines, found 0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E302 | 5 | 0 | 5 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @hoel-bagard on 2024-04-01 12:42_

---

_Label `rule` added by @MichaReiser on 2024-04-08 13:24_

---

_Label `preview` added by @MichaReiser on 2024-04-08 13:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:457 on 2024-04-08 13:26_

Would you mind adding a test where the `...` is on its own line? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:782 on 2024-04-08 13:30_

Should we move this check into the `Function` branch of the above match statement? Because I think it shouldn't apply if we have 


```python
class A:...
class B: ...
```

We could write it as:

```
state.follows = if logical_line.last_token == TokenKind::Ellipsis {
    Follows::DumyDef
} else {
    Follows::Def
};
```

(could be worth adding a test for this)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:1039 on 2024-04-08 13:31_

Nit: It might be worth extracting this check into a small helper function to avoid repeating it three times (and it gives as the opportunity to give it a meaningful name).

---

_@MichaReiser reviewed on 2024-04-08 13:31_

Thank you. 

The ecosystem checks look good to me. I recommend adding a few more tests (see inline comments) but we can then land your change. 

---

_@hoel-bagard reviewed on 2024-04-10 13:41_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:782 on 2024-04-10 13:41_

Done in 09d0bdd27.

---

_@hoel-bagard reviewed on 2024-04-10 13:47_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:457 on 2024-04-10 13:47_

I can, but that should be a violation, right ?

---

_@MichaReiser reviewed on 2024-04-10 13:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:457 on 2024-04-10 13:54_

Yes, it should require a blank line if the body is not collapsed.

---

_@hoel-bagard reviewed on 2024-04-10 14:23_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:457 on 2024-04-10 14:23_

Done in [7197500](https://github.com/astral-sh/ruff/pull/10704/commits/71975001dce8113be4e0e0b8f32550233a7ac74a).

---

_@hoel-bagard reviewed on 2024-04-10 14:23_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:1039 on 2024-04-10 14:23_

Done in [6751ef1](https://github.com/astral-sh/ruff/pull/10704/commits/6751ef14b5673432b40a8b5c8a44b95a4bc02d70).

---

_Comment by @MichaReiser on 2024-04-11 06:48_

Thanks for addressing the feedback. I overlooked this last time but the rule should continue to error if there's only one blank line between two functions (it should either be 0 or 2, but never 1).  I found this in the ecosystem checks.

```python
@overload
def components(models: dict[str, Model], wrap_script: bool = ..., # type: ignore[overload-overlap] # XXX: mypy bug
    wrap_plot_info: Literal[True] = ..., theme: ThemeLike = ...) -> tuple[str, dict[str, str]]: ...
@overload
def components(models: dict[str, Model], wrap_script: bool = ..., wrap_plot_info: Literal[False] = ...,
    theme: ThemeLike = ...) -> tuple[str, dict[str, RenderRoot]]: ...

def components(models: Model | Sequence[Model] | dict[str, Model], wrap_script: bool = True,
               wrap_plot_info: bool = True, theme: ThemeLike = None) -> tuple[str, Any]:
    ''' Return HTML components to embed a Bokeh plot. The data for the plot is
    stored directly in the returned HTML.

    An example can be found in examples/embed/embed_multiple.py'''
```

This should error because there's only a single blank line between the implementation and the last `@overload` declaration.

---

_Review requested from @MichaReiser by @hoel-bagard on 2024-04-13 09:02_

---

_Comment by @hoel-bagard on 2024-04-13 09:05_

I didn't think about that, I've fixed it in [ad7e466](https://github.com/astral-sh/ruff/pull/10704/commits/ad7e466444904a3b1c3bc035ce91e9af23e9b72d).

---

_@MichaReiser approved on 2024-04-15 08:23_

Thanks

---

_Merged by @MichaReiser on 2024-04-15 08:23_

---

_Closed by @MichaReiser on 2024-04-15 08:23_

---

_Branch deleted on 2024-04-15 08:27_

---
