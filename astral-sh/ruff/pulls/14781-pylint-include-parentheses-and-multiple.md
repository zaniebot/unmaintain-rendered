```yaml
number: 14781
title: "[`pylint`]  Include parentheses and multiple comparators in check for `boolean-chained-comparison (PLR1716)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - fixes
  - preview
assignees: []
merged: true
base: main
head: chained-parens
created_at: 2024-12-05T04:11:44Z
updated_at: 2024-12-09T04:58:50Z
url: https://github.com/astral-sh/ruff/pull/14781
synced_at: 2026-01-12T15:55:49Z
```

# [`pylint`]  Include parentheses and multiple comparators in check for `boolean-chained-comparison (PLR1716)`

---

_@dylwil3_

This PR introduces three changes to the diagnostic and fix behavior (still under preview) for [boolean-chained-comparison (PLR1716)](https://docs.astral.sh/ruff/rules/boolean-chained-comparison/#boolean-chained-comparison-plr1716).

1. We now offer a _fix_ in the case of parenthesized expressions like `(a < b) and b < c`. The fix will merge the chains of comparisons and then balance parentheses by _adding_ parentheses to one side of the expression.
2. We now trigger a diagnostic (and fix) in the case where some comparisons have multiple comparators like `a < b < c and c < d`.
3. When adjacent comparators are parenthesized, we prefer the left parenthesization and apply the replacement to the whole parenthesized range. So, for example, `a < (b) and ((b)) < c` becomes `a < (b) < c`.

While these seem like somewhat disconnected changes, they are actually related. If we only offered (1), then we would see the following fix behavior:

```diff
- (a < b) and b < c and ((c < d))
+ (a < b < c) and ((c < d))
```

This is because the fix which add parentheses to the first pair of comparisons overlaps with the fix that removes the `and` between the second two comparisons. So the latter fix is deferred. However, the latter fix does not get a second chance because, upon the next lint iteration, there is no violation of `PLR1716`.

Upon adopting (2), however, both fixes occur by the time ruff completes several iterations and we get:

```diff
- (a < b) and b < c and ((c < d))
+ ((a < b < c < d))
```

Finally, (3) fixes a previously unobserved bug wherein the autofix for `a < (b) and b < c` used to result in `a<(b<c` which gives a syntax error. It could in theory have been fixed in a separate PR, but seems to be on theme here.


----------

- Closes #13524
- (1), (2), and (3) are implemented in separate commits for ease of review and modification.
- Technically a user can trigger an error in ruff (by reaching max iterations) if they have a humongous boolean chained comparison with differing parentheses levels.



---

_Label `rule` added by @dylwil3 on 2024-12-05 04:11_

---

_Label `fixes` added by @dylwil3 on 2024-12-05 04:11_

---

_Label `preview` added by @dylwil3 on 2024-12-05 04:11_

---

_Comment by @github-actions[bot] on 2024-12-05 04:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +2 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/7aa40e84c52af37a555d8656f2a407b54221b249/astropy/cosmology/flrw/lambdacdm.py#L267'>astropy/cosmology/flrw/lambdacdm.py:267:15:</a> PLR1716 Contains chained boolean comparison that can be simplified
+ <a href='https://github.com/astropy/astropy/blob/7aa40e84c52af37a555d8656f2a407b54221b249/astropy/cosmology/flrw/lambdacdm.py#L267'>astropy/cosmology/flrw/lambdacdm.py:267:15:</a> PLR1716 [*] Contains chained boolean comparison that can be simplified
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1716 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---

_Comment by @dylwil3 on 2024-12-06 05:22_

No obligation to review @zanieb , but figured I'd ping you since you wrote the first PR addressing this issue. Let me know if this fix is way off base from what you had in mind 😄 

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/boolean_chained_comparison.py`:132 on 2024-12-06 16:41_

Can we add a few test cases where the expressions themselves are parenthesized and have comments? 

```
a < ( # sneaky comment
	b
  # more comments 
) and b < c
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:78 on 2024-12-06 16:53_

Very not related to your change but it confused me big time when reading through the code. Would you mind changing the comment on line 65 and the variable on line 69 not to mention statements, as these are clearly expressions.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:78 on 2024-12-06 16:55_

 Oh, and nice find on the `first`. That's sneaky

---

_@MichaReiser approved on 2024-12-06 17:01_

 This is great. I suggest adding some more tests with comments because the comment range (or parentheses for that fact) isn't part of the expression range

---

_@dylwil3 reviewed on 2024-12-06 17:58_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/boolean_chained_comparison.py`:132 on 2024-12-06 17:58_

> where the expressions themselves are parenthesized

good catch! the fix causes a syntax error in that case. 

In fact, that bug is present in the _current_ version of the fix too: Apply the fix in this [playground example](https://play.ruff.rs/9fb781e1-3189-4c24-b297-0e98b766fb1f) and you get `SyntaxError: unexpected EOF while parsing [Ln 1, Col 7]`.

I see the cause of the issue (we're ignoring parentheses when comparing expressions before merging them), but I'll have to tinker a little to figure out how to solve this.

Once I do I'll also check to see if comments are an issue.

Thanks again for finding this!


---

_@MichaReiser reviewed on 2024-12-06 18:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/boolean_chained_comparison.py`:132 on 2024-12-06 18:03_

I think it's also fine to bail on providing a fix in those cases

---

_@dylwil3 reviewed on 2024-12-06 18:48_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/boolean_chained_comparison.py`:132 on 2024-12-06 18:48_

Actually that wasn't so bad! Should be fixed, and added some tests mixing the various new behaviors.

---

_Merged by @dylwil3 on 2024-12-09 04:58_

---

_Closed by @dylwil3 on 2024-12-09 04:58_

---

_Branch deleted on 2024-12-09 04:58_

---
