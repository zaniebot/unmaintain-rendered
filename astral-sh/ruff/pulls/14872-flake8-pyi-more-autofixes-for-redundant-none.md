```yaml
number: 14872
title: "[`flake8-pyi`]: More autofixes for redundant-none-literal (`PYI061`)"
type: pull_request
state: merged
author: kiran-4444
labels:
  - fixes
assignees: []
merged: true
base: main
head: flake8-pyi061-improvement
created_at: 2024-12-09T13:51:39Z
updated_at: 2024-12-12T19:45:27Z
url: https://github.com/astral-sh/ruff/pull/14872
synced_at: 2026-01-12T15:55:49Z
```

# [`flake8-pyi`]: More autofixes for redundant-none-literal (`PYI061`)

---

_@kiran-4444_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR solves #14537. I've tested it, and it's working as expected. I'll be updating this with tests tomorrow. The implementation isn't yet sanitised and can be refactored further. Any feedback in doing this is greatly appreciated.

## Test Plan

Tested locally, will update the tests.


---

_Review requested from @AlexWaygood by @kiran-4444 on 2024-12-09 13:51_

---

_Renamed from "[`flake8-pyi`]: More autofixes for redundant-none-literal/PYI061 (`PYI061`)" to "[`flake8-pyi`]: More autofixes for redundant-none-literal (`PYI061`)" by @kiran-4444 on 2024-12-09 13:51_

---

_Label `fixes` added by @AlexWaygood on 2024-12-09 14:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:229 on 2024-12-09 15:08_

this will only work if the user has `from typing import Literal` somewhere in the file. If the user has `import typing` somewhere in the file instead of `from typing import Literal`, the fix here will break the user's code, which won't be good! Instead of using an `ExprName` that we're constructing ourselves as the subscript value, we should just use the value of the original subscript expression

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-09 15:10_

this isn't quite right: this will lead to the fix producing changes like this:

```diff
- Literal[1, None]
+ Literal[1] | Literal[None]
```

but what we actually want it to do is something like this:

```diff
- Literal[1, None]
+ Literal[1] | None
```

the good news is that this means that you can simplify your code a little bit :-)

