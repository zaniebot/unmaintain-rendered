```yaml
number: 2445
title: "Implement pylint's `too-many-statements` rule (`PLR0915`)"
type: pull_request
state: merged
author: chanman3388
labels:
  - rule
assignees: []
merged: true
base: main
head: PLR0915
created_at: 2023-02-01T17:07:16Z
updated_at: 2023-02-02T13:18:42Z
url: https://github.com/astral-sh/ruff/pull/2445
synced_at: 2026-01-12T15:55:08Z
```

# Implement pylint's `too-many-statements` rule (`PLR0915`)

---

_@chanman3388_

https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/too-many-statements.html

A bit of a quirky one, I made a bunch of test cases in python to see how pylint would work (tested on 2.9.3) to derive the expected behaviour.

Issue #970 

---

_Comment by @chanman3388 on 2023-02-01 17:21_

I'm unsure how to fix the unittest as it passes for me locally:
```
➜  ruff git:(PLR0915) cargo test rules::pylint::tests::max_statements                    
    Finished test [unoptimized + debuginfo] target(s) in 0.84s
     Running unittests src/lib.rs (target/debug/deps/ruff-6394b47557a9e4c6)

running 1 test
test rules::pylint::tests::max_statements ... ok
```

---

_Converted to draft by @chanman3388 on 2023-02-01 17:34_

---

_Comment by @charliermarsh on 2023-02-01 17:35_

Looks like it's failing due to ` No such file or directory`. Let me see...

---

_@charliermarsh reviewed on 2023-02-01 17:58_

---

_Review comment by @charliermarsh on `src/rules/pylint/settings.rs`:59 on 2023-02-01 17:58_

Nit: in the example, use `max-statements`, since we use that casing in the TOML.

---

_Review comment by @chanman3388 on `src/rules/pylint/settings.rs`:59 on 2023-02-01 17:58_

Thank you

---

_@chanman3388 reviewed on 2023-02-01 17:58_

---

_Comment by @charliermarsh on 2023-02-01 18:00_

Can't for the life of me figure out that failure. Going to try downgrading `insta` I guess?

---

_Comment by @chanman3388 on 2023-02-01 18:07_

> Can't for the life of me figure out that failure. Going to try downgrading `insta` I guess?

I think I figured it out, I put the wrong path inside of `src/rules/pylint/mod.rs`

---

_Marked ready for review by @chanman3388 on 2023-02-01 18:20_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/too_many_statements.rs`:79 on 2023-02-02 02:32_

Nit: extra print here.

---

_@charliermarsh reviewed on 2023-02-02 02:32_

---

_@charliermarsh reviewed on 2023-02-02 02:32_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/too_many_statements.rs`:133 on 2023-02-02 02:32_

Thank you for all the tests.

---

_@charliermarsh reviewed on 2023-02-02 02:33_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/too_many_statements.rs`:76 on 2023-02-02 02:33_

Did you review the Pylint source when writing this? Or was it all inferred from the tests?

---

_@chanman3388 reviewed on 2023-02-02 04:55_

---

_Review comment by @chanman3388 on `src/rules/pylint/rules/too_many_statements.rs`:76 on 2023-02-02 04:55_

I made a load of examples by hand and ran pylint on them to see what was going on. In each of the unittests, I added a comment on the function def line which indicates the number of statements as reported by pylint 2.9.3. Some of the behaviour seems odd and I was considering raising some issues against pylint.

---

_@chanman3388 reviewed on 2023-02-02 05:22_

---

_Review comment by @chanman3388 on `src/rules/pylint/rules/too_many_statements.rs`:76 on 2023-02-02 05:22_

Hm, the architecture for pylint is slightly different, however I did find this:
```
def visit_tryfinally(self, node: nodes.TryFinally) -> None:
    """Increments the branches counter."""
    self._inc_branch(node, 2)
    self._inc_all_stmts(2)
```

---

_Merged by @charliermarsh on 2023-02-02 13:18_

---

_Closed by @charliermarsh on 2023-02-02 13:18_

---

_Label `rule` added by @charliermarsh on 2023-02-02 13:18_

---
