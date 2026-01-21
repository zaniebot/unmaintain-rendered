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
updated_at: 2026-01-21T15:03:25Z
url: https://github.com/astral-sh/uv/pull/17336
synced_at: 2026-01-21T16:05:12Z
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

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:86 on 2026-01-07 09:41_

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

_Review comment by @zanieb on `crates/uv-configuration/src/target_triple.rs`:949 on 2026-01-08 13:29_

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

_Review comment by @konstin on `crates/uv-configuration/src/target_triple.rs`:303 on 2026-01-12 10:40_

That version is hardcoded both here and in https://github.com/astral-sh/uv/blob/330e56e778cb5313a8ddb84cace8ce32e87a944b/crates/uv-configuration/src/target_triple.rs#L300 and in the env var docs, I see use diverging in these locations.

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:483 on 2026-01-12 10:41_

We can avoid duplicating the RPATH/RECORD/writing logic by inverting this condition.

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:541 on 2026-01-12 10:42_

Is that a known constant?

---

_Review comment by @konstin on `crates/uv-platform/src/macos.rs`:82 on 2026-01-13 08:28_

Those seem like very fine-grained unit test, shouldn't we be able to rely on the derive for that?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:45 on 2026-01-13 08:40_

This looks a lot like `macos_deployment_target`, can we unify this into e.g. a `MacOSVersion::from_env`?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:24 on 2026-01-13 10:07_

Currently, these fields aren't written beyond the default constructor. Do we anticipate that we will expose all of them?

For example, will there be a delocate in uv where we modify the `require_archs` beyond the input tag, or where we wouldn't want to sanitize the rpaths?

---

_Review comment by @konstin on `crates/uv-delocate/src/error.rs`:41 on 2026-01-13 10:32_

This error is duplicating the filename context

---

_Review comment by @konstin on `crates/uv-delocate/src/macho.rs`:105 on 2026-01-13 10:55_

Why would the file not exist after we found it in the walkdir?

---

_Review comment by @konstin on `crates/uv-delocate/tests/delocate_integration.rs`:17 on 2026-01-13 10:59_

This test doesn't cover the output of `list_wheel_dependencies`, the part that's different from `delocate_wheel`

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:115 on 2026-01-13 11:21_

We can avoid the unwrap with if-let for all `strip_prefix`

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:122 on 2026-01-13 11:31_

The branches look the same to me

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:165 on 2026-01-13 11:38_

Aren't we stripping any potential relative path here? 

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:128 on 2026-01-13 11:40_

Is it intentional that we're using canonicalize as `is_file` check?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:365 on 2026-01-13 11:43_

Do we need to handle the error path here? This looks like a way to create a half-delocated wheel through a broken file somewhere in the dep tree

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:178 on 2026-01-13 11:45_

Isn't it the other way round, libraries that the binary in the wheel depends on?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:37 on 2026-01-13 11:53_

This flag is deprecated in the python delocate: https://github.com/matthew-brett/delocate/commit/ae65b6de12136053a4d4069c38ef0558702b31b2

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:385 on 2026-01-13 12:00_

We should be logging each edge here

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:27 on 2026-01-13 12:06_

I originally reads this as a typo of `libs_dir`, can we name this e.g. `lib_subdir`?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:521 on 2026-01-13 12:09_

```suggestion
        let dest_path = dest_dir.join(&filename_str);
```

---

_Review comment by @konstin on `crates/uv-delocate/tests/macho_parsing.rs`:73 on 2026-01-13 12:13_

Do we have a test case that transitive delocating works?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:588 on 2026-01-13 12:17_

This isn't necessarily a problem cause both are used in uv, but how are deciding between `relative_to` and `diff_paths`?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:589 on 2026-01-13 12:21_

I see two `return None` in that function, should they ever be reachable from this code? https://docs.rs/pathdiff/0.2.3/src/pathdiff/lib.rs.html#43

fwiw I'm not advocating for an `.unwrap()`here, I'd rather reduce their number, but maybe a `debug_assert!` or a runtime error, this call doesn't look like something that should have a static fallback.

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:597 on 2026-01-13 12:23_

Can you change the comment to explain what the `dependent_in_wheel.exists()` condition does?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:467 on 2026-01-13 12:24_

Do we care about the `min_macos_version` of the libraries we vendor?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:238 on 2026-01-13 12:34_

