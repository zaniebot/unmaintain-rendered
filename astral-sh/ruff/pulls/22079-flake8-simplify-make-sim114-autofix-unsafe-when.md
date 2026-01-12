```yaml
number: 22079
title: "[`flake8-simplify`] Make `SIM114` autofix unsafe when branches have different comments"
type: pull_request
state: open
author: phongddo
labels:
  - bug
  - fixes
assignees: []
draft: true
base: main
head: phongddo/sim114-fix-diff-comments
created_at: 2025-12-19T12:10:06Z
updated_at: 2026-01-02T11:40:26Z
url: https://github.com/astral-sh/ruff/pull/22079
synced_at: 2026-01-12T15:57:41Z
```

# [`flake8-simplify`] Make `SIM114` autofix unsafe when branches have different comments

---

_@phongddo_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ruff/issues/19576

Improves `SIM114` rule to handle branch merging with comment preservation safely, offer unsafe fixes when branches contain different comments to prevent comment loss.

## Problem

The original `SIM114` autofix has an issue where it wasn't detecting trailing comments because it was checking comments within `body_range` starting from the **end of line** rather than the **end of test** as documented. 

For example, this case was incorrectly handled:

```python
if x == 1: # 1
    print("Hello")
elif x == 2: # 2
    print("Hello")
```

Running with `--fix` would produce:

```bash
-if x == 1: # 1
-    print("Hello")
-elif x == 2: # 2
+if x == 1 or x == 2: # 1
     print("Hello")

Would fix 1 error.
```

It offers a **safe** fix even though the comments are not identical. I believe this is exactly the case mentioned in the reported issue, so in this situation we should offer unsafe fixes instead. In my opinion, this is a good strategy and consistent with how other rules are handled as @ntBre mentioned in other PR linked to the issue (please let me know what you think). 

## Solution

