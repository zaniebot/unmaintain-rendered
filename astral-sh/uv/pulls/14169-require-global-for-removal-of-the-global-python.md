```yaml
number: 14169
title: "Require `--global` for removal of the global Python pin"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/080
head: zb/rm-global
created_at: 2025-06-20T19:46:21Z
updated_at: 2025-06-26T17:22:39Z
url: https://github.com/astral-sh/uv/pull/14169
synced_at: 2026-01-12T16:11:03Z
```

# Require `--global` for removal of the global Python pin

---

_@zanieb_

While reviewing https://github.com/astral-sh/uv/pull/14107, @oconnor663 pointed out a bug where we allow `uv python pin --rm` to delete the global pin without the `--global` flag. I think that shouldn't be allowed? I'm not 100% certain though.

---

_Label `breaking` added by @zanieb on 2025-06-20 19:46_

---

_Marked ready for review by @zanieb on 2025-06-21 19:43_

---

_Review comment by @jtfmumm on `crates/uv-python/src/version_files.rs`:221 on 2025-06-26 12:36_

Do we need this method or could you use a new `is_global()` method instead? (see comment below)

---

_Review comment by @jtfmumm on `crates/uv-python/src/version_files.rs`:221 on 2025-06-26 12:40_

Something like:
```rust
  pub fn is_global(&self) -> bool {
      user_uv_config_dir().is_some_and(|dir| self.path == dir.join(PYTHON_VERSION_FILENAME))
  }
  ```

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/pin.rs`:76 on 2025-06-26 12:42_

Can this be replaced with a new `PythonVersionFile::is_global()` method (see comment above)?

---

_@jtfmumm reviewed on 2025-06-26 12:42_

---

_@zanieb reviewed on 2025-06-26 12:45_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:221 on 2025-06-26 12:45_

This constructor is used at https://github.com/astral-sh/uv/blob/ddab215bf937166f131e2df4c076923f27ca193c/crates/uv/src/commands/python/pin.rs#L206 to consolidate logic on where the global file is constructed.

We could add a `is_global` method too, but it'd just be a convenience method? I figured we could wait until there's more than one use-case for that.

---

_@jtfmumm reviewed on 2025-06-26 12:57_

---

_Review comment by @jtfmumm on `crates/uv-python/src/version_files.rs`:221 on 2025-06-26 12:57_

I don't have a strong opinion, but the condition below seemed more obscure than necessary. If we keep this method, can you mention when `None` is returned in the rustdoc?

---

_@zanieb reviewed on 2025-06-26 13:24_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:221 on 2025-06-26 13:24_

I'm happy to add the convenience method.

Separately, I'm still not sure how we could remove the `new_global` constructor without requiring the caller to provide the config path which is what I'm trying to avoid here so the logic is consolidated in the Python version file module. I can mention `None` in the docs.

---

_@jtfmumm reviewed on 2025-06-26 14:18_

---

_Review comment by @jtfmumm on `crates/uv-python/src/version_files.rs`:221 on 2025-06-26 14:18_

Yeah, the constructor makes sense

---

_@jtfmumm approved on 2025-06-26 14:58_

`file.path()` needs to be `self.path` in `is_global()` but otherwise looks good

---

_Comment by @zanieb on 2025-06-26 15:12_

Thanks! That's what I get for multitasking and pushing without `cargo check`.

---

_Comment by @codspeed-hq[bot] on 2025-06-26 17:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Frm-global?runnerMode=Instrumentation)

### Merging #14169 will **not alter performance**

<sub>Comparing <code>zb/rm-global</code> (c57cc1c) with <code>release/080</code> (54eea89)</sub>



### Summary

`✅ 3` untouched benchmarks  
`⁉️ 9` dropped benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zb%2Frm-global?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `` build_platform_tags[burntsushi-archlinux] `` | 278.9 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-long-compatible] `` | 13.2 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-long-incompatible] `` | 15.3 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-short-compatible] `` | 4.9 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing_failure[flyte-long-extension] `` | 1.8 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing_failure[flyte-short-extension] `` | 1.8 µs | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-long-compatible] `` | 1.9 µs | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-long-incompatible] `` | 1.3 µs | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-short-compatible] `` | 1.6 µs | N/A | N/A |


---

_Merged by @zanieb on 2025-06-26 17:22_

---

_Closed by @zanieb on 2025-06-26 17:22_

---

_Branch deleted on 2025-06-26 17:22_

---
