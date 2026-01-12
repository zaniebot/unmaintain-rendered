```yaml
number: 11115
title: "Read user configuration from `~/.config/ruff/ruff.toml` on macOS"
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.5
head: charlie/xdg
created_at: 2024-04-23T20:08:24Z
updated_at: 2024-06-24T13:21:45Z
url: https://github.com/astral-sh/ruff/pull/11115
synced_at: 2026-01-12T15:55:34Z
```

# Read user configuration from `~/.config/ruff/ruff.toml` on macOS

---

_@charliermarsh_

## Summary

This PR moves Ruff's user-specific configuration from `~/Library/Application Support/ruff/ruff.toml` to `~/.config/ruff/ruff.toml`.

Many other tools do this. On my machine alone: dagger, zed, gatsby, gh, wandb, etc.

I also polled Twitter and it won in a landslide:

![Screenshot 2024-04-23 at 4 07 19 PM](https://github.com/astral-sh/ruff/assets/1309177/d2840968-f310-4501-85a8-6994c4e7085b)

Let's ship this in v0.5.0, along with a deprecation warning (so we'll continue to respect `~/Library/Application Support/ruff/ruff.toml`, but log a warning).

Closes https://github.com/astral-sh/ruff/issues/10739.

## Test Plan

- Created a file with an unused import.
- Ran `cargo check foo.py`.
- Verified that the deprecated configuration was loaded and respected, with a warning:

```
warning: Reading configuration from `~/Library/Application Support` is deprecated. Please move your configuration to `/Users/crmarsh/.config/ruff/ruff/ruff.toml`.
[2024-04-23][16:09:10][ruff::resolve][DEBUG] Using configuration file (via cwd) at: /Users/crmarsh/Library/Application Support/ruff/ruff.toml
```

- Created `~/.config/ruff/ruff.toml`.
- Verified that the new configuration was loaded:
```
[2024-04-23][16:10:23][ruff::resolve][DEBUG] Using configuration file (via cwd) at: /Users/crmarsh/.config/ruff/ruff.toml
```


---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-04-23 20:11_

---

_Label `breaking` added by @charliermarsh on 2024-04-23 20:11_

---

_Label `configuration` added by @charliermarsh on 2024-04-23 20:11_

---

_Label `breaking` removed by @charliermarsh on 2024-04-23 20:11_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-23 20:11_

---

_Review requested from @konstin by @charliermarsh on 2024-04-23 20:11_

---

_@charliermarsh reviewed on 2024-04-23 20:28_

---

_Review comment by @charliermarsh on `Cargo.toml`:63 on 2024-04-23 20:28_

@konstin suggested this crate, and it does what we want across Windows and Linux / macOS, but open to other suggestions.

---

_@charliermarsh reviewed on 2024-04-23 20:28_

---

_Review comment by @charliermarsh on `Cargo.toml`:61 on 2024-04-23 20:28_

We can remove `dirs` in a future release, but it stays around for now for backwards compatibility.

---

_Comment by @github-actions[bot] on 2024-04-23 20:45_

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

_Comment by @MichaReiser on 2024-04-24 07:28_

Sooo, we're doing twitter driven development now :joy:

---

_@MichaReiser approved on 2024-04-24 07:30_

---

_Comment by @konstin on 2024-04-24 11:36_

I would change the reasoning/description: Follow the XDG specification on all Unix platforms. Ruff will now use the XDG config directory to read user-level configuration (default: `~/.config/ruff/ruff.toml`) over `Library/Application Support/ruff/ruff.toml` on macOS.

---

_@konstin approved on 2024-04-24 11:37_

---

_@MichaReiser reviewed on 2024-06-24 12:23_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:117 on 2024-06-24 12:23_

Could we use https://docs.rs/etcetera/latest/etcetera/app_strategy/struct.Apple.html here and remove the `dirs` dependency (trading one dependency for another ;))

---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-24 12:28_

---

_@MichaReiser reviewed on 2024-06-24 12:38_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:117 on 2024-06-24 12:38_

Okay I checked, we can use the `Apple` strategy but we need to use the `data_dir` to map to the `Application Support` folder (hardcoded). 

The `etcetera` crate clearly documents that `config_dir` shouldn't be used on Mac when the config is user editable. I'm going to make this change.

From the [`dirs` documentation](https://crates.io/crates/dirs): 

```
// Mac: Some(/Users/Alice/Library/Application Support)
dirs::config_dir();
```

From the [`etcetera` documentation](https://docs.rs/etcetera/latest/etcetera/base_strategy/struct.Apple.html):

```rust
assert_eq!(
    base_strategy.data_dir().strip_prefix(&home_dir),
    Ok(Path::new("Library/Application Support/"))
);
```

---

_@charliermarsh reviewed on 2024-06-24 12:50_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/pyproject.rs`:117 on 2024-06-24 12:50_

Interesting. Feel free to merge.

---

_Comment by @codspeed-hq[bot] on 2024-06-24 12:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/xdg)

### Merging #11115 will **degrade performances by 4.69%**

<sub>Comparing <code>charlie/xdg</code> (c4d143a) with <code>ruff-0.5</code> (9348821)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/xdg)._

### Benchmarks breakdown

|     | Benchmark | `ruff-0.5` | `charlie/xdg` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.69% |


---

_Comment by @MichaReiser on 2024-06-24 13:03_

I ran through the test plan and verified the deprecation message.

---

_Merged by @MichaReiser on 2024-06-24 13:20_

---

_Closed by @MichaReiser on 2024-06-24 13:20_

---

_Branch deleted on 2024-06-24 13:20_

---

_Label `breaking` added by @MichaReiser on 2024-06-24 13:20_

---