I know I asked this in the last iteration too, but does this ensure that compressed tag sets are sorted?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:529 on 2026-01-13 12:40_

We should delocate all modules in the wheel, not just the first one. It's a set in the original implementation, though that implementation fails with namespace packages: https://github.com/matthew-brett/delocate/blob/cf158a3a0c64fb1ed2db42aee6b1b470f652488f/delocate/tools.py#L937-L956 . Should we instead delocate the entire wheel, skipping only over top level directories that aren't module names (i.e. `.dist-info`, existing `.libs`)?

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:760 on 2026-01-13 12:56_

This should test wheel filename strings to wheel filename strings, so we can see the string formed output and whether the wheel filename are valid.

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:614 on 2026-01-13 13:14_

If we changed the tag, we must update the .dist-info/WHEELfile, otherwise we get bugs like https://github.com/astral-sh/uv/issues/16581

https://github.com/matthew-brett/delocate/blob/a97bc907ea5fab9256e2c92b995b332605fcd98b/delocate/delocating.py#L794-L821

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:121 on 2026-01-13 13:17_

```suggestion
        let relative_str = relative.portable_display();
```


---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:86 on 2026-01-13 13:20_

Is there a reason why we're using the version with manual iteration?

---

_Review comment by @konstin on `crates/uv-delocate/src/wheel.rs`:125 on 2026-01-13 13:24_

Is there a reason we're not using the existing `find_dist_info` with it's robust logic?

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:722 on 2026-01-13 13:30_

```suggestion
    #[attr_added_in("next version")]
```

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:728 on 2026-01-13 13:30_

```suggestion
    #[attr_added_in("next version")]
```

---

_@konstin reviewed on 2026-01-13 13:30_

---

_Converted to draft by @charliermarsh on 2026-01-15 18:50_

---

_@charliermarsh reviewed on 2026-01-15 18:58_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/delocate.rs`:24 on 2026-01-15 18:58_

I'm going to remove the two booleans.

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/delocate.rs`:128 on 2026-01-15 19:19_

Yes!

---

_@charliermarsh reviewed on 2026-01-15 19:19_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/delocate.rs`:178 on 2026-01-15 19:33_

I think the comment is correct... The keys are external libraries (like `/usr/local/lib/libfoo.dylib`), and the values are wheel binaries (or other bundled libs) that depend on this external library... I will improve...

---

_@charliermarsh reviewed on 2026-01-15 19:33_

---

_@charliermarsh reviewed on 2026-01-15 19:49_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/delocate.rs`:467 on 2026-01-15 19:49_

We do -- I believe that's `libraries.keys().map(PathBuf::as_path).

---

_@charliermarsh reviewed on 2026-01-15 19:51_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/delocate.rs`:760 on 2026-01-15 19:51_

I don't think I agree with this? That would be testing the wheel filename display, which shouldn't be tested here.

---

_@charliermarsh reviewed on 2026-01-15 19:54_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/delocate.rs`:529 on 2026-01-15 19:54_

Doesn't this just control where we _put_ the dylibs? we're still iterating over all binaries in the entire wheel. 

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/wheel.rs`:86 on 2026-01-15 20:07_

(I thought we'd need to wrap the `File` in a buffered reader anyway, which was incorrect.)

---

_@charliermarsh reviewed on 2026-01-15 20:07_

---

_Review requested from @konstin by @charliermarsh on 2026-01-15 20:25_

---

_Review requested from @zanieb by @charliermarsh on 2026-01-15 20:25_

---

_Marked ready for review by @charliermarsh on 2026-01-15 20:25_

---

_@konstin reviewed on 2026-01-21 12:36_

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:529 on 2026-01-21 12:36_

:+1: for the new naming, makes this much easier to follow.

---

_@konstin reviewed on 2026-01-21 12:39_

---

_Review comment by @konstin on `crates/uv-delocate/src/delocate.rs`:760 on 2026-01-21 12:39_

Maybe that's a different test, what I'm interested in seeing more than the exact shape of the tags is that the eventual string wheel filename is correct, after going through everything include the `WheelFilename`'s internal representation and the `Display` implementation.

---

_@charliermarsh reviewed on 2026-01-21 15:03_

---

_Review comment by @charliermarsh on `crates/uv-delocate/src/delocate.rs`:760 on 2026-01-21 15:03_

Done

---
