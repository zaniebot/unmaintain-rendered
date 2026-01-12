```yaml
number: 8630
title: Add new rule to check for useless quote escapes
type: pull_request
state: merged
author: ThiefMaster
labels:
  - rule
assignees: []
merged: true
base: main
head: quotes-remove-backslash
created_at: 2023-11-12T16:48:05Z
updated_at: 2023-11-13T22:16:57Z
url: https://github.com/astral-sh/ruff/pull/8630
synced_at: 2026-01-10T23:40:55Z
```

# Add new rule to check for useless quote escapes

---

_Pull request opened by @ThiefMaster on 2023-11-12 16:48_

When using the autofixer for `Q000` it does not remove the backslashes from quotes that no longer need escaping.
This new rule checks for such backslashes (regardless whether they come from the autofixer or not) and can remove them.

I used `Q100` to avoid ocnflicts with `Q00x` rules `flake8-quotes` might add (even though it's not very actively maintained)

fixes #8617

---

_Review comment by @ThiefMaster on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:81 on 2023-11-12 16:48_

If you can some up with a better text please change it. As usual, naming is hard...

---

_Review comment by @ThiefMaster on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:394 on 2023-11-12 16:50_

Is there a nicer way to pass this list of edits without having to manually consume the first item and pass it separately?

Not sure if Rust has something like Python's `*args` that could be used in `Fix::safe_exits(...)`...

---

_@ThiefMaster reviewed on 2023-11-12 16:50_

---

_@ThiefMaster reviewed on 2023-11-12 17:12_

---

_Review comment by @ThiefMaster on `crates/ruff_linter/src/codes.rs`:406 on 2023-11-12 17:12_

Should this rule go into `RuleGroup::Preview`? One on hand it's a new rule, but on the other hand its purpose is fixing a bug in another rule...

---

_Comment by @github-actions[bot] on 2023-11-12 17:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/tests/test_ext_inheritance_diagram.py#L219'>tests/test_ext_inheritance_diagram.py:219:16:</a> Q004 [*] Unnecessary escape on inner quote character
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| Q004 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2023-11-12 17:44_

Haven't reviewed the code yet, but the ecosystem check seems to be flagging some cases like `"\\'"`, in which the backslash escapes _are_ necessary (I think?) to add a backslash literal.

---

_Comment by @ThiefMaster on 2023-11-12 17:45_

Yeah, just noticed that I'm not checking for escaped backslashes - already on it

---

_Comment by @charliermarsh on 2023-11-12 17:48_

Awesome, thanks @ThiefMaster!

---

_@charliermarsh reviewed on 2023-11-12 17:49_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:406 on 2023-11-12 17:49_

Yeah, it does need to go in preview -- as part of our [versioning policy](https://docs.astral.sh/ruff/versioning/), we only add new stable rules in minor (not patch) releases.

---

_@charliermarsh reviewed on 2023-11-12 17:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:406 on 2023-11-12 17:50_

I know it's a bit awkward, but basically, if we implemented this as an improvement in the `Q001` fix, it would be fine to ship as part of stable. But if we're adding a new rule, which can raise new violations, we want to avoid promoting it outside of a minor release.

---

_@ThiefMaster reviewed on 2023-11-12 19:11_

---

_Review comment by @ThiefMaster on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:404 on 2023-11-12 19:11_

This is awfully similar to `unescape_string`. Probably we could literally call this and check if the strings are identical, instead of having the same logic twice...

The performance here is probably slightly better since it avoids building a new string and bails out as soon as it found an escaped quote, but I'd be surprised if there was any noticeable difference even on a large project...

---

_Comment by @ThiefMaster on 2023-11-12 19:36_

Ecosystems looks much better now. The one new violation is actually correct since those quotes there should *not* be escaped.

---

_Comment by @ThiefMaster on 2023-11-13 13:48_

The mkdocs CI failure seems unrelated (it didn't fail until I rebased to the latest master).

---

_@ThiefMaster reviewed on 2023-11-13 21:32_

---

_Review comment by @ThiefMaster on `crates/ruff_linter/resources/test/fixtures/flake8_quotes/doubles_escaped_unnecessary.py`:1 on 2023-11-13 21:32_

@charliermarsh you may want to rename the `Q100` references to `Q004` in these two files as well ;)

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_quotes/doubles_escaped_unnecessary.py`:1 on 2023-11-13 21:38_

Thank you!

---

_@charliermarsh reviewed on 2023-11-13 21:38_

---

_@charliermarsh reviewed on 2023-11-13 21:38_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`:394 on 2023-11-13 21:38_

I did it slightly more manually, but in a way that combines it with the validation condition above (to ensure that we _must_ have at least one edit, and avoid runtime panics).

---

_Label `rule` added by @charliermarsh on 2023-11-13 21:39_

---

_@charliermarsh approved on 2023-11-13 21:42_

---

_@charliermarsh approved on 2023-11-13 21:43_

---

_Merged by @charliermarsh on 2023-11-13 21:59_

---

_Closed by @charliermarsh on 2023-11-13 21:59_

---

_Branch deleted on 2023-11-13 22:02_

---
