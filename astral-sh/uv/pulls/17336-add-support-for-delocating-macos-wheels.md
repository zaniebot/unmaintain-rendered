```yaml
number: 17336
title: Add support for delocating macOS wheels
type: pull_request
state: open
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/delocate
created_at: 2026-01-06T15:14:48Z
updated_at: 2026-01-12T02:10:01Z
url: https://github.com/astral-sh/uv/pull/17336
synced_at: 2026-01-12T16:12:44Z
```

# Add support for delocating macOS wheels

---

_@charliermarsh_

## Summary

This PR adds support for delocating macOS wheels. The behavior is not yet exposed on the CLI; it's only intended for internal-to-uv use. The implementation is based on https://github.com/matthew-brett/delocate, specifically `delocate-listdeps {wheel}` to list library dependencies (exposed as `list_wheel_dependencies()`) and `delocate-wheel --require-archs {delocate_archs} -w {dest_dir} {wheel}` to copy external libraries into binaries (exposed as `delocate_wheel()`).


---

_Review requested from @zanieb by @charliermarsh on 2026-01-06 15:25_

---

_Review requested from @konstin by @charliermarsh on 2026-01-06 15:25_

---

_Marked ready for review by @charliermarsh on 2026-01-06 17:38_

---

_Label `internal` added by @charliermarsh on 2026-01-06 17:52_

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:215 on 2026-01-07 09:37_

How does this logic compare to the one in install-wheel-rs?

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:99 on 2026-01-07 09:41_

```suggestion
fn hash_file(path: &Path) -> Result<(String, u64), DelocateError> {
    let mut file = File::open(path)?;
    let mut hasher = Sha256::new();
    let size = io::copy(&mut file, &mut hasher)?;

    let hash = hasher.finalize();
    let hash_str = format!("sha256={}", URL_SAFE_NO_PAD.encode(hash));

    Ok((hash_str, size))
}
```

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:106 on 2026-01-07 09:42_

Do we actually want to conserve all permissions, or only the executable bit? https://github.com/astral-sh/uv/blob/cc1ca8b4a1730be2997222b6ab317e7a69c49099/crates/uv-build-backend/src/wheel.rs#L675

Alternatively, should we remember the permission from the origin wheel instead?

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:96 on 2026-01-07 09:44_

We should show both paths in the error message

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:25 on 2026-01-07 09:48_

Does this guarantee that the compressed tag sets are sorted? https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#compressed-tag-sets

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:59 on 2026-01-07 10:08_

We have existing slightly different logic in `macos_deployment_target` in uv-configuration.

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:70 on 2026-01-07 10:08_

Do these have a specific source that documents what are system prefixes?

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:98 on 2026-01-07 10:12_

Do we need the patch version?

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:150 on 2026-01-07 10:14_

We should use the constants from goblin: https://docs.rs/goblin/latest/goblin/mach/header/index.html

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:102 on 2026-01-07 10:20_

This looks a lot like https://docs.rs/goblin/latest/goblin/mach/fn.peek_bytes.html

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:266 on 2026-01-07 10:26_

goblin uses `bytes.pread::<&str>(cmd.offset + command.dylib.name as usize)` for this, can we use that too? https://github.com/m4b/goblin/blob/fddcc4747ccf306469ff6092a953bd667ec8ed7d/src/mach/mod.rs#L216

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:295 on 2026-01-07 11:05_

This is already parsed in `macho.rpaths`

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:90 on 2026-01-07 11:11_

Where are we reading this field?

In case we aren't reading it, the names are already parsed as `macho.libs` by goblin

---

_Review comment by @konstin on `crates/uv-delocate/src/error.rs`:37 on 2026-01-07 11:17_

Those should be separate variants instead of a string payload

---

_Review comment by @konstin on `crates/uv-delocate/tests/integration_test.rs`:16 on 2026-01-07 11:27_

Inline mods don't match the patterns we usually use

---

_@konstin reviewed on 2026-01-07 11:28_

---

_@charliermarsh reviewed on 2026-01-07 14:02_

---

_Review comment by @charliermarsh on `crates/uv-delocate/tests/integration_test.rs`:16 on 2026-01-07 14:02_

It makes it easier to `#cfg` out chunks of tests.

---

_@charliermarsh reviewed on 2026-01-07 14:03_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/wheel.rs`:25 on 2026-01-07 14:03_

Wait, does our `WheelFilename` display logic even do that?

---

_@charliermarsh reviewed on 2026-01-07 14:06_

---

_Review comment by @charliermarsh on `crates/uv-delocate/tests/integration_test.rs`:16 on 2026-01-07 14:06_

I can move them into separate files though.

---

_@charliermarsh reviewed on 2026-01-07 14:08_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/macho.rs`:266 on 2026-01-07 14:08_

We can but we have to pull in `scroll` (which we already transitively depend on).

---

_@konstin reviewed on 2026-01-07 14:19_

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:25 on 2026-01-07 14:19_

We retain the input order, so given that all input wheels are correctly sorted, and all wheel the uv build backend creates are correctly sorted (`py3-none-any` matches that), we won't create invalid compressed tag sets. Or simpler: If there wasn't already an unsorted compressed tag set, we won't create one.

---

_Review requested from @konstin by @charliermarsh on 2026-01-07 15:28_

---

_Comment by @charliermarsh on 2026-01-07 15:39_

Thanks for the great review @konstin.

---

_@zanieb reviewed on 2026-01-08 13:29_

---

_Review comment by @zanieb on `crates/uv-configuration/src/target_triple.rs`:959 on 2026-01-08 13:29_

I think this might belong in `uv-platform`, but I think we can explore that separately from this pull request.

---

_@zanieb reviewed on 2026-01-08 13:31_

---

_Review comment by @zanieb on `crates/uv-delocate/src/macho.rs`:102 on 2026-01-08 13:31_

Hm this is duplicated with `uv-configuration` now?

---

_Review comment by @zanieb on `crates/uv-delocate/src/macho.rs`:31 on 2026-01-08 13:35_

I think I'd expect this to have `Known(Arch)` wrapping one of our other types instead. e.g., `uv-platform-tags::Arch`?

---

_@zanieb reviewed on 2026-01-08 13:35_

---

_@zanieb reviewed on 2026-01-08 13:39_

---

_Review comment by @zanieb on `crates/uv-delocate/src/macho.rs`:102 on 2026-01-08 13:39_

maybe that means https://github.com/astral-sh/uv/pull/17336#discussion_r2672368365 is actually relevant?

---

_@charliermarsh reviewed on 2026-01-08 13:54_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/macho.rs`:102 on 2026-01-08 13:54_

Yeah I just didn’t think it was important to  de-duplicate it, but happy to.

---

_@zanieb reviewed on 2026-01-08 14:14_

---

_Review comment by @zanieb on `crates/uv-delocate/src/macho.rs`:102 on 2026-01-08 14:14_

I'm just a little wary of our platform code sprawling further, I don't feel super strongly.

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:102 on 2026-01-08 14:47_

It would be nice to centralize the default macOS version assumption so they don't get out of sync.

---

_@konstin reviewed on 2026-01-08 14:47_

---

_@charliermarsh reviewed on 2026-01-08 15:22_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/macho.rs`:31 on 2026-01-08 15:22_

Personally it seems okay to me to have this here since it's specifically about Mach-O binary types and the use is very limited.

---
