```yaml
number: 1059
title: Add allowed-confusable settings
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: allowed-confusables
created_at: 2022-12-05T13:35:11Z
updated_at: 2023-04-30T19:47:49Z
url: https://github.com/astral-sh/ruff/pull/1059
synced_at: 2026-01-12T15:55:05Z
```

# Add allowed-confusable settings

---

_@JonathanPlasse_

- Related to #1040

> **Warning**: The settings work when running manually, but they are ignored during the tests.
> What did I do wrong?

---

_Comment by @charliermarsh on 2022-12-05 15:13_

To fix your warning, you have to add a dedicated test case that tweaks the settings -- here's an example from `isort/mod.rs`:

```rust
#[test_case(Path::new("combine_as_imports.py"))]
fn combine_as_imports(path: &Path) -> Result<()> {
    let snapshot = format!("combine_as_imports_{}", path.to_string_lossy());
    let mut checks = test_path(
        Path::new("./resources/test/fixtures/isort")
            .join(path)
            .as_path(),
        &Settings {
            isort: isort::settings::Settings {
                combine_as_imports: true,
                ..isort::settings::Settings::default()
            },
            src: vec![Path::new("resources/test/fixtures/isort").to_path_buf()],
            ..Settings::for_rule(CheckCode::I001)
        },
        true,
    )?;
    checks.sort_by_key(|check| check.location);
    insta::assert_yaml_snapshot!(snapshot, checks);
    Ok(())
}
```

---

_Comment by @charliermarsh on 2022-12-05 17:17_

I'll go ahead and add the test + merge so this can make the next release.

---

_Merged by @charliermarsh on 2022-12-05 17:53_

---

_Closed by @charliermarsh on 2022-12-05 17:53_

---

_Branch deleted on 2023-04-30 19:47_

---
