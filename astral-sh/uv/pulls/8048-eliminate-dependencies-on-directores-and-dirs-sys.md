```yaml
number: 8048
title: "Eliminate dependencies on `directores` and `dirs-sys`"
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: tracking/050
head: dir
created_at: 2024-10-09T15:33:54Z
updated_at: 2024-10-24T14:30:27Z
url: https://github.com/astral-sh/uv/pull/8048
synced_at: 2026-01-12T16:08:08Z
```

# Eliminate dependencies on `directores` and `dirs-sys`

---

_@j178_

## Summary

Migrate all directory related logic to `etcetera`, eliminated two dependecies.


---

_Comment by @j178 on 2024-10-09 17:04_

I added a validation test in https://github.com/astral-sh/uv/pull/8048/commits/df5066d677cce04afdb656c5f6407b34948d5571, to compare the outputs of the old and new code, and it passed successfully. This confirms that this PR keeps the directories unchanged.

```rust
#[cfg(test)]
mod tests {
    use etcetera::BaseStrategy;
    use std::path::PathBuf;

    fn dirs_before() -> (PathBuf, PathBuf, PathBuf, PathBuf) {
        let data_dir = directories::ProjectDirs::from("", "", "uv")
            .unwrap()
            .data_dir()
            .to_path_buf();

        let cache_dir = directories::ProjectDirs::from("", "", "uv")
            .unwrap()
            .cache_dir()
            .to_path_buf();

        let config_dir = {
            // On Windows, use, e.g., C:\Users\Alice\AppData\Roaming
            #[cfg(windows)]
            {
                dirs_sys::known_folder_roaming_app_data()
            }
            // On Linux and macOS, use, e.g., /home/alice/.config.
            #[cfg(not(windows))]
            {
                std::env::var_os("XDG_CONFIG_HOME")
                    .and_then(dirs_sys::is_absolute_path)
                    .or_else(|| dirs_sys::home_dir().map(|path| path.join(".config")))
            }
        };

        let home_dir = {
            #[cfg(windows)]
            {
                dirs_sys::known_folder_profile()
            }
            #[cfg(not(windows))]
            {
                dirs_sys::home_dir()
            }
        };

        (data_dir, cache_dir, config_dir.unwrap(), home_dir.unwrap())
    }

    fn dirs_after() -> (PathBuf, PathBuf, PathBuf, PathBuf) {
        let data_dir = etcetera::base_strategy::choose_native_strategy()
            .unwrap()
            .data_dir()
            .join("uv");
        let data_dir = if cfg!(windows) {
            data_dir.join("data")
        } else {
            data_dir
        };

        let cache_dir = etcetera::base_strategy::choose_native_strategy()
            .unwrap()
            .cache_dir()
            .join("uv");
        let cache_dir = if cfg!(windows) {
            cache_dir.join("cache")
        } else {
            cache_dir
        };

        let config_dir = etcetera::base_strategy::choose_base_strategy()
            .unwrap()
            .config_dir();
        let home_dir = etcetera::home_dir().unwrap();

        (data_dir, cache_dir, config_dir, home_dir)
    }

    #[test]
    fn ensure_directory_not_changed() {
        let before = dirs_before();
        let after = dirs_after();
        assert_eq!(before, after);
    }
}
```

---

_Marked ready for review by @j178 on 2024-10-09 17:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-09 17:40_

---

_Comment by @charliermarsh on 2024-10-09 17:40_

Thanks! I can review, I'm responsible for some of the mess.

---

_Label `internal` added by @charliermarsh on 2024-10-09 17:46_

---

_@charliermarsh reviewed on 2024-10-09 21:23_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:173 on 2024-10-09 21:23_

I think this isn't quite the same. It looks like etcetera will use local `AppData` and then falls back to Roaming `AppData`, whereas the above always uses the Roaming version?

---

_@charliermarsh reviewed on 2024-10-09 21:25_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:389 on 2024-10-09 21:25_

This seems to be _mostly_ the same except that it defers to `USERPROFILE` first?

---

_@j178 reviewed on 2024-10-10 01:35_

---

_Review comment by @j178 on `crates/uv-settings/src/lib.rs`:173 on 2024-10-10 01:35_

`dirs-sys` uses the [`KnownFolderID`](https://learn.microsoft.com/en-us/windows/win32/shell/knownfolderid) `FOLDERID_RoamingAppData`, while `etcetera` uses the [`CSIDL`](https://learn.microsoft.com/en-us/windows/win32/shell/csidl) `CSIDL_APPDATA`. And `FOLDERID_RoamingAppData` is equivalent to `CSIDL_APPDATA` in CSIDL.

If the `SHGetFolderPathW` call fails, etcetera will fall back to use `self.home_dir.join("AppData").join("Roaming")`.  So it seems etcetera always uses the Roaming APPDATA too, they should be the same.


---

_@j178 reviewed on 2024-10-10 01:43_

---

_Review comment by @j178 on `crates/uv-tool/src/lib.rs`:389 on 2024-10-10 01:43_

Yeah, that's unfortunate. What's your suggestion?

---

_@charliermarsh reviewed on 2024-10-10 09:22_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:389 on 2024-10-10 09:22_

It looks like we already depend on `homedirs` via `axoupdater`... So in theory we could call `homedirs::my_home` here at least on Windows, which does seem to use `FOLDERID_Profile`. Or we could just inline our own `home_dir` impl for Windows here, and add a TODO to move to `etcetera` on the next breaking release (0.5.0). _Or_ we could just ship this whole thing in 0.5.0.

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:389 on 2024-10-10 09:41_

@zanieb -- I think my preference here is just to ship this as part of 0.5.0 in the near-ish future, and accept that we now respect non-empty `USERPROFILE` on Windows (which seems fine).

---

_@charliermarsh reviewed on 2024-10-10 09:41_

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-10-16 19:39_

---

_Comment by @charliermarsh on 2024-10-21 18:22_

Gonna merge into v0.5.0.

---

_@charliermarsh approved on 2024-10-21 18:39_

---

_Merged by @charliermarsh on 2024-10-21 18:42_

---

_Closed by @charliermarsh on 2024-10-21 18:42_

---

_Branch deleted on 2024-10-24 14:30_

---
