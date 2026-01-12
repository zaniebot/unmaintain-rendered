```yaml
number: 2707
title: "Add `--show-fixes` flag to show applied fixes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/show-fix
created_at: 2023-02-10T03:27:13Z
updated_at: 2023-02-13T02:29:42Z
url: https://github.com/astral-sh/ruff/pull/2707
synced_at: 2026-01-12T04:52:00Z
```

# Add `--show-fixes` flag to show applied fixes

---

_Pull request opened by @charliermarsh on 2023-02-10 03:27_

# Summary

Haven't bothered to fix up the tests yet, but here's the current UI:

![Screen Shot 2023-02-09 at 10 24 05 PM](https://user-images.githubusercontent.com/1309177/217993170-e265c9ec-fed6-47a6-aae1-f0b8a415f783.png)

Once merged, closes #2661.

---

_Comment by @charliermarsh on 2023-02-10 03:29_

@henryiii - This is just a first draft, but from your perspective, what's missing from the output?

You'd mentioned line numbers in the issue, but line numbers are a little tricky to include here, because they won't map to lines in the changed file, and they might not even map to line numbers in the _original_ file, because we preform multiple autofix passes. That is: sometimes, fixing a rule introduces a new violation, so we continue autofixing until the code stabilizes (e.g., if we remove a statement, it might render an import unused, and so on).


---

_Comment by @charliermarsh on 2023-02-10 03:30_

We _could_ include the location information and message for every fixed violation, but the outputs are going to be confusing in some cases due to the above disclaimer.

---

_Label `cli` added by @charliermarsh on 2023-02-10 04:09_

---

_Comment by @henryiii on 2023-02-10 16:29_

I don't think the location is that important, though I think the message should be there. That provides a description of sorts of what it did (removed imports, removed print statements, etc). Just the number only helps advanced users who'd memorized all the numbers or are willing to look each on up.

Something like:

```
my/file.py:
  F821 (x12): I don't actually remember what F821 is, other than very common
  A123 (x2): Some other fixable thing
```

Other than that, looks great!


---

_Comment by @charliermarsh on 2023-02-10 16:31_

What if we included the human-readable name alongside the code, like `unused-variable`, etc.?

---

_Comment by @ngnpope on 2023-02-10 16:44_

This is nice. Agreed that it probably doesn't make sense to give line/character breakdown. If you're removing lines, then there is no line to reference. And then it seems strange to have some with that information and some without.

Can I propose something like the following for the output?

```
Fixed 81 errors:
  crates/ruff/resources/test/fixtures/flake8_return/RET504.py:
     1 × E731 Do not assign a `lambda` expression, use a `def`
  crates/ruff/resources/test/fixtures/flake8_return/RET505.py:
    26 × F841 Local variable `…` is assigned to but never used
  crates/ruff/resources/test/fixtures/flake8_return/RET506.py:
    19 × F841 Local variable `…` is assigned to but never used
  crates/ruff/resources/test/fixtures/flake8_return/RET507.py:
    16 × F841 Local variable `…` is assigned to but never used
  crates/ruff/resources/test/fixtures/flake8_return/RET508.py:
    19 × F841 Local variable `…` is assigned to but never used
```

This makes it easy to parse visually as things align nicely. Some thoughts:

- Sort the outer list of files alphabetically - it's easier to scan through
- Sort the inner lists by decreasing number of fixes - it's nice to know where the biggest wins were
- Output the rule message to avoid having to look up rule codes
  - Note that rules with more than one message format should be grouped separately, e.g.

    ```
    100 × RUF100 Unused blanket `noqa` directive
     10 × RUF100 Unused `noqa` directive
      1 × RUF100 Unused `noqa` directive (…)
    ```
  - But don't repeat for all variations of a message, i.e. substitute `…` for all placeholders
- Right align the count of violations fixed under _all_ files by the number of digits for the biggest number
- And I love the use of nice unicode symbols... So note the `×` and `…` 😉 

---

_Comment by @charliermarsh on 2023-02-10 16:48_

I'm hesitant to include the message formats because they're not very clear in a lot of cases, and we haven't committed to maintaining variants that _would_ be clear in this context. I'd kind of prefer to use the human-readable rule name for that purpose, which we _are_ committed to maintaining.

---

_Comment by @ngnpope on 2023-02-10 16:48_

> What if we included the human-readable name alongside the code, like unused-variable, etc.?

I think this will happen anyway when we switch to focusing on human-friendly names as the primary code.

Also, we lose a bit of context when a rule has different messages.

---

_Comment by @ngnpope on 2023-02-10 16:48_

(Sorry - am crossing comments. Bad timing! 😅 )

---

_Comment by @ngnpope on 2023-02-10 16:49_

> Also, we lose a bit of context when a rule has different messages.

Although maybe this is an indication that a rule should be split up into multiple rules? 🤔 

---

_Comment by @charliermarsh on 2023-02-11 04:18_

(This'll go out in the next release, not tonight's.)

---

_Merged by @charliermarsh on 2023-02-12 20:48_

---

_Closed by @charliermarsh on 2023-02-12 20:48_

---

_Branch deleted on 2023-02-12 20:48_

---

_Renamed from "Show applied fixes by file and diagnostic" to "Add `--show-fixes` flag to show applied fixes" by @charliermarsh on 2023-02-13 02:29_

---
