```yaml
number: 16480
title: Add new rule InEmptyCollection
type: pull_request
state: merged
author: naslundx
labels:
  - rule
assignees: []
merged: true
base: main
head: add-rule-in-empty-collection
created_at: 2025-03-03T19:11:59Z
updated_at: 2025-05-07T08:31:29Z
url: https://github.com/astral-sh/ruff/pull/16480
synced_at: 2026-01-12T15:55:55Z
```

# Add new rule InEmptyCollection

---

_@naslundx_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Introducing a new rule based on discussions in #15732 and #15729 that checks for unnecessary in with empty collections. 

I called it in_empty_collection and gave the rule number PLR6202 but of course open for suggestions/clarifications.

Technically fixable (can be replaced by boolean or, if in an if-statement, removed entirely) and can implement that as well if it makes sense.

Rule is in preview group. (Tried following https://docs.astral.sh/ruff/contributing/#example-adding-a-new-lint-rule as best I could)


## Test Plan

<!-- How was it tested? -->

Tried locally against sample code. Added unit test, and created snapshot.


---

_Comment by @github-actions[bot] on 2025-03-03 19:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/dc7f34491baea805b5cd01d10e93bd2a8729cb6d/tools/lib/provision.py#L153'>tools/lib/provision.py:153:28:</a> RUF060 Unnecessary membership test on empty collection
+ <a href='https://github.com/zulip/zulip/blob/dc7f34491baea805b5cd01d10e93bd2a8729cb6d/tools/lib/provision.py#L153'>tools/lib/provision.py:153:73:</a> RUF060 Unnecessary membership test on empty collection
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/89b84cb56295c46e1d8834b599e858bc65c15a5b/testing/test_assertrewrite.py#L630'>testing/test_assertrewrite.py:630:20:</a> RUF060 Unnecessary membership test on empty collection
+ <a href='https://github.com/pytest-dev/pytest/blob/89b84cb56295c46e1d8834b599e858bc65c15a5b/testing/test_assertrewrite.py#L630'>testing/test_assertrewrite.py:630:32:</a> RUF060 Unnecessary membership test on empty collection
+ <a href='https://github.com/pytest-dev/pytest/blob/89b84cb56295c46e1d8834b599e858bc65c15a5b/testing/test_assertrewrite.py#L637'>testing/test_assertrewrite.py:637:39:</a> RUF060 Unnecessary membership test on empty collection
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF060 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-03-03 19:32_

---

_Comment by @MichaReiser on 2025-03-04 10:38_

@dylwil3 would you mind taking a look at this? I think you've most context on the change as you've been involved in both linked issues

---

_Assigned to @dylwil3 by @MichaReiser on 2025-03-04 10:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/in_empty_collection.rs`:56 on 2025-03-06 13:59_

Just as in the other PR, we can use that `match_builtin` helper here.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/in_empty_collection.rs`:38 on 2025-03-06 14:00_

maybe call it something like `is_empty_collection` instead?

---

_@dylwil3 reviewed on 2025-03-06 14:06_

Thanks @naslundx , this looks nice! One thing is that my comment in the linked issue was that we should open a separate issue to decide whether this should be a rule, but we never actually did that. 

So would you be willing to propose this as a rule in a new issue, and link this PR to it? Then we can see what folks think and do some design/decision-making there.

If we do decide to go ahead with this rule, then we'll also probably have to move it to a `RUF` rule, otherwise there will be a clash when `pylint` adds an `R6202`.

---

_Comment by @naslundx on 2025-03-08 19:54_

Thanks @dylwil3 ! Created an issue here and to reach a decision https://github.com/astral-sh/ruff/issues/16569

Will let the PR rest until then.

---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/pylint/in_empty_collection.py`:1 on 2025-03-09 14:15_

Can you also detect something like this?

```python
a=[]
b= 7
if b in a:
    pass
```

I think is possible or?

---

_@VascoSch92 reviewed on 2025-03-09 14:15_

---

_@naslundx reviewed on 2025-05-04 18:39_

---

_Review comment by @naslundx on `crates/ruff_linter/resources/test/fixtures/pylint/in_empty_collection.py`:1 on 2025-05-04 18:39_

With the current implementation, no. If there are mechanisms implemented to allow for this, please point me in the right direction!

---

_@dylwil3 reviewed on 2025-05-04 21:34_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/in_empty_collection.py`:1 on 2025-05-04 21:34_

Something like this may be possible when the symbol is defined in the same file, but it would be a bit tricky to do reliably especially when there is complicated control flow. I think we can look into extending the rule to cover this case later, but for now I think we should stick with catching literals.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/in_empty_collection.rs`:48 on 2025-05-04 21:46_

We certainly at least want `list`, `tuple`,  and `dict` here as well, but maybe we also want:

- a match arm for strings, bytes literals, and f-strings of length zero?
- the remaining builtin functions that create empty containers, which I think are: `bytearray`, `bytes`, `frozenset`, and `str`

---

_@dylwil3 approved on 2025-05-04 21:47_

Thanks for this! Would you mind adding a few more containers and tests for them?

---

_@naslundx reviewed on 2025-05-05 14:21_

---

_Review comment by @naslundx on `crates/ruff_linter/src/rules/pylint/rules/in_empty_collection.rs`:48 on 2025-05-05 14:21_

Thanks, that makes a lot of sense! The f-string was a bit tricky to figure out but I think I got it right.

---

_@naslundx reviewed on 2025-05-05 14:21_

---

_Review comment by @naslundx on `crates/ruff_linter/src/rules/pylint/rules/in_empty_collection.rs`:48 on 2025-05-05 14:21_

Updates are in a new commit

---

_Comment by @naslundx on 2025-05-05 14:26_

> we'll also probably have to move it to a RUF rule, otherwise there will be a clash when pylint adds an R6202.

Lmk if I should change the code @dylwil3 

---

_Comment by @dylwil3 on 2025-05-05 17:30_

> Lmk if I should change the code @dylwil3

Yes thank you for the reminder! Looks like you're lucky number `RUF060`


---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/in_empty_collection.rs`:60 on 2025-05-05 17:43_

I think this won't catch implicit concatenation like `"a" in f"" ""`, so maybe:

```suggestion
        Expr::FString(s) => s
            .value
            .elements()
            .all(|elt| elt.as_literal().is_some_and(|elt| elt.is_empty())),
```

---

_@dylwil3 requested changes on 2025-05-05 17:44_

Almost there! One nit and then we'll need to move the rule to `RUF060`. Thanks for bearing with me!

---

_Comment by @naslundx on 2025-05-06 08:02_

Change rule number to RUF060 :heavy_check_mark: 

---

_@naslundx reviewed on 2025-05-06 08:46_

---

_Review comment by @naslundx on `crates/ruff_linter/src/rules/pylint/rules/in_empty_collection.rs`:60 on 2025-05-06 08:46_

Thanks!

---

_@dylwil3 approved on 2025-05-06 13:50_

This is a great contribution, and I was really impressed with how comfortable you got with the codebase so quickly! Thank you!

---

_Merged by @dylwil3 on 2025-05-06 13:52_

---

_Closed by @dylwil3 on 2025-05-06 13:52_

---

_Comment by @naslundx on 2025-05-07 08:31_

Thanks a lot @dylwil3 ! 
I think there is still some room for improvement on this rule, I'll take another look when I find some time. :)

---
