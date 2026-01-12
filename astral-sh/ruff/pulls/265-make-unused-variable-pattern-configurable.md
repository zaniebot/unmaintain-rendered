```yaml
number: 265
title: Make unused variable pattern configurable
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: dummy_variable_rgx
created_at: 2022-09-24T04:37:07Z
updated_at: 2022-09-24T14:48:25Z
url: https://github.com/astral-sh/ruff/pull/265
synced_at: 2026-01-12T15:55:04Z
```

# Make unused variable pattern configurable

---

_@harupy_

Resolve #134.

---

_Review comment by @harupy on `src/settings.rs`:109 on 2022-09-24 05:11_

pylint uses the following default value:

```
_+$|(_[a-zA-Z0-9_]*[a-zA-Z0-9]+?$)|dummy|^ignored_|^unused_
```

https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#dummy-variables-rgx

---

_@harupy reviewed on 2022-09-24 05:11_

---

_@harupy reviewed on 2022-09-24 05:36_

---

_Review comment by @harupy on `src/settings.rs`:110 on 2022-09-24 05:36_

```suggestion
    Lazy::new(|| Regex::new("^_$").unwrap());
```

Should we align with flake8?

---

_Review comment by @harupy on `resources/test/fixtures/F841_dummy_variable_rgx.py`:10 on 2022-09-24 05:36_

Flake8 result:

```
> flake8 resources/test/fixtures/F841_dummy_variable_rgx.py
resources/test/fixtures/F841_dummy_variable_rgx.py:2:5: F841 local variable 'x' is assigned to but never used
resources/test/fixtures/F841_dummy_variable_rgx.py:4:5: F841 local variable '__' is assigned to but never used
resources/test/fixtures/F841_dummy_variable_rgx.py:5:5: F841 local variable '_x' is assigned to but never used
resources/test/fixtures/F841_dummy_variable_rgx.py:6:5: F841 local variable '__x' is assigned to but never used
resources/test/fixtures/F841_dummy_variable_rgx.py:7:5: F841 local variable 'x_' is assigned to but never used
resources/test/fixtures/F841_dummy_variable_rgx.py:8:5: F841 local variable 'x__' is assigned to but never used
resources/test/fixtures/F841_dummy_variable_rgx.py:9:5: F841 local variable '__x__' is assigned to but never used
resources/test/fixtures/F841_dummy_variable_rgx.py:10:5: F841 local variable '_x_y_z' is assigned to but never used
```

---

_@harupy reviewed on 2022-09-24 05:37_

---

_@charliermarsh reviewed on 2022-09-24 14:23_

---

_Review comment by @charliermarsh on `src/settings.rs`:110 on 2022-09-24 14:23_

Should we make this `_+$|(_[a-zA-Z0-9_]*[a-zA-Z0-9]+?$)|dummy|^ignored_|^unused_` to match Pylint?

---

_Comment by @charliermarsh on 2022-09-24 14:24_

Looks great!

---

_@charliermarsh reviewed on 2022-09-24 14:24_

---

_Review comment by @charliermarsh on `src/settings.rs`:110 on 2022-09-24 14:24_

(Will make this change then merge.)

---

_@harupy reviewed on 2022-09-24 14:35_

---

_Review comment by @harupy on `src/settings.rs`:110 on 2022-09-24 14:35_

Yes, that sounds good to me :)

---

_@charliermarsh reviewed on 2022-09-24 14:43_

---

_Review comment by @charliermarsh on `src/settings.rs`:110 on 2022-09-24 14:43_

(A went for something in-between -- I think `dummy` and `ignored` and `unused` are a bit odd as defaults.)

---

_Merged by @charliermarsh on 2022-09-24 14:43_

---

_Closed by @charliermarsh on 2022-09-24 14:43_

---

_@harupy reviewed on 2022-09-24 14:48_

---

_Review comment by @harupy on `src/settings.rs`:110 on 2022-09-24 14:48_

Makes sense!

---
