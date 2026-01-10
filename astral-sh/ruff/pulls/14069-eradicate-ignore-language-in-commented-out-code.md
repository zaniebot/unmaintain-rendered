```yaml
number: 14069
title: "[`eradicate`] ignore `# language=` in commented-out-code rule (ERA001)"
type: pull_request
state: merged
author: fabiob
labels:
  - bug
assignees: []
merged: true
base: main
head: adds-language-to-era001
created_at: 2024-11-03T21:23:21Z
updated_at: 2024-11-04T16:33:59Z
url: https://github.com/astral-sh/ruff/pull/14069
synced_at: 2026-01-10T20:50:57Z
```

# [`eradicate`] ignore `# language=` in commented-out-code rule (ERA001)

---

_Pull request opened by @fabiob on 2024-11-03 21:23_

Fixes one common case cited on #6019

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `commented-out-code` rule (ERA001) from `eradicate` is currently flagging a very common idiom that marks Python strings as another language, to help with syntax highlighting:

![image](https://github.com/user-attachments/assets/d523e83d-95cb-4668-a793-45f01d162234)

This PR adds this idiom to the list of allowed exceptions to the rule.

## Test Plan

I've added some additional test cases.


---

_@charliermarsh approved on 2024-11-03 21:24_

---

_Comment by @charliermarsh on 2024-11-03 21:24_

Do you mind pushing the test cases? I only see the code changes.

For reference: https://www.jetbrains.com/help/pycharm/using-language-injections.html

---

_Label `bug` added by @charliermarsh on 2024-11-03 21:25_

---

_Comment by @fabiob on 2024-11-03 21:26_

Sorry @charliermarsh , forgot to force-push after I amended the commit with the test cases.

---

_Comment by @charliermarsh on 2024-11-03 21:26_

No prob, thanks for the PR.

---

_Comment by @fabiob on 2024-11-03 21:26_

And looks like I need to fix the formatting... Working on it.

---

_Comment by @fabiob on 2024-11-03 21:36_

> For reference: https://www.jetbrains.com/help/pycharm/using-language-injections.html

I've added some more test cases and fixed the regexp with the additional cases described here.

---

_Merged by @charliermarsh on 2024-11-03 21:50_

---

_Closed by @charliermarsh on 2024-11-03 21:50_

---

_Comment by @github-actions[bot] on 2024-11-03 21:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-11-03 22:10_

Thanks and welcome to the project.

---

_Branch deleted on 2024-11-03 22:12_

---

_@InSyncWithFoo reviewed on 2024-11-04 14:32_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 14:32_

This nested quantifier doesn't look so nice: `[a-zA-Z](?: ?[-_.a-zA-Z0-9]+)+`. The optional space at the start of the group leads to multiple ways of matching the same text.

I know Rust's regex engine guarantees linear time, but this is nevertheless a bad pattern. Prefer `[a-zA-Z][-_.a-zA-Z0-9]*(?: [-_.a-zA-Z0-9]+)*` instead.

Technically, language IDs [can be anything](https://github.com/JetBrains/intellij-community/blob/986fff1b53ed8005bffae54a652d85c8bf901065/platform/core-api/src/com/intellij/lang/Language.java#L62), so it's perhaps better to drop the leading character requirement: `[-_.a-zA-Z0-9]+(?: [-_.a-zA-Z0-9]+)*`.

---

_@charliermarsh reviewed on 2024-11-04 15:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 15:10_

Should we just use `language=`? All language identifiers will start with that, and false positives seem somewhat rare / not a huge deal here.

---

_@fabiob reviewed on 2024-11-04 15:21_

---

_Review comment by @fabiob on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 15:21_

@InSyncWithFoo maybe I was extra cautious about false positives.

> Technically, language IDs [can be anything](https://github.com/JetBrains/intellij-community/blob/986fff1b53ed8005bffae54a652d85c8bf901065/platform/core-api/src/com/intellij/lang/Language.java#L62),

Good catch! Maybe we can just require a non-space character after the equals sign. This should take care of most false positives anyway, as it's uncommon for Python users to omit spaces around assignment operators.

The only thing that worries me is about detecting a commented-out named parameter on a multi-line method invocation or declaration. But maybe I'm just being overzealous.

```python
def check_spelling(
  text,
  # language=DEFAULT_LANGUAGE
) ...
```

---

_@InSyncWithFoo reviewed on 2024-11-04 15:25_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 15:25_

[A quick search](https://github.com/search?q=%2F%28%3F%3A%23%29%5B%5E%5Cn%5CS%5D*language%3D%2F+path%3A%22.py%22&type=code) shows that matching on `language=` alone would lead to many false negatives.

Language IDs are also displayed to the user as part of the UI, so I doubt they would contain non-ASCII characters. I would say limiting to `[-_.a-zA-Z0-9]` and spaces is the most balanced heuristic.

---

_@InSyncWithFoo reviewed on 2024-11-04 15:38_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 15:38_

> [...] it's uncommon for Python users to omit spaces around assignment operators.

Don't forget keyword arguments, which are recommended to be written with no spaces around the equal sign:

```python
# frobnicate(
#     language='en'  # Could also be `language=EN` with a predefined constant `EN`
# )
```

I think both this and the false negative you mention are well within the acceptable error margin.

---

_@fabiob reviewed on 2024-11-04 15:55_

---

_Review comment by @fabiob on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 15:55_

So @InSyncWithFoo , will you write the PR with the new regexp and test cases, or should I?

---

_@InSyncWithFoo reviewed on 2024-11-04 16:00_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 16:00_

I'll handle it.

---

_@InSyncWithFoo reviewed on 2024-11-04 16:33_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/eradicate/detection.rs`:19 on 2024-11-04 16:33_

Turns out language IDs in comments must be normalized.

This is JSON Lines (one value on each line, comments allowed):

![](https://github.com/user-attachments/assets/84a2f648-7edc-4075-9aa3-b8ce2c5b4977)

This is pure JSON (one single top-level value, comments disallowed):

![](https://github.com/user-attachments/assets/8e651588-37f9-46ca-9681-9c1eb430eb06)

`jsonlines` must be used even though autocompletion popups use `json lines`:

![](https://github.com/user-attachments/assets/15f9e26c-64dd-4c02-bc27-a274c6ecea5a)

Notably, this rule applies to some languages but not others (though Ruff doesn't need to care about this):

![](https://github.com/user-attachments/assets/10f475d7-3ad3-4692-a41a-97e193d104ee)




---
