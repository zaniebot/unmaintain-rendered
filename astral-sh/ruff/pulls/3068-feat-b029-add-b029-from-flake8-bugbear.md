```yaml
number: 3068
title: "feat(B029): Add B029 from flake8-bugbear"
type: pull_request
state: merged
author: carlosmiei
labels:
  - rule
assignees: []
merged: true
base: main
head: b029
created_at: 2023-02-20T20:16:03Z
updated_at: 2023-02-21T09:52:48Z
url: https://github.com/astral-sh/ruff/pull/3068
synced_at: 2026-01-12T04:39:44Z
```

# feat(B029): Add B029 from flake8-bugbear

---

_Pull request opened by @carlosmiei on 2023-02-20 20:16_

- Added B029 from flake8-bugbear
![image](https://user-images.githubusercontent.com/43336371/220191336-82cb464e-382a-4d88-9195-c581c79d38e6.png)


- I think I followed all the required steps, but let me know if I missed something

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_bugbear/B029.py`:8 on 2023-02-20 20:20_

Can we replace this with the version from [flake8-bugbear](https://github.com/PyCQA/flake8-bugbear/blob/main/tests/b029.py) itself?

```py
"""
Should emit:
B029 - on lines 8 and 13
"""

try:
    pass
except ():
    pass

try:
    pass
except () as e:
    pass
```

---

_@charliermarsh reviewed on 2023-02-20 20:20_

---

_Comment by @charliermarsh on 2023-02-20 20:20_

Looking good!

---

_Label `rule` added by @charliermarsh on 2023-02-20 20:20_

---

_Review comment by @carlosmiei on `crates/ruff/resources/test/fixtures/flake8_bugbear/B029.py`:8 on 2023-02-20 20:21_

@charliermarsh sure! 

---

_@carlosmiei reviewed on 2023-02-20 20:21_

---

_@charliermarsh reviewed on 2023-02-20 20:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B029_B029.py.snap`:25 on 2023-02-20 20:30_

Do you have `insta` installed? (`cargo install cargo-insta`) You should be able to run `cargo test --lib`, then `cargo insta review` to review and check in the generated snapshot.

---

_@carlosmiei reviewed on 2023-02-20 20:31_

---

_Review comment by @carlosmiei on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B029_B029.py.snap`:25 on 2023-02-20 20:31_

@charliermarsh  yeah it's weird I did run `cargo test --all` and `cargo insta review` but the file was not updated 🤔 

---

_@carlosmiei reviewed on 2023-02-20 20:33_

---

_Review comment by @carlosmiei on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B029_B029.py.snap`:25 on 2023-02-20 20:33_

(result from running `test --all` and `cargo insta review` 
![image](https://user-images.githubusercontent.com/43336371/220194253-fa1c3572-f031-438a-a89b-cf8f29e2e9ee.png)


---

_@charliermarsh reviewed on 2023-02-20 20:44_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B029_B029.py.snap`:25 on 2023-02-20 20:44_

Oh, you need to add the test to `crates/ruff/src/rules/flake8_bugbear/mod.rs` (hopefully the pattern is clear once you open up that file).

---

_Review comment by @carlosmiei on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B029_B029.py.snap`:25 on 2023-02-20 20:47_

@charliermarsh nvm 🤦 I accidentally deleted it upon resolving the merge conflict here ![image](https://user-images.githubusercontent.com/43336371/220196193-0375b715-b36d-4092-8fa0-8fc2231b75b3.png)

will restore it asap

---

_@carlosmiei reviewed on 2023-02-20 20:47_

---

_@charliermarsh reviewed on 2023-02-20 20:54_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B029_B029.py.snap`:25 on 2023-02-20 20:54_

Happens to the best of us :D

---

_Merged by @charliermarsh on 2023-02-20 20:57_

---

_Closed by @charliermarsh on 2023-02-20 20:57_

---

_Comment by @charliermarsh on 2023-02-20 20:57_

Thanks!

---

_Branch deleted on 2023-02-21 09:52_

---
