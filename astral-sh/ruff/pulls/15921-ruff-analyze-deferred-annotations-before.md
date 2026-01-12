```yaml
number: 15921
title: "[`ruff`] Analyze deferred annotations before enforcing `mutable-(data)class-default` and `function-call-in-dataclass-default-argument` (`RUF008`,`RUF009`,`RUF012`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: classvar
created_at: 2025-02-04T01:46:06Z
updated_at: 2025-02-05T12:47:08Z
url: https://github.com/astral-sh/ruff/pull/15921
synced_at: 2026-01-12T15:55:53Z
```

# [`ruff`] Analyze deferred annotations before enforcing `mutable-(data)class-default` and `function-call-in-dataclass-default-argument` (`RUF008`,`RUF009`,`RUF012`)

---

_@dylwil3_

This PR moves the rules [`mutable-dataclass-default` (`RUF008`)](https://docs.astral.sh/ruff/rules/mutable-dataclass-default/#mutable-dataclass-default-ruf008), [function-call-in-dataclass-default-argument (RUF009)](https://docs.astral.sh/ruff/rules/function-call-in-dataclass-default-argument/#function-call-in-dataclass-default-argument-ruf009), and [`mutable-class-default` (`RUF012`)](https://docs.astral.sh/ruff/rules/mutable-class-default/#mutable-class-default-ruf012)   to the checker for deferred scopes, which avoids false positives caused by a failure to account for deferred resolution of annotations. 

This PR does not address the issue of stringized annotations, which are also deferred but require a different approach.

Closes #15857

-----------

Note: As a consequence of this change, no lint for `RUF012` will be emitted in the following example with a `NameError`:

```python
class A:
   mutable_default : ClassVar[list[int]] = []

from typing import ClassVar
```

But that's okay - it's not the job of `RUF012` or `RUF008` to detect this.

---

_Label `bug` added by @dylwil3 on 2025-02-04 01:46_

---

_Label `rule` added by @dylwil3 on 2025-02-04 01:46_

---

_Comment by @github-actions[bot] on 2025-02-04 01:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/commands/dashboard/importers/v1/__init__.py#L47'>superset/commands/dashboard/importers/v1/__init__.py:47:5:</a> D400 First line should end with a period
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/commands/dashboard/importers/v1/__init__.py#L47'>superset/commands/dashboard/importers/v1/__init__.py:47:5:</a> D400 First line should end with a period
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L992'>superset/models/helpers.py:992:13:</a> D401 First line of docstring should be in imperative mood: "Some engines change the case or generate bespoke column names, either by"
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L992'>superset/models/helpers.py:992:13:</a> D401 First line of docstring should be in imperative mood: "Some engines change the case or generate bespoke column names, either by"
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L992'>superset/models/helpers.py:992:13:</a> D415 First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L992'>superset/models/helpers.py:992:13:</a> D415 First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/security_tests.py#L2023'>tests/integration_tests/security_tests.py:2023:9:</a> ANN201 Missing return type annotation for public function `test_get_guest_user_with_request_form`
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/security_tests.py#L2023'>tests/integration_tests/security_tests.py:2023:9:</a> ANN201 Missing return type annotation for public function `test_get_guest_user_with_request_form`
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/sql_validator_tests.py#L102'>tests/integration_tests/sql_validator_tests.py:102:9:</a> ANN201 Missing return type annotation for public function `test_valid_syntax`
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/sql_validator_tests.py#L102'>tests/integration_tests/sql_validator_tests.py:102:9:</a> ANN201 Missing return type annotation for public function `test_valid_syntax`
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN201 | 4 | 2 | 2 | 0 | 0 |
| D400 | 2 | 1 | 1 | 0 | 0 |
| D401 | 2 | 1 | 1 | 0 | 0 |
| D415 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -6 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/commands/chart/importers/v1/__init__.py#L1'>superset/commands/chart/importers/v1/__init__.py:1:1:</a> D104 Missing docstring in public package
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/commands/chart/importers/v1/__init__.py#L1'>superset/commands/chart/importers/v1/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/connectors/sqla/models.py#L789'>superset/connectors/sqla/models.py:789:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/connectors/sqla/models.py#L789'>superset/connectors/sqla/models.py:789:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L626'>superset/models/helpers.py:626:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L626'>superset/models/helpers.py:626:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L818'>superset/models/helpers.py:818:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/superset/models/helpers.py#L818'>superset/models/helpers.py:818:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/security/row_level_security_tests.py#L170'>tests/integration_tests/security/row_level_security_tests.py:170:9:</a> ANN202 Missing return type annotation for private function `_get_test_dataset`
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/security/row_level_security_tests.py#L170'>tests/integration_tests/security/row_level_security_tests.py:170:9:</a> ANN202 Missing return type annotation for private function `_get_test_dataset`
- <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/sql_validator_tests.py#L115'>tests/integration_tests/sql_validator_tests.py:115:9:</a> PLR6301 Method `test_invalid_syntax` could be a function, class method, or static method
+ <a href='https://github.com/apache/superset/blob/c64018d42135f36e81d0f630176880572e281dc3/tests/integration_tests/sql_validator_tests.py#L115'>tests/integration_tests/sql_validator_tests.py:115:9:</a> PLR6301 Method `test_invalid_syntax` could be a function, class method, or static method
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D102 | 6 | 3 | 3 | 0 | 0 |
| D104 | 2 | 1 | 1 | 0 | 0 |
| ANN202 | 2 | 1 | 1 | 0 | 0 |
| PLR6301 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF012_RUF012.py.snap`:77 on 2025-02-04 08:44_

Hmm, why are we emitting an error here? It looks like it already is annotated with ClassVar?

---

_@AlexWaygood approved on 2025-02-04 08:45_

Looks good, just one question 

---

_@MichaReiser reviewed on 2025-02-04 08:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/mutable_class_default.rs`:82 on 2025-02-04 08:58_

Unrelated to this PR: It's sort of annoying that you had to change the method signature just because you can only get a read-only `Checker` here. I think we should start refactoring `Checker` so that:

* Add a `Checker::push_diagnostic` or `report_diagnostic` method 
* Maybe: Add a `Checker::extend_diagnostics` or `report_diagnostics` method
* Then, change `Checker::diagnostics` to a `RefCell<Vec<Diagnostic>>`

Doing so has a few advantages:

* It is no longer required to take a `&mut Checker` only to push diagnostics
* We have a central place to perform some operation on diagnostics. E.g. we could adopt Red Knot's approach to filter out suppressed diagnostics when they're emitted instead of filtering them out at the very end. 

Obviously, this isn't something for this PR but maybe a fun refactor for another day ;)

---

_@Daverball reviewed on 2025-02-04 15:11_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/ruff/rules/mutable_class_default.rs`:82 on 2025-02-04 15:11_

Yes, please. Every single time I've run into lifetime issues or borrow checker complaints, it was because of a mutable borrow of `Checker`. This also would allow us to pass the checker directly into some of the helper functions, instead of having to pass five separate arguments that are all just borrowed from the checker.

---

_@dylwil3 reviewed on 2025-02-04 22:29_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF012_RUF012.py.snap`:77 on 2025-02-04 22:29_

Somehow I forgot to actually check that stringized annotations were working... and they aren't!

It turns out that fixing that is gonna require other changes (and I noticed a few other affected rules). So I'm gonna leave that as a followup PR and just solve the problem in the linked issue for this PR. I did move one other related rule to `deferred_scopes`.

---

_@dylwil3 reviewed on 2025-02-04 22:30_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/mutable_class_default.rs`:82 on 2025-02-04 22:30_

Sounds like fun I'll give it a shot!

---

_Renamed from "[`ruff`] Analyze deferred annotations before enforcing `mutable-(data)class-default` (`RUF008`,`RUF012`)" to "[`ruff`] Analyze deferred annotations before enforcing `mutable-(data)class-default` and `function-call-in-dataclass-default-argument` (`RUF008`,`RUF009`,`RUF012`)" by @dylwil3 on 2025-02-04 22:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF012_RUF012.py.snap`:77 on 2025-02-05 12:33_

Ahh, and I see this isn't a regression; we also have this false negative on `main`. Feel free to merge, in that case!

Probably a lot more rules should be using this helper function when they inspect annotations: https://github.com/astral-sh/ruff/blob/eb08345fd5434ed3db243233df6dd91757a6af3b/crates/ruff_linter/src/checkers/ast/mod.rs#L453-L471

(I introduced the helper in #12951 ;)

---

_@AlexWaygood reviewed on 2025-02-05 12:33_

---

_Merged by @dylwil3 on 2025-02-05 12:44_

---

_Closed by @dylwil3 on 2025-02-05 12:44_

---

_@dylwil3 reviewed on 2025-02-05 12:47_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF012_RUF012.py.snap`:77 on 2025-02-05 12:47_

Yes exactly! Originally I started to use that helpful helper, but then noticed the number of places I had to put it was sort of exploding. So I wanted to take a step back in a separate PR and either see if there's a better way or if that failed then at least separate that change from the one in this PR.

---