- Fixed `body_range` to start from the end of the test (as it's documented) instead of the end of the test's line, allowing detection of trailing comments and validation against comments in the other branch before merging branches.  
- Modified `merge_branches` to offer an unsafe fix when the code block is identical but comments differ, instead of skipping the merge entirely. Otherwise, a safe fix is offered as is.
- Updated rule documentation to include `Fix Safety` section to explain unsafe fix in this scenario.

## Manual Verification

✏️ Those ecosystem reports are about unsafe fixes that we offer when merging branches with different comments.

**The above test case → unsafe fix**

```bash
SIM114 Combine `if` branches using logical `or` operator
  |
1 | / if x == 1: # 1
2 | |     print("Hello")
3 | | elif x == 2: # 2
4 | |     print("Hello")
  | |__________________^
  |
help: Combine `if` branches

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

**The issue's test case → unsafe fix** 

```bash
SIM114 Combine `if` branches using logical `or` operator
   |
 1 | / if isinstance(exc, HTTPError) and (
 2 | |     (500 <= exc.status <= 599)
 3 | |     or exc.status == 408  # server errors
 4 | |     or exc.status == 429  # request timeout
 5 | |     ):  # too many requests
 6 | |         return True
 7 | |
 8 | | # Consider all SSL errors as temporary. There are a lot of bug
 9 | | # reports from people where various SSL errors cause a crash
10 | | # but are actually just temporary. On the other hand, we have
11 | | # no information if this ever revealed a problem where retrying
12 | | # was not the right choice.
13 | | elif isinstance(exc, ssl.SSLError):
14 | |     return True
   | |_______________^
   |
help: Combine `if` branches

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

**No comments test case → safe fix** 

```bash
SIM114 [*] Combine `if` branches using logical `or` operator
  |
1 | / if x == 1:
2 | |     print("Hello")
3 | | elif x == 2:
4 | |     print("Hello")
  | |__________________^
  |
help: Combine `if` branches

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

## Test Plan
- Added a comment in snapshot test. I'm not sure how we can distinguish between a safe and an unsafe fix in the snapshot test, so please guide me 🙏 

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 12:17_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/main.py#L164'>main.py:164:33:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/99059b74429eb968569914f1951bdeca074fb7ed/test/utils.py#L268'>test/utils.py:268:62:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM114`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |
| SIM114 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/main.py#L164'>main.py:164:33:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/99059b74429eb968569914f1951bdeca074fb7ed/test/utils.py#L268'>test/utils.py:268:62:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM114`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |
| SIM114 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>





---

_Marked ready for review by @phongddo on 2025-12-19 12:30_

---

_Comment by @phongddo on 2025-12-19 12:33_

Hey @ntBre 👋 I've got this PR up with a suggestion to address the issue https://github.com/astral-sh/ruff/issues/19576. Please take a look whenever you have a moment. Thank you 😄 

---

_Review requested from @ntBre by @ntBre on 2025-12-19 17:30_

---

_Label `bug` added by @ntBre on 2025-12-19 17:30_

---

_Label `fixes` added by @ntBre on 2025-12-19 17:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM114_SIM114.py.snap`:436 on 2025-12-19 17:32_

This added `note` is how we can tell that the fix is now unsafe 👍 

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM114.py`:11 on 2025-12-19 17:34_

Ah this looks like a duplicate test case? I think I'd still lean toward not removing it here because it causes all the line numbers in the snapshot to churn. Same with the new comment below.

---

_@ntBre reviewed on 2025-12-19 17:36_

Thanks! I still need to review more closely, just a quick suggestion on reverting the test changes to make the snapshots easier to review. It should make the new unsafe notes more prominent too.

---

_Comment by @ntBre on 2025-12-19 17:38_

Could you take a look at the ecosystem check too? I don't think we should be changing when the rule applies, only the safety of its fix, but it looks like there are new diagnostics showing up.

---

_@phongddo reviewed on 2025-12-19 17:40_

---

_Review comment by @phongddo on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM114.py`:11 on 2025-12-19 17:40_

Ah, sorry for not mentioning it earlier, it’s a duplicate one so I removed it. I’m fine reverting it too!

---

_@phongddo reviewed on 2025-12-19 22:00_

---

_Review comment by @phongddo on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM114.py`:11 on 2025-12-19 22:00_

I reverted in https://github.com/astral-sh/ruff/pull/22079/commits/9981ea57b677cca8cd08671f60c7ef245313a8f1

---

_Comment by @phongddo on 2025-12-19 22:15_

> I still need to review more closely

Sure, please take your time 🙏 

> Could you take a look at the ecosystem check too? I don't think we should be changing when the rule applies, only the safety of its fix, but it looks like there are new diagnostics showing up.

I've looked into those reports, so please correct me if I'm wrong, but I think those changes make sense since we now offer an unsafe fix when merging branches with different comments. Previously, we didn't merge branches at all and simply returned without any fix ([this check](https://github.com/astral-sh/ruff/pull/22079/changes#diff-11a16368fae4b28fc3b47729a9db05e583106d078120ecbf523de5ab43de6773L87-L89)). However, we ended up merging (without an unsafe fix) in the trailing comment scenario (the below example) due to the incorrect way we construct the body range that I fixed [here](https://github.com/astral-sh/ruff/pull/22079/changes#diff-11a16368fae4b28fc3b47729a9db05e583106d078120ecbf523de5ab43de6773R191), which I described in the PR's description:

```python
if x == 1: # 1
    print("Hello")
elif x == 2: # 2
    print("Hello")

# Snapshot test case
if a:  # we preserve comments, too!
    b
elif c:  # but not on the second branch
    b
```

With the fix, we now correctly check comment equality and offer an unsafe fix instead of ignoring it. For example, [this report from ecosystem](https://github.com/langchain-ai/langchain/blob/795e746ca70771925b65f9d3b1f551745a8316e7/libs/core/langchain_core/messages/block_translators/openai.py#L76)

```bash
+ [libs/core/langchain_core/messages/block_translators/openai.py:76:13:](https://github.com/langchain-ai/langchain/blob/795e746ca70771925b65f9d3b1f551745a8316e7/libs/core/langchain_core/messages/block_translators/openai.py#L76) SIM114 Combine `if` branches using logical `or` operator
```

**Previous** → All checks passed!

**After**

```bash
SIM114 Combine `if` branches using logical `or` operator
  |
1 |   if filename := block.get("filename"):
2 |       file["filename"] = filename
3 | / elif (extras := block.get("extras")) and ("filename" in extras):
4 | |     file["filename"] = extras["filename"]
5 | | elif (extras := block.get("metadata")) and ("filename" in extras):
6 | |     # Backward compat
7 | |     file["filename"] = extras["filename"]
  | |_________________________________________^
  |
help: Combine `if` branches

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

In my opinion, providing an unsafe fix in this situation feels more consistent with other rules like [`ASYNC115`](https://docs.astral.sh/ruff/rules/async-zero-sleep/#fix-safety). That said, please let me know if you disagree, I'm fine to revert and keep the previous behavior (by reverting the comments equality check, and it only returns safe fix) 😄 

---

_Review requested from @ntBre by @phongddo on 2025-12-19 22:29_

---

_Comment by @ntBre on 2025-12-24 16:53_

I think there are two different kinds of comments at play here. I think I agree with the comments in this test case:

https://github.com/astral-sh/ruff/blob/adef89eb7cfb6eb7c3e0646dcd120e6f1adc7af2/crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM114.py#L107-L112

that such comments should disable the rule. This seems like a clear signal to me that the user wants the two blocks of code to be separate, even if the _code_ in the body happens to be the same.

In contrast, this case:

https://github.com/astral-sh/ruff/blob/e3498121b4700f5a9dce9bf6d72af4bc411e262d/crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM114.py#L138-L141

seems like a known bug to me, recorded in our test cases. Removing this second comment should make the fix unsafe.

As I commented on the first PR (https://github.com/astral-sh/ruff/pull/19759#pullrequestreview-3112435910), I think one easy fix here would be to check if the replacement range contains any comments and mark the fix as unsafe if it does, separate from the `first_comments.eq(second_comments)` check.

However, I also kind of feel like the case in the issue, something like [this](https://play.ruff.rs/c411c2ee-7121-4263-85fa-4f891cff9e70):

```py
def foo():
    if isinstance(exc, HTTPError) and (
		(500 <= exc.status <= 599)
		or exc.status == 408  # server errors
		or exc.status == 429  # request timeout
	):  # too many requests
        return True

	# Consider all SSL errors as temporary. There are a lot of bug
	# reports from people where various SSL errors cause a crash
	# but are actually just temporary. On the other hand, we have
	# no information if this ever revealed a problem where retrying
	# was not the right choice.
    elif isinstance(exc, ssl.SSLError):
        return True
```

falls pretty clearly into the first category. If I wrote this comment, I would _not_ want an autofix to delete it, and as a result, I probably wouldn't want the rule to fire at all. So another bug fix here would be to associate that comment with one of the bodies so that the rule doesn't fire.

Overall, I think we should just avoid a diagnostic and fix entirely in both of these cases to avoid trying to differentiate between comments it's okay to delete and those it's not.

Whatever we end up doing, I think we need to add a test case like the sample from the issue because that comment placement seems a bit different from any of the other tests we have.

---

_Comment by @phongddo on 2025-12-24 18:18_

> Overall, I think we should just avoid a diagnostic and fix entirely in both of these cases to avoid trying to differentiate between comments it's okay to delete and those it's not.

I agree 👍 avoid a diagnostic and fix makes sense to me! 

---

_Comment by @phongddo on 2025-12-24 18:23_

@ntBre I reverted the unsafe fix and kept only the `body_range` fix [here](https://github.com/astral-sh/ruff/pull/22079/changes#diff-11a16368fae4b28fc3b47729a9db05e583106d078120ecbf523de5ab43de6773R176). This change would skip the diagnostic and fix entirely where branch comments differ. I also added a test case similar to the one mentioned in the issue (and adjust comments in other test cases). Please let me know what you think 😄 

---

_Comment by @ntBre on 2025-12-25 15:50_

Thank you!

Oh no, I think I see why end-of-line comments might have been treated separately :grimacing: There's a newly removed RUF100 unused noqa comment diagnostic in the [ecosystem report](https://github.com/pypa/cibuildwheel/blob/99059b74429eb968569914f1951bdeca074fb7ed/test/utils.py#L263-L277):

```py
if platform == "pyodide" and python_abi_tags is None:
    python_abi_tags = [
        "cp312-cp312",
        "cp313-cp313",
    ]
elif platform == "android" and python_abi_tags is None:  # noqa: SIM114
    python_abi_tags = [
        "cp313-cp313",
        "cp314-cp314",
    ]
elif platform == "ios" and python_abi_tags is None:
    python_abi_tags = [
        "cp313-cp313",
        "cp314-cp314",
    ]
```

I think this comment is now preventing SIM114 from firing, making it "unused" as a noqa comment, but removing it would make SIM114 trigger again. Apparently this has come up before, but I didn't realize it (https://github.com/astral-sh/ruff/issues/6025). Let's make sure to add a comment about this somewhere, it's a bit surprising.

I think we can fix this if we keep the diagnostic for removing end-of-line comments but make the fix unsafe, but suppress the diagnostic for comments like the ones in the issue. Is that doable?

Sorry for the back and forth, but I keep noticing new things in the ecosystem check!



---

_Comment by @MichaReiser on 2025-12-26 08:34_

> I think we can fix this if we keep the diagnostic for removing end-of-line comments but make the fix unsafe, but suppress the diagnostic for comments like the ones in the issue. Is that doable?

Could we detect if any of the comments are a pragma comment instead and not emit a diagnostic in that case?

---

_Comment by @ntBre on 2025-12-26 15:00_

> Could we detect if any of the comments are a pragma comment instead and not emit a diagnostic in that case?

Ah that's a good idea, I'd prefer not to emit diagnostics for the end-of-line comments otherwise too. 

(We actually need to emit a diagnostic _only_ if it's a pragma comment so that the pragma isn't unused, but I definitely agree with treating them specially)

---

_Converted to draft by @phongddo on 2026-01-02 11:28_

---

_Comment by @phongddo on 2026-01-02 11:40_

> Could we detect if any of the comments are a pragma comment instead and not emit a diagnostic in that case?

I also agree with this! I've made this PR a draft to make some changes to not emit diagnostic for pragma comments. I'll request your review again when it's ready. Thanks for the comments @ntBre @MichaReiser 🙏 


---
