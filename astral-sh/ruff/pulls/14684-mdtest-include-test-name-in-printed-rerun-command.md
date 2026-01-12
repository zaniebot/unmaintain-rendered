```yaml
number: 14684
title: "mdtest: include test name in printed rerun command"
type: pull_request
state: merged
author: connorskees
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: mdtest-name
created_at: 2024-11-29T18:45:38Z
updated_at: 2024-11-30T11:05:23Z
url: https://github.com/astral-sh/ruff/pull/14684
synced_at: 2026-01-12T15:55:48Z
```

# mdtest: include test name in printed rerun command

---

_@connorskees_

This change follows up to https://github.com/astral-sh/ruff/pull/14670 by adding a test name argument to the printed `cargo test` command that can be copied to rerun a specific failing test. 

This avoids reruns appearing as though they're running and passing all other tests. 

This unfortunately has to work by borrowing some code from [dir-test](https://github.com/fe-lang/dir-test), as it's not possible to reconstruct the full test name using hacks like [`module_path!`](https://doc.rust-lang.org/std/macro.module_path.html) or (at least in my testing) [`std::any::type_name`](https://doc.rust-lang.org/beta/std/any/fn.type_name.html). 

---

_Review requested from @carljm by @connorskees on 2024-11-29 18:45_

---

_Review requested from @MichaReiser by @connorskees on 2024-11-29 18:45_

---

_Review requested from @AlexWaygood by @connorskees on 2024-11-29 18:45_

---

_Review requested from @sharkdp by @connorskees on 2024-11-29 18:45_

---

_Comment by @github-actions[bot] on 2024-11-29 18:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `testing` added by @AlexWaygood on 2024-11-29 18:53_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-29 18:53_

---

_Comment by @MichaReiser on 2024-11-29 18:58_

I looked at `dir_test,` and the reason why `type_name` doesn't work is that `dir_test` keeps the function as is and instead generates new functions that hold the test corpus. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2024-11-29 19:00_

```suggestion
    for component in rel_path.parent().unwrap().components() {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/tests/mdtest.rs`:59 on 2024-11-29 19:01_

It could be worth using `SystemPath` for `fixture_path`. It avoids the need to deal with non-utf8 paths through the test code

```
let fixture_path = SystemPath::new(fixture.path()).unwrap();
```

---

_@MichaReiser approved on 2024-11-29 19:02_

Nice

---

_@connorskees reviewed on 2024-11-29 22:48_

---

_Review comment by @connorskees on `crates/red_knot_python_semantic/tests/mdtest.rs`:59 on 2024-11-29 22:48_

hm this ends up requiring changes in a lot of places, unless we just say `.as_std_path()` at some point down the line. probably worth doing as a separate change

---

_Merged by @MichaReiser on 2024-11-30 11:01_

---

_Closed by @MichaReiser on 2024-11-30 11:01_

---
