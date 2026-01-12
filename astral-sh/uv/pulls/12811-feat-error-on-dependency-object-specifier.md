```yaml
number: 12811
title: "feat: Error on dependency object specifier"
type: pull_request
state: merged
author: kiran-4444
labels:
  - error messages
  - breaking
assignees: []
merged: true
base: main
head: 12638/dependency-object-specifier
created_at: 2025-04-10T16:27:38Z
updated_at: 2025-04-10T20:59:09Z
url: https://github.com/astral-sh/uv/pull/12811
synced_at: 2026-01-12T16:10:24Z
```

# feat: Error on dependency object specifier

---

_@kiran-4444_

## Summary

This PR errors out when an Unknown Dependency Object Specifier is used in dependency groups.


Fixes #12638 

## Test Plan

The current behaviour is as follows:

```bash
➜  example git:(12638/dependency-object-specifier) ✗ cargo run -- sync
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.21s
     Running `/home/luna/Documents/uv/target/debug/uv sync`
error: Failed to generate package metadata for `example==0.1.0 @ virtual+.`
  Caused by: Group `bar` contains a Dependency Object Specifier, which is not supported by uv
  ```


And the pyproject.toml to produce this is:

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.2"
dependencies = []

[dependency-groups]
foo = ["pyparsing"]
bar = [{set-phasers-to = "stun"}]
```
  
  

---

_Review requested from @Gankra by @Gankra on 2025-04-10 19:36_

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:158 on 2025-04-10 19:42_

Can you make this take and print out the map? (Also I wonder if this should be less jargony...)

```suggestion
    #[error("Group `{0}` contains an unknown dependency object specifier: {1:?}")]
    DependencyObjectSpecifierNotSupported(GroupName, BTreeMap<String, String>),
```

---

_@Gankra approved on 2025-04-10 19:42_

---

_Label `internal` added by @Gankra on 2025-04-10 19:57_

---

_Review requested from @zanieb by @Gankra on 2025-04-10 20:55_

---

_Comment by @Gankra on 2025-04-10 20:56_

Rad, thanks! (Gonna just land this, if we want to bikeshed the error message more I'm willing to do that followup)

---

_Merged by @Gankra on 2025-04-10 20:56_

---

_Closed by @Gankra on 2025-04-10 20:56_

---

_Label `error messages` added by @Gankra on 2025-04-10 20:56_

---

_Label `internal` removed by @Gankra on 2025-04-10 20:57_

---

_Label `breaking` added by @Gankra on 2025-04-10 20:58_

---

_Added to milestone `v0.7.0` by @Gankra on 2025-04-10 20:58_

---

_Comment by @Gankra on 2025-04-10 20:59_

Just realized this is technically breaking and should probably be delayed for 0.7.0 -- I'll handle the revert/reland if so.

---
