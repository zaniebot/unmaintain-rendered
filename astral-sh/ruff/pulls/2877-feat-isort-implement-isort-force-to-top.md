```yaml
number: 2877
title: "feat(isort): Implement isort.force_to_top"
type: pull_request
state: merged
author: spaceone
labels: []
assignees: []
merged: true
base: main
head: isort-force-to-top
created_at: 2023-02-14T00:55:10Z
updated_at: 2023-02-16T19:02:00Z
url: https://github.com/astral-sh/ruff/pull/2877
synced_at: 2026-01-12T15:55:11Z
```

# feat(isort): Implement isort.force_to_top

---

_@spaceone_

test what `isort` does via:
`isort -t "lib1" -t "lib3" -t "lib5" -t "lib3.lib4" -t "z" crates/ruff/resources/test/fixtures/isort/force_to_top.py`

Issue #2695

---

_@charliermarsh reviewed on 2023-02-15 16:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/sorting.rs`:132 on 2023-02-15 16:02_

Are the commented-out lines included by accident?

---

_Review comment by @spaceone on `crates/ruff/src/rules/isort/sorting.rs`:132 on 2023-02-15 16:14_

no, on purpose currently. I am not sure if we should sort `from`-imports also there. From reading the `isort` code it just splits anything at ` ` and looks if this is in `force_to_top`.
Also the commented code had one error which I don't remember where I couldn't find a rust-solution. 

---

_@spaceone reviewed on 2023-02-15 16:14_

---

_Review comment by @spaceone on `crates/ruff/src/rules/isort/sorting.rs`:132 on 2023-02-15 16:15_

`isort` also doesn't really have much tests for `force-to-top`, so I have to reverse engineer a little how it handles `from`-imports.

---

_@spaceone reviewed on 2023-02-15 16:15_

---

_@spaceone reviewed on 2023-02-15 16:18_

---

_Review comment by @spaceone on `crates/ruff/src/rules/isort/sorting.rs`:132 on 2023-02-15 16:18_

I think the problem was that I don't know how to convert a `import_from1.module` into a string. maybe you can help here?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/sorting.rs`:132 on 2023-02-15 16:57_

Does this trait help at all?

```rust
pub trait Importable {
    fn module_name(&self) -> String;
    fn module_base(&self) -> String;
}

impl Importable for AliasData<'_> {
    fn module_name(&self) -> String {
        self.name.to_string()
    }

    fn module_base(&self) -> String {
        self.module_name().split('.').next().unwrap().to_string()
    }
}

impl Importable for ImportFromData<'_> {
    fn module_name(&self) -> String {
        ast::helpers::format_import_from(self.level, self.module)
    }

    fn module_base(&self) -> String {
        self.module_name().split('.').next().unwrap().to_string()
    }
}
```

---

_@charliermarsh reviewed on 2023-02-15 16:57_

---

_@spaceone reviewed on 2023-02-15 17:27_

---

_Review comment by @spaceone on `crates/ruff/src/rules/isort/sorting.rs`:132 on 2023-02-15 17:27_

thanks. can be merged then.

---

_Merged by @charliermarsh on 2023-02-16 19:02_

---

_Closed by @charliermarsh on 2023-02-16 19:02_

---