```suggestion
            right: Box::new(Expr::NoneLiteral(
                ExprNoneLiteral {
                    range: TextRange::default(),
                }
            )),
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:99 on 2024-12-09 15:12_

the type of the `checker` variable is already `&mut Checker` here, so this `borrow_mut()` call isn't necessary (and means we can avoid the import of the `BorrowMut` trait at the top of the file)

```suggestion
        create_fix_edit_2(checker, literal_expr, &literal_elements).map(|edit| {
```

---

_@AlexWaygood reviewed on 2024-12-09 15:13_

Thanks! A couple of issues here, but this broadly looks like it's on the right track :-)

---

_@kiran-4444 reviewed on 2024-12-10 05:57_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:229 on 2024-12-10 05:57_

Oh yes, I totally missed this. Thanks!

---

_@kiran-4444 reviewed on 2024-12-10 05:59_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-10 05:59_

This seems to behave a bit weird though:
```
- Literal[1, None] # is being formatted to:
+ Literal[1,] | None
```
I'm not sure where that trailing comma is from.


But the following is working fine:
```
- Literal[1,2,3,None]
+ Literal[1,2,3] | None
```

---

_@AlexWaygood reviewed on 2024-12-10 13:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-10 13:20_

The trailing comma is because Python has a different AST for subscript slices with only a single element than it does for subscript slices with multiple elements. You can see this quite easily if you use Python's `ast` module to inspect the AST of a snippet. The first of these has the node for `1` directly as the subscript slice, but the second of these has a `Tuple` node as the subscript slice, with the nodes for `1` and `2` as the tuple's elements:

```pycon
>>> import ast
>>> print(ast.dump(ast.parse('x[1]'), indent=2))
Module(
  body=[
    Expr(
      value=Subscript(
        value=Name(id='x', ctx=Load()),
        slice=Constant(value=1),
        ctx=Load()))])
>>> print(ast.dump(ast.parse('x[1, 2]'), indent=2))
Module(
  body=[
    Expr(
      value=Subscript(
        value=Name(id='x', ctx=Load()),
        slice=Tuple(
          elts=[
            Constant(value=1),
            Constant(value=2)],
          ctx=Load()),
        ctx=Load()))])
```

It's the same with Ruff's AST for Python; we can see this in the Ruff playground here: https://play.ruff.rs/?secondary=AST

So you will need to check how many elements are left in the `Literal` slice after removing `None`. If there is only one element left in the `Literal` slice, you will want to remove the intermediate `Expr::Tuple` from the AST. But if there is still more than one element left in the `Literal` slice, you'll need to keep the `Expr::Tuple` there.

Does that make sense? :-)

---

_@kiran-4444 reviewed on 2024-12-11 05:49_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 05:49_

That totally made sense to me. Thanks!

---

_@AlexWaygood reviewed on 2024-12-11 07:49_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 07:49_

No problem!

---

_@kiran-4444 reviewed on 2024-12-11 07:56_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 07:56_

Do I need to update the snapshots as well? and are there any tests that I should update?

---

_@AlexWaygood reviewed on 2024-12-11 08:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 08:05_

> Do I need to update the snapshots as well?

Yes please!

> and are there any tests that I should update?

Let's just update the snapshots for now, then I'll think about how the PR looks overall once the snapshot tests are passing 👍

---

_@AlexWaygood reviewed on 2024-12-11 10:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 10:39_

@kiran-4444 you'll need to run `cargo insta review` locally and then interactively "accept" the new changes to the snapshots for the tests to pass. There shouldn't be any new snapshot files added as a result of this PR -- `cargo insta review` should merge the new file that's currently being added into the existing snapshots for the rule.

Happy to do this for you and push to your branch if you need a hand with it. You need `cargo-insta` to be installed for it to work -- there's some instructions here: https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#prerequisites

---

_@kiran-4444 reviewed on 2024-12-11 10:49_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 10:49_

Hmm, I tried this now. Let me know if this is right. Or else, please help me doing this :)

---

_@AlexWaygood reviewed on 2024-12-11 10:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 10:51_

Sorry, I should have been clearer! You first need to run `cargo test -p ruff_linter` to make sure the `cargo-insta` tool has an up-to-date idea of what the new snapshots should be from the latest version of your PR. Then you need to run `cargo insta review` to accept or reject the changes, and then you need to commit the new changes and push them to your branch.

Totally my fault for unclear instructions :-)

---

_Comment by @github-actions[bot] on 2024-12-11 11:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +16 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/13e2df0d7074cbc1a8d59d7044d5bfcb69147a3d/pandas/core/groupby/groupby.py#L4082'>pandas/core/groupby/groupby.py:4082:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/billing_page.py#L326'>corporate/views/billing_page.py:326:24:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/billing_page.py#L326'>corporate/views/billing_page.py:326:24:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L158'>corporate/views/remote_billing_page.py:158:26:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L158'>corporate/views/remote_billing_page.py:158:26:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:42:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:42:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L160'>corporate/views/remote_billing_page.py:160:48:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L160'>corporate/views/remote_billing_page.py:160:48:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L678'>corporate/views/remote_billing_page.py:678:26:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L678'>corporate/views/remote_billing_page.py:678:26:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L679'>corporate/views/remote_billing_page.py:679:42:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L679'>corporate/views/remote_billing_page.py:679:42:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L680'>corporate/views/remote_billing_page.py:680:48:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L680'>corporate/views/remote_billing_page.py:680:48:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:5:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:5:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI061 | 17 | 0 | 1 | 16 | 0 |

</p>
</details>




---

_@kiran-4444 reviewed on 2024-12-11 11:29_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 11:29_

Not an issue, should've read the contribution instructions well.

I think the snapshots are fine now? What are the next steps?

---

_@AlexWaygood reviewed on 2024-12-11 11:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:255 on 2024-12-11 11:55_

Brilliant, thanks! Yes, the snapshots are all fine now. There's still one check failing on this PR, however: our linter, Clippy, has some complaints. Could you take a look at the error messages there and try to fix them? Let me know if anything's confusing for you in what it says!

---

_@AlexWaygood approved on 2024-12-12 19:42_

Thanks @kiran-4444, this is a fantastic improvement! I pushed a couple of small changes:
- We can't safely offer the fix with the `|` syntax on Python <=3.9, as the `|` syntax for unions was added in Python 3.10. Possibly we could in the future use `typing.Union` for the fix and offer it even on older versions of Python, but I think we can leave that to another PR :-)
- I simplified the code slightly by combining the two `create_fix_edit()` functions

---

_Merged by @AlexWaygood on 2024-12-12 19:44_

---

_Closed by @AlexWaygood on 2024-12-12 19:44_

---
