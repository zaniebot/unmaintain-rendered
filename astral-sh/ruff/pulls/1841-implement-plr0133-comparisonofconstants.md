```yaml
number: 1841
title: "Implement `PLR0133` (`ComparisonOfConstants`)"
type: pull_request
state: merged
author: max0x53
labels: []
assignees: []
merged: true
base: main
head: plr0133
created_at: 2023-01-13T01:19:07Z
updated_at: 2023-01-13T19:23:52Z
url: https://github.com/astral-sh/ruff/pull/1841
synced_at: 2026-01-12T05:36:32Z
```

# Implement `PLR0133` (`ComparisonOfConstants`)

---

_Pull request opened by @max0x53 on 2023-01-13 01:19_

This PR adds [Pylint `R0133`](https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/comparison-of-constants.html)

Feel free to suggest changes and additions, I have tried to maintain parity with the Pylint implementation [`comparison_checker.py`](https://github.com/PyCQA/pylint/blob/main/pylint/checkers/base/comparison_checker.py#L247)

`pylint` output:

```bash
$ pylint  ../ruff/resources/test/fixtures/pylint/constant_comparison.py | grep R0133
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:3:3: R0133: Comparison between constants: '100 == 100' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:6:3: R0133: Comparison between constants: '1 == 3' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:9:3: R0133: Comparison between constants: '1 != 3' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:13:3: R0133: Comparison between constants: '4 == 3' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:23:3: R0133: Comparison between constants: '1 > 0' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:29:3: R0133: Comparison between constants: '1 >= 0' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:35:3: R0133: Comparison between constants: '1 < 0' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:41:3: R0133: Comparison between constants: '1 <= 0' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:51:3: R0133: Comparison between constants: 'hello == ' has a constant value (comparison-of-constants)
<PATH>/ruff/resources/test/fixtures/pylint/constant_comparison.py:58:3: R0133: Comparison between constants: 'True == False' has a constant value (comparison-of-constants)
```

`ruff` output:

```bash
resources/test/fixtures/pylint/constant_comparison.py:3:4: PLR0133 Two constants compared in a comparison, consider replacing `100 == 100`
resources/test/fixtures/pylint/constant_comparison.py:6:4: PLR0133 Two constants compared in a comparison, consider replacing `1 == 3`
resources/test/fixtures/pylint/constant_comparison.py:9:4: PLR0133 Two constants compared in a comparison, consider replacing `1 != 3`
resources/test/fixtures/pylint/constant_comparison.py:13:4: PLR0133 Two constants compared in a comparison, consider replacing `4 == 3`
resources/test/fixtures/pylint/constant_comparison.py:23:4: PLR0133 Two constants compared in a comparison, consider replacing `1 > 0`
resources/test/fixtures/pylint/constant_comparison.py:29:4: PLR0133 Two constants compared in a comparison, consider replacing `1 >= 0`
resources/test/fixtures/pylint/constant_comparison.py:35:4: PLR0133 Two constants compared in a comparison, consider replacing `1 < 0`
resources/test/fixtures/pylint/constant_comparison.py:41:4: PLR0133 Two constants compared in a comparison, consider replacing `1 <= 0`
resources/test/fixtures/pylint/constant_comparison.py:51:4: PLR0133 Two constants compared in a comparison, consider replacing `'hello' == ''`
resources/test/fixtures/pylint/constant_comparison.py:58:4: PLR0133 Two constants compared in a comparison, consider replacing `True == False`
```

I have separated the change into 2 commits, the first commit combines the EqComp and IsComp wrappers within `violations.rs` into a single wrapper. Otherwise I would have to have had added a 3rd wrapper to handle this case which is in the 2nd commit.

See #970 

---

_Review comment by @not-my-profile on `src/violations.rs`:823 on 2023-01-13 06:24_

I think you should rather implement a 3rd wrapper. I think merging enums like this is a bad idea because it looses compile-time safety, as evidenced by the `unreachable!()` statements that you had to add. Potential runtime panics are bad and it's better if the compiler can check that the match blocks cover all cases.

Furthermore we want to split up this file in the future (moving the struct definitions to the respective rule files) which also is easier if we don't artificially merge unrelated enums.

---

_@not-my-profile reviewed on 2023-01-13 06:24_

---

_@max0x53 reviewed on 2023-01-13 10:24_

---

_Review comment by @max0x53 on `src/violations.rs`:823 on 2023-01-13 10:24_

Updated to be a 3rd wrapper, I was coming at the problem from the point of view of side stepping the `Serialize + Deserialize + Eq` requirements that are not found on `Cmpop`. However, as you point out the wrappers are also used for type safety in other cases.

---

_@not-my-profile reviewed on 2023-01-13 11:02_

---

_Review comment by @not-my-profile on `src/violations.rs`:1210 on 2023-01-13 11:02_

I know that most violation structs currently use tuple structs but we actually want to change all these to field structs in the future whenever the types don't clearly identify the purpose of the field. So in this case I think the following would be preferable:

```rs
pub struct ConstantComparison {
    pub left_const: String,
    pub op: ViolationsCmpop,
    pub right_const: String,
}
```

---

_Merged by @charliermarsh on 2023-01-13 17:14_

---

_Closed by @charliermarsh on 2023-01-13 17:14_

---

_Review comment by @charliermarsh on `src/violations.rs`:1226 on 2023-01-13 17:17_

If you're interested, this is autofixable. You could look at `impl AlwaysAutofixableViolation for IsLiteral` for an example.

---

_@charliermarsh reviewed on 2023-01-13 17:17_

---

_Branch deleted on 2023-01-13 19:23_

---
