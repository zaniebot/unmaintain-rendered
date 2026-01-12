```yaml
number: 2040
title: Upgrade to toml v0.5.11
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/toml
created_at: 2023-01-20T21:26:24Z
updated_at: 2023-01-21T12:26:10Z
url: https://github.com/astral-sh/ruff/pull/2040
synced_at: 2026-01-12T15:55:07Z
```

# Upgrade to toml v0.5.11

---

_@charliermarsh_

In #1680, we moved over to `toml_edit`. But it looks like `toml` now uses `toml_edit`, and has implemented some improvements (e.g., this closes #1894).

---

_Comment by @charliermarsh on 2023-01-20 21:39_

\cc @messense 

---

_Merged by @charliermarsh on 2023-01-20 22:20_

---

_Closed by @charliermarsh on 2023-01-20 22:20_

---

_Branch deleted on 2023-01-20 22:20_

---

_Comment by @messense on 2023-01-21 00:07_

Looks like the released version on crates.io doesn’t use toml_edit currently: https://crates.io/crates/toml/0.5.11/dependencies

---

_Comment by @charliermarsh on 2023-01-21 00:23_

Oh, strange. Any idea what the state of the crate is? Should I revert this?

---

_Comment by @messense on 2023-01-21 01:27_

Only `toml` released a new version without any noticeable behavior changes, `toml_edit` is still on 0.17.1 which doesn't resolve the issue.

> Should I revert this?

For now you can revert it to be more compliant with TOML 1.0. At least a new `toml_edit` release a required for #1894.

Or we can just depends on `toml` main branch using git dependency?

---

_Comment by @charliermarsh on 2023-01-21 02:25_

Interesting, I did run through the example described in that issue and it seemed to work with the new crate. Is that unexpected?

---

_Comment by @charliermarsh on 2023-01-21 05:00_

@messense - It looks like v0.5.11 was mostly about deprecations in advance of the toml-edit migration. I am confused though why this now works:

```toml
[tool.hatch]
version.source = "vcs"

[tool.hatch.version.raw-options]
local_scheme = "no-local-version"
```

So, I might leave it as-is for now? Is that bad? What did we lose by not supporting TOML 1.0?

---

_Comment by @messense on 2023-01-21 05:31_

It still fails for me?

```
cargo run
   Compiling toml-test v0.1.0 (/private/tmp/toml-test)
    Finished dev [unoptimized + debuginfo] target(s) in 0.16s
     Running `target/debug/toml-test`
Err(
    Error {
        inner: ErrorInner {
            kind: Custom,
            line: Some(
                5,
            ),
            col: 0,
            at: Some(
                55,
            ),
            message: "duplicate key: `version`",
            key: [
                "tool",
                "hatch",
            ],
        },
    },
)
```

**Cargo.toml**

```toml
[package]
name = "toml-test"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
toml = "0.5.11"
```

**src/main.rs**

```rust
fn main() {
    let content = r#"# pyproject.toml

[tool.hatch]
version.source = "vcs"

[tool.hatch.version.raw-options]
local_scheme = "no-local-version""#;
    println!("{:#?}", toml::from_str::<toml::Value>(content));
}
```

---

_Comment by @charliermarsh on 2023-01-21 05:55_

Interesting, let me try again in the morning.

---

_Comment by @charliermarsh on 2023-01-21 12:26_

Yeah I can reproduce that, but interestingly, this works fine:

```rs
let content = r#"# pyproject.toml

[tool.hatch]
version.source = "vcs"

[tool.hatch.version.raw-options]
local_scheme = "no-local-version""#;

println!("{:#?}", toml::from_str::<Pyproject>(content));
```

---
