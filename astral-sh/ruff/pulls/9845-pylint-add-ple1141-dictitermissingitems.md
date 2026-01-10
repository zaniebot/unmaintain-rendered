```yaml
number: 9845
title: "[`pylint`] Add PLE1141 `DictIterMissingItems`"
type: pull_request
state: merged
author: DanielNoord
labels:
  - rule
assignees: []
merged: true
base: main
head: dict-iter-without-items
created_at: 2024-02-05T21:55:16Z
updated_at: 2024-02-19T14:31:08Z
url: https://github.com/astral-sh/ruff/pull/9845
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Add PLE1141 `DictIterMissingItems`

---

_Pull request opened by @DanielNoord on 2024-02-05 21:55_

## Summary

References https://github.com/astral-sh/ruff/issues/970.

Implements [`dict-iter-missing-items`](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/dict-iter-missing-items.html).

Took the tests from "upstream" [here](https://github.com/DanielNoord/pylint/blob/main/tests/functional/d/dict_iter_missing_items.py).

~I wasn't able to implement code for one false positive, but it is pretty estoric: https://github.com/pylint-dev/pylint/issues/3283. I would personally argue that adding this check as preview rule without supporting this specific use case is fine. I did add a "test" for it.~ This was implemented.

## Test Plan

Followed the Contributing guide to create tests, hopefully I didn't miss any.
Also ran CI on my own fork and seemed to be all okay 😄 

~Edit: the ecosystem check seems a bit all over the place? 😅~ All good.


---

_Comment by @github-actions[bot] on 2024-02-05 22:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-02-05 22:19_

> Edit: the ecosystem check seems a bit all over the place? 😅

Could it be that the main branch on your fork is behind the main branch on this repository? IOW, can you try rebasing your branch with the latest changes on main?

---

_Comment by @DanielNoord on 2024-02-05 22:38_

@dhruvmanila A rebase did not seem to fix the issue. The edit of the comment is still the same. I don't see how this PR could have that ecosystem impact without any test related to these messages failing?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/dict_iter_missing_items.py`:17 on 2024-02-05 23:10_

I wonder if we could avoid raising this false positive. I haven't explored this but maybe we could use `binding.statement(model)` to get the statement where the binding is defined and then we could check if the RHS is a dictionary with a 2-value tuple key.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:66 on 2024-02-05 23:15_

We could also provide a fix if you're up to implement it :)

The fix could be to replace the identifier range with `{identifier}.items()`. You can either use the AST and generate the source code but I think it would be easier to just replace it with the string value like: `format!("{}.items()", identifier)`.

---

_@dhruvmanila reviewed on 2024-02-05 23:15_

---

_Comment by @dhruvmanila on 2024-02-05 23:15_

> @dhruvmanila A rebase did not seem to fix the issue. The edit of the comment is still the same. I don't see how this PR could have that ecosystem impact without any test related to these messages failing?

Yeah, don't worry about the ecosystem results.

---

_@DanielNoord reviewed on 2024-02-06 22:40_

---

_Review comment by @DanielNoord on `crates/ruff_linter/resources/test/fixtures/pylint/dict_iter_missing_items.py`:17 on 2024-02-06 22:40_

I played around with this and came up with something, as well as two additional test cases to make sure it "works".

It's in the last commit but I'm really not sure about the style (this `rust` thing is kind a new to me). So please tell me how to format it to be as expected (feel free to push to the branch).

---

_@DanielNoord reviewed on 2024-02-06 22:41_

---

_Review comment by @DanielNoord on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:66 on 2024-02-06 22:41_

I tried to keep the PR small as it is my first, but this proved to be really easy indeed. Added a fix. Classified it as safe as I think it is? But please let me know if it isn't.

---

_Comment by @DanielNoord on 2024-02-06 22:42_

Thanks for the feedback @dhruvmanila.

I force pushed as I made a `clippy` error in a previous commit. This should address your comments. Hopefully the `ecosystem` check can help out here as well :)

---

_Review requested from @dhruvmanila by @DanielNoord on 2024-02-07 12:58_

---

_@dhruvmanila reviewed on 2024-02-07 14:36_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/dict_iter_missing_items.py`:17 on 2024-02-07 14:36_

Thanks! Looks good. I've extracted out the logic to a function to avoid having nested if conditions.

---

_@DanielNoord reviewed on 2024-02-07 14:37_

---

_Review comment by @DanielNoord on `crates/ruff_linter/resources/test/fixtures/pylint/dict_iter_missing_items.py`:17 on 2024-02-07 14:37_

Thanks, that looks much better!

---

_Comment by @codspeed-hq[bot] on 2024-02-07 14:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/DanielNoord:dict-iter-without-items)

### Merging #9845 will **not alter performance**

<sub>Comparing <code>DanielNoord:dict-iter-without-items</code> (76d85ec) with <code>main</code> (b4f2882)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@dhruvmanila approved on 2024-02-07 14:50_

Thanks!

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-02-07 14:50_

---

_Comment by @DanielNoord on 2024-02-19 08:26_

Hi, @charliermarsh is there anything I can do to move this PR forward?

---

_Label `rule` added by @dhruvmanila on 2024-02-19 11:07_

---

_Comment by @dhruvmanila on 2024-02-19 14:26_

I think it's good to go. I'll merge it but feel free to review it and I can make any follow-up changes @charliermarsh 

---

_Renamed from "Add PLE1141, ``DictIterMissingItems``" to "[`pylint`] Add PLE1141 `DictIterMissingItems`" by @dhruvmanila on 2024-02-19 14:26_

---

_Merged by @dhruvmanila on 2024-02-19 14:26_

---

_Closed by @dhruvmanila on 2024-02-19 14:26_

---

_Comment by @charliermarsh on 2024-02-19 14:31_

Thanks @dhruvmanila.

---
