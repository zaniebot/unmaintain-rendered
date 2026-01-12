```yaml
number: 5841
title: "Improve CLI documentation for `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/cli-docs-run
created_at: 2024-08-06T22:40:00Z
updated_at: 2024-08-07T13:26:25Z
url: https://github.com/astral-sh/uv/pull/5841
synced_at: 2026-01-12T16:07:03Z
```

# Improve CLI documentation for `uv run`

---

_@zanieb_

Adds more long-form help to `uv run`, which renders in `uv help run` and the CLI reference on the website.

---

_Review requested from @charliermarsh by @zanieb on 2024-08-06 22:43_

---

_@charliermarsh reviewed on 2024-08-07 01:43_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:458 on 2024-08-07 01:43_

"environment of discovered interpreter" (missing "the")

---

_@charliermarsh reviewed on 2024-08-07 01:43_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:455 on 2024-08-07 01:43_

"When used outside a project" to match above?

---

_@charliermarsh reviewed on 2024-08-07 01:43_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1987 on 2024-08-07 01:43_

Defined via? Defined as?

---

_@charliermarsh reviewed on 2024-08-07 01:44_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2017 on 2024-08-07 01:44_

I don't think this applies to inline metadata, does it?

---

_@charliermarsh approved on 2024-08-07 01:44_

---

_Label `documentation` added by @charliermarsh on 2024-08-07 01:45_

---

_@charliermarsh reviewed on 2024-08-07 02:07_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2102 on 2024-08-07 02:07_

Ok, I changed this in https://github.com/astral-sh/uv/pull/5846. I think you can just strike this paragraph... As of that PR, we now _will_ reuse a virtualenv if it exists, just following our standard semantics, same as if there were no project...

---

_@charliermarsh reviewed on 2024-08-07 02:07_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2102 on 2024-08-07 02:07_

`--no-project --isolated` would instead use an ephemeral env.

---

_@zanieb reviewed on 2024-08-07 12:59_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2017 on 2024-08-07 12:59_

I'm not sure. You can have a `tool.uv` section? I guess I'll check

---

_@charliermarsh reviewed on 2024-08-07 13:07_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2017 on 2024-08-07 13:07_

I don't _think_ we would respect that, because we return:

```rust
/// PEP 723 metadata as parsed from a `script` comment block.
///
/// See: <https://peps.python.org/pep-0723/>
#[derive(Debug, Serialize, Deserialize)]
#[serde(rename_all = "kebab-case")]
pub struct Pep723Metadata {
    pub dependencies: Vec<pep508_rs::Requirement<VerbatimParsedUrl>>,
    pub requires_python: Option<pep440_rs::VersionSpecifiers>,
}
```

---

_@zanieb reviewed on 2024-08-07 13:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2017 on 2024-08-07 13:10_

Yeah I confirmed we do not, which is debatably wrong. I'm fine with not reading `[tool.uv.dev-dependencies]` but we do need to respect `[tool.uv]` settings: #5855 

---

_Merged by @zanieb on 2024-08-07 13:26_

---

_Closed by @zanieb on 2024-08-07 13:26_

---

_Branch deleted on 2024-08-07 13:26_

---
