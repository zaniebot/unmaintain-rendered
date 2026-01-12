```yaml
number: 4557
title: "Automatically detect workspace packages in `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/add-workspace
created_at: 2024-06-26T17:32:55Z
updated_at: 2024-06-27T07:02:11Z
url: https://github.com/astral-sh/uv/pull/4557
synced_at: 2026-01-12T16:06:18Z
```

# Automatically detect workspace packages in `uv add`

---

_@charliermarsh_

## Summary

If the package _isn't_ marked as `workspace = true`, locking will fail given:

```rust
let workspace_package_declared =
    // We require that when you use a package that's part of the workspace, ...
    !workspace.packages().contains_key(&requirement.name)
    // ... it must be declared as a workspace dependency (`workspace = true`), ...
    || matches!(
        source,
        Some(Source::Workspace {
            // By using toml, we technically support `workspace = false`.
            workspace: true,
            ..
        })
    )
    // ... except for recursive self-inclusion (extras that activate other extras), e.g.
    // `framework[machine_learning]` depends on `framework[cuda]`.
    || &requirement.name == project_name;
if !workspace_package_declared {
    return Err(LoweringError::UndeclaredWorkspacePackage);
}
```

Closes https://github.com/astral-sh/uv/issues/4552.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-26 17:32_

---

_Label `cli` added by @charliermarsh on 2024-06-26 17:33_

---

_Comment by @charliermarsh on 2024-06-26 17:33_

I am wondering why we require that workspace packages are marked as workspace if we _enforce_ it here. Should we just do this automatically?

---

_Review requested from @konstin by @charliermarsh on 2024-06-26 17:33_

---

_@ibraheemdev reviewed on 2024-06-26 17:44_

Should we error if you try to add a requirement with the same name from a different source? e.g. if you have a workspace member `foo` and run `uv add git+https://github.com/bar/foo`.

---

_Comment by @charliermarsh on 2024-06-26 17:47_

That seems reasonable, yeah.

---

_Comment by @charliermarsh on 2024-06-26 17:51_

Ahh, I think we already do that?

---

_Comment by @charliermarsh on 2024-06-26 17:56_

I added a test and improved the error messages.

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-26 18:00_

---

_@ibraheemdev approved on 2024-06-26 18:02_

---

_Merged by @charliermarsh on 2024-06-26 18:03_

---

_Closed by @charliermarsh on 2024-06-26 18:03_

---

_Branch deleted on 2024-06-26 18:03_

---

_Comment by @konstin on 2024-06-27 07:02_

> I am wondering why we require that workspace packages are marked as workspace if we enforce it here. Should we just do this automatically?

It ensures that you can see in a single pyproject.toml what source each requirements has (registry, url or workspace) and prevents accidentally using registry packages. I've copied this from cargo.

---
