```yaml
number: 587
title: Implement B015
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: B015
created_at: 2022-11-04T16:25:09Z
updated_at: 2022-11-06T00:20:41Z
url: https://github.com/astral-sh/ruff/pull/587
synced_at: 2026-01-12T05:48:45Z
```

# Implement B015

---

_Pull request opened by @harupy on 2022-11-04 16:25_

#389 

---

_@harupy reviewed on 2022-11-04 16:29_

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__B015_B015.py.snap`:8 on 2022-11-04 16:29_

Weird. I think `column` should be 0.

---

_@charliermarsh reviewed on 2022-11-04 16:44_

---

_Review comment by @charliermarsh on `src/snapshots/ruff__linter__tests__B015_B015.py.snap`:8 on 2022-11-04 16:44_

I think this is the location of the `==`?

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__B015_B015.py.snap`:8 on 2022-11-04 16:47_

Yes.

```
 1 == 1
   ^ actual location
 ^ expected location
```

---

_@harupy reviewed on 2022-11-04 16:47_

---

_@harupy reviewed on 2022-11-04 16:53_

---

_Review comment by @harupy on `src/check_ast.rs`:1332 on 2022-11-04 16:53_

```suggestion
                        println!("{:?}", expr);
                        flake8_bugbear::plugins::useless_comparison(self, expr, parent);
```

prints out:

```
Located {
  location: Location { row: 3, column: 2 },
  end_location: Some(Location { row: 3, column: 6 }),
  custom: (),
  node: Compare {
    left: Located { location: Location { row: 3, column: 0 },
    end_location: Some(Location { row: 3, column: 1 }),
    custom: (),
    node: Constant { value: Int(1), kind: None } },
    ops: [Eq], comparators: [Located { location: Location { row: 3, column: 5 }, end_location: Some(Location { row: 3, column: 6 }), custom: (), node: Constant { value: Int(1), kind: None } }] } }
```

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1332 on 2022-11-04 17:33_

Could you use the start location of `node.left` for now?

---

_@charliermarsh reviewed on 2022-11-04 17:33_

---

_Review comment by @harupy on `src/check_ast.rs`:1332 on 2022-11-04 17:46_

Got it. Made the change.

---

_@harupy reviewed on 2022-11-04 17:46_

---

_Merged by @charliermarsh on 2022-11-04 17:47_

---

_Closed by @charliermarsh on 2022-11-04 17:47_

---

_@andersk reviewed on 2022-11-06 00:06_

---

_Review comment by @andersk on `src/check_ast.rs`:1331 on 2022-11-06 00:06_

This incorrectly fires for any comparison expression *contained* in an expression statement (e.g. `print(x < y)`), even if it’s not the whole statement.

- #616

---

_@charliermarsh reviewed on 2022-11-06 00:20_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1331 on 2022-11-06 00:20_

Thanks, good call, that’s on me as reviewer.

---
