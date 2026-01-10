```yaml
number: 587
title: Support editable in pip-sync and pip-compile
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konstin/editable-in-pip-sync
created_at: 2023-12-07T21:27:57Z
updated_at: 2023-12-17T04:19:10Z
url: https://github.com/astral-sh/uv/pull/587
synced_at: 2026-01-10T15:44:44Z
```

# Support editable in pip-sync and pip-compile

---

_Pull request opened by @konstin on 2023-12-07 21:27_

Support `-e path/do/dir` in pip-sync and and pip-compile.

---

_Comment by @konstin on 2023-12-07 21:28_

Current dependencies on/for this PR:
* main
  * **PR #564** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/564?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #565** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/565?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #566** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/566?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #567** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/567?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #587** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/587?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/587?utm_source=stack-comment).

---

_Comment by @konstin on 2023-12-07 21:28_

Current dependencies on/for this PR:
* main
  * **PR #564** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/564?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #565** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/565?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #566** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/566?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #567** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/567?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #587** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/587?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/587?utm_source=stack-comment).

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_compile.rs`:184 on 2023-12-12 17:56_

We can't `impl Metadata for SourceDist` for those before building, no package name (unless we make some custom strange syntax)

---

_@konstin reviewed on 2023-12-12 17:56_

---

_@konstin reviewed on 2023-12-12 17:59_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:287 on 2023-12-12 17:59_

These should be modeled as `CachedDist` instead

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/lib.rs`:95 on 2023-12-12 21:10_

Without?

---

_@charliermarsh reviewed on 2023-12-12 21:10_

---

_@charliermarsh reviewed on 2023-12-12 21:14_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:287 on 2023-12-12 21:14_

Should this not be part of the `puffin_installer::Installer` install? Why do we need a separate path for editable deps here? (Or is that what you mean by your comment above.)

---

_@charliermarsh reviewed on 2023-12-12 21:15_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:233 on 2023-12-12 21:15_

I'm not a huge fan of all the extra paths for editable installs. Can we try to fit them into our existing abstractions? For example, I would've expected that the editable installs just happen within `Downloader` and end up in `wheels`. Why do we need a separate path? (Also, does this actually build a "wheel"?)

---

_@charliermarsh reviewed on 2023-12-12 21:17_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:233 on 2023-12-12 21:17_

I think this is my primary concern -- can we fit editables into our existing abstractions? Or, at least, add more abstraction such that there isn't so much of this low-level operations code in our command files? (For example, at the very least, could `Downloader` have a separate hook for doing the editable installs?)

---

_@konstin reviewed on 2023-12-13 08:51_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:287 on 2023-12-13 08:51_

Yeah, currently they aren't part of that path because the types are different

---

_@konstin reviewed on 2023-12-13 22:30_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:2593 on 2023-12-13 22:30_

This is wrong but passes on the cli, i think i have to normalize the `..` out, will investigate

---

_Marked ready for review by @konstin on 2023-12-13 22:33_

---

_Comment by @konstin on 2023-12-13 22:35_

The most important things work: We build editable wheel, extract metadata or install them, we build them in parallel, we report their building progress, we only audit them if they are installed. I would try to merge this and open issues for everything else, there are merge conflicts again and i only solved a big bunch of them already.

---

_Review requested from @charliermarsh by @konstin on 2023-12-13 22:35_

---

_@charliermarsh reviewed on 2023-12-13 23:08_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/reporters.rs`:268 on 2023-12-13 23:08_

Why this change? I don't think this is the same. In the existing code, only the _version_ is dimmed.

---

_@charliermarsh reviewed on 2023-12-13 23:09_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:390 on 2023-12-13 23:09_

Is this just moved? Any changes?

---

_@charliermarsh reviewed on 2023-12-13 23:16_

---

_Review comment by @charliermarsh on `scripts/editable-installs/requirements.txt`:14 on 2023-12-13 23:16_

Is all this stuff in `scripts/editable-installs` intended to be checked in?

---

_@charliermarsh reviewed on 2023-12-13 23:17_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/reporters.rs`:253 on 2023-12-13 23:17_

I don't think we should change these to `&dyn Display`. But we could change them to `&dyn Metadata`, which should be enough for `impl ColorDisplay`, right?

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:105 on 2023-12-13 23:21_

I think we should wrap this tuple in a struct.

---

_@charliermarsh reviewed on 2023-12-13 23:21_

---

_@charliermarsh reviewed on 2023-12-13 23:23_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:208 on 2023-12-13 23:23_

Why / when would these be requested?

---

_@charliermarsh reviewed on 2023-12-13 23:24_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:205 on 2023-12-13 23:24_

I don't think we should be using `.to_string()` here. It should probably be `.distribution_id()`? Honestly, we might want a dedicated type for this. It took me a while to understand why it was a hash map with string keys.

---

_@charliermarsh reviewed on 2023-12-13 23:26_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:208 on 2023-12-13 23:26_

I think we should consider using a `VerbatimPath` here rather than this wrapper type.

Where is the path being mapped back to the original version? I can't find that.

---

_@charliermarsh reviewed on 2023-12-13 23:26_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:2593 on 2023-12-13 23:26_

In what way is it wrong?

---

_@charliermarsh reviewed on 2023-12-13 23:28_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:208 on 2023-12-13 23:28_

I actually find this wrapper type a little confusing. We match on `editable` to create it, and it includes `editable` as one of its fields. So it's some kind of denormalized representation?

---

_@charliermarsh reviewed on 2023-12-13 23:29_

Nice, I know this was a ton of work. Good job figuring it out.

I'm not gonna push on trying to remove the dedicated paths for editable installs etc., but I do think we should consider changing the path representation at least.

---

_Comment by @charliermarsh on 2023-12-13 23:30_

I will fix all of these merge conflicts, and I may even take some of the smaller changes here and commit them as separate PRs to help move this along.

---

_@charliermarsh reviewed on 2023-12-13 23:30_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:224 on 2023-12-13 23:30_

Why does this now need to be cloned? Can we avoid this?

---

_@charliermarsh reviewed on 2023-12-13 23:31_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:224 on 2023-12-13 23:31_

Like `CanonicalUrl::new` only needs a borrowed representation, so it seems unnecessary.

---

_Comment by @charliermarsh on 2023-12-14 00:28_

@konstin - Did you a solid and fixed all the merge conflicts...

---

_@charliermarsh reviewed on 2023-12-14 00:30_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:2590 on 2023-12-14 00:30_

I think this test introduces a dependency on Maturin. Or, at least, I can't run this test locally, so we need to fix the setup.

---

_@charliermarsh reviewed on 2023-12-14 00:55_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:205 on 2023-12-14 00:55_

I added a dedicated type on `main`.

---

_@charliermarsh reviewed on 2023-12-14 01:16_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/reporters.rs`:253 on 2023-12-14 01:16_

In short: I think we could change all of these to `&dyn Metadata`, then add:

```rust
impl ColorDisplay for &dyn Metadata {
    fn to_color_string(&self) -> String {
        let name = self.name();
        let version_or_url = self.version_or_url();
        format!("{}{}", name, version_or_url.to_string().dimmed())
    }
}
```

---

_@charliermarsh reviewed on 2023-12-14 01:19_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/reporters.rs`:253 on 2023-12-14 01:19_

https://github.com/astral-sh/puffin/pull/645

---

_@konstin reviewed on 2023-12-14 11:38_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:390 on 2023-12-14 11:38_

Yes, i just turned the method into a function

---

_@konstin reviewed on 2023-12-14 11:39_

---

_Review comment by @konstin on `scripts/editable-installs/requirements.txt`:14 on 2023-12-14 11:39_

Yes, those are helpful for development

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:224 on 2023-12-14 11:55_

We'd have to rewrite the signature of `AllowedUrls`, if i just use `&Url` then `editable.url()` doesn't live long enough

---

_@konstin reviewed on 2023-12-14 11:55_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:2590 on 2023-12-14 11:55_

That's a bug! Could you share the error?

---

_@konstin reviewed on 2023-12-14 11:55_

---

_@konstin reviewed on 2023-12-14 11:58_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:208 on 2023-12-14 11:58_

The `requirement` is what the user specified and what we want to write to `requirements.txt`, this may be a local path or a git url. The `path` is where you can actually find the directory or checkout for the build. Currently we only use it with a path twice.

---

_@konstin reviewed on 2023-12-14 11:59_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:208 on 2023-12-14 11:59_

The mapping back happens at https://github.com/astral-sh/puffin/blob/9e66e4601ff5154e638740d13595acfdc69ba9bb/crates/puffin-resolver/src/resolution.rs#L267-L271


---

_Comment by @konstin on 2023-12-14 19:17_

Rebased onto main. I've started using `VerbatimUrl` for editables too, though not in path parsing yet.

---

_@konstin reviewed on 2023-12-14 19:22_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_sync.rs`:2135 on 2023-12-14 19:22_

I believe it would be better to show the resolved absolute path here instead of the original, it helps with debugging.

---

_@charliermarsh reviewed on 2023-12-14 19:48_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_sync.rs`:2135 on 2023-12-14 19:48_

That's reasonable but it'll require at least one new abstraction since this uses `Display`, and `Display` for `VerbatimUrl` uses the given string. So we need another trait.

---

_@konstin reviewed on 2023-12-14 20:53_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_sync.rs`:2135 on 2023-12-14 20:53_

What about `original() -> str` and `resolved() -> &str` in dist? They feel like `Debug` and `Display` but i wouldn't want to use `Debug` for either.

---

_@charliermarsh reviewed on 2023-12-14 20:54_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_sync.rs`:2135 on 2023-12-14 20:54_

I would call `original` either `verbatim` or `given` but it sounds roughly correct yes. The problem is that these are implements on the distribution types (like `SourceDist`), so we need a separate method on that entire hierarchy.

---

_@konstin reviewed on 2023-12-14 20:57_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_sync.rs`:2135 on 2023-12-14 20:57_

Will do in a follow up PR

---

_Review requested from @charliermarsh by @konstin on 2023-12-15 08:39_

---

_@charliermarsh reviewed on 2023-12-16 21:24_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_install.rs`:235 on 2023-12-16 21:24_

So this doesn't support editables yet? Should there be a TODO?

---

_@charliermarsh reviewed on 2023-12-16 21:25_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/reporters.rs`:268 on 2023-12-16 21:25_

I think this is still incorrect.

---

_@charliermarsh reviewed on 2023-12-16 21:27_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:238 on 2023-12-16 21:27_

I believe the clones and change to take an owned reference here can and should be avoided.

---

_@charliermarsh reviewed on 2023-12-16 21:30_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:194 on 2023-12-16 21:30_

I know it's crazy, but is it guaranteed that the build requirements don't include an editable package? Right now, we seem to assume so.

---

_Comment by @charliermarsh on 2023-12-16 21:31_

If I find time, I'll try to handle these last few followups and merge since this keeps hitting conflicts :)

---

_@charliermarsh reviewed on 2023-12-16 22:00_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:2590 on 2023-12-16 22:00_

```
          5 │+error: Failed to build editables␊
          6 │+  Caused by: Failed to build editable: ../../scripts/editable-installs/maturin_editable␊
          7 │+  Caused by: Failed to build: ../../scripts/editable-installs/maturin_editable␊
          8 │+  Caused by: Build backend failed to build wheel through `build_editable()`:␊
          9 │+--- stdout:␊
         10 │+Running `maturin pep517 build-wheel -i /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpM0ztUQ/.venv/bin/python --compatibility off --editable`␊
         11 │+--- stderr:␊
         12 │+🍹 Building a mixed python/rust project␊
         13 │+🔗 Found pyo3 bindings␊
         14 │+🐍 Found CPython 3.12 at /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpM0ztUQ/.venv/bin/python␊
         15 │+📡 Using build options features from pyproject.toml␊
         16 │+   Compiling target-lexicon v0.12.12␊
         17 │+   Compiling autocfg v1.1.0␊
         18 │+   Compiling proc-macro2 v1.0.70␊
         19 │+   Compiling once_cell v1.18.0␊
         20 │+   Compiling libc v0.2.150␊
         21 │+   Compiling unicode-ident v1.0.12␊
         22 │+   Compiling parking_lot_core v0.9.9␊
         23 │+   Compiling heck v0.4.1␊
         24 │+   Compiling cfg-if v1.0.0␊
         25 │+   Compiling scopeguard v1.2.0␊
         26 │+   Compiling smallvec v1.11.2␊
         27 │+   Compiling unindent v0.2.3␊
         28 │+   Compiling indoc v2.0.4␊
         29 │+   Compiling lock_api v0.4.11␊
         30 │+   Compiling memoffset v0.9.0␊
         31 │+   Compiling quote v1.0.33␊
         32 │+   Compiling syn v2.0.39␊
         33 │+   Compiling pyo3-build-config v0.20.0␊
         34 │+   Compiling parking_lot v0.12.1␊
         35 │+   Compiling pyo3-ffi v0.20.0␊
         36 │+   Compiling pyo3 v0.20.0␊
         37 │+   Compiling pyo3-macros-backend v0.20.0␊
         38 │+   Compiling pyo3-macros v0.20.0␊
         39 │+   Compiling maturin_editable v0.1.0 (/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable)␊
         40 │+error: linking with `clang` failed: exit status: 1␊
         41 │+  |␊
         42 │+  = note: env -u IPHONEOS_DEPLOYMENT_TARGET -u TVOS_DEPLOYMENT_TARGET LC_ALL="C" PATH="/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/bin:/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpM0ztUQ/.venv/bin:/Users/crmarsh/.local/share/rtx/installs/python/3.10.13/bin:/Users/crmarsh/workspace/puffin/target/release:/Users/crmarsh/.bun/bin:/opt/homebrew/sbin:/opt/homebrew/bin:/Library/Frameworks/Python.framework/Versions/3.7/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/crmarsh/.cargo/bin:/Users/crmarsh/.deno/bin:/Users/crmarsh/.poetry/bin:/Users/crmarsh/.pyenv/bin:/Users/crmarsh/bin:/opt/homebrew/bin:/Users/crmarsh/.local/bin:/Users/crmarsh/.antigen/bundles/agkozak/zsh-z:/Users/crmarsh/.antigen/bundles/zsh-users/zsh-syntax-highlighting:/Users/crmarsh/.antigen/bundles/zsh-users/zsh-autosuggestions:/Users/crmarsh/.local/bin" VSLANG="1033" ZERO_AR_DATE="1" "clang" "-Wl,-exported_symbols_list,/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/rustc0PS1h0/list" "-arch" "arm64" "/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/rustc0PS1h0/symbols.o" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/maturin_editable.maturin_editable.91ec885a940e6035-cgu.0.rcgu.o" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/maturin_editable.maturin_editable.91ec885a940e6035-cgu.1.rcgu.o" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/maturin_editable.maturin_editable.91ec885a940e6035-cgu.2.rcgu.o" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/maturin_editable.4a42g8ucu8w57flt.rcgu.o" "-L" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps" "-L" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libpyo3-23ac21fa9877a14c.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libmemoffset-fd46ffa8dc916e95.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libparking_lot-c122df108af6f399.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libparking_lot_core-5995d18fcd96ee3c.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libcfg_if-52840a58dc691991.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libsmallvec-00ff269f207bba05.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/liblock_api-f71e1f3e27acdcca.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libscopeguard-edcdd31530886fe0.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libpyo3_ffi-a83a66c6dcf71c21.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/liblibc-137b43a52fa183f9.rlib" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libunindent-f17976f67c75cbf5.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libstd-5563368f93f04a18.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libpanic_unwind-56e96ebffd3d9808.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libobject-68ad5facd2da3c54.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libmemchr-ed648c021defb5b4.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libaddr2line-815db56da00be265.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libgimli-5186709c031b65af.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/librustc_demangle-7cb2a31ae866e369.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libstd_detect-c39e8cee81fb9ad0.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libhashbrown-b8aeb6382a15b7e5.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/librustc_std_workspace_alloc-152de6c346c443c1.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libminiz_oxide-274e1083efe4f227.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libadler-519dc439ccb69841.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libunwind-24c437e0616b2003.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libcfg_if-70eb1def4bb8ab07.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/liblibc-9c748d96a757609c.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/liballoc-7543628317133907.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/librustc_std_workspace_core-8af68f47e6f26d40.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libcore-a60a966a64bff48d.rlib" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libcompiler_builtins-eeccd9f755247d6f.rlib" "-liconv" "-lSystem" "-lc" "-lm" "-L" "/Users/crmarsh/.rustup/toolchains/1.74-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib" "-o" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/target/release/deps/libmaturin_editable.dylib" "-Wl,-dead_strip" "-dynamiclib" "-Wl,-dylib" "-nodefaultlibs" "-undefined" "dynamic_lookup" "-Wl,-install_name,@rpath/maturin_editable.cpython-312-darwin.so" "-fuse-ld=/Users/crmarsh/workspace/sold/build/ld64"␊
         43 │+  = note: mold: fatal: unknown command line option: -undefined␊
         44 │+          clang: error: linker command failed with exit code 1 (use -v to see invocation)␊
         45 │+          ␊
         46 │+␊
         47 │+error: could not compile `maturin_editable` (lib) due to previous error␊
         48 │+💥 maturin failed␊
         49 │+  Caused by: Failed to build a native library through cargo␊
         50 │+  Caused by: Cargo build finished with "exit status: 101": `CARGO_ENCODED_RUSTFLAGS="-C\u{1f}link-arg=-fuse-ld=/Users/crmarsh/workspace/sold/build/ld64" PYO3_ENVIRONMENT_SIGNATURE="cpython-3.12-64bit" PYO3_PYTHON="/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpM0ztUQ/.venv/bin/python" PYTHON_SYS_EXECUTABLE="/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpM0ztUQ/.venv/bin/python" "cargo" "rustc" "--features" "pyo3/extension-module" "--message-format" "json-render-diagnostics" "--manifest-path" "/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/Cargo.toml" "--release" "--lib" "--" "-C" "link-arg=-undefined" "-C" "link-arg=dynamic_lookup" "-C" "link-args=-Wl,-install_name,@rpath/maturin_editable.cpython-312-darwin.so"`␊
         51 │+Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpM0ztUQ/.venv/bin/python', '--compatibility', 'off', '--editable'] returned non-zero exit status 1␊
         52 │+---␊
```

---

_@charliermarsh reviewed on 2023-12-16 22:29_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:232 on 2023-12-16 22:29_

Isn't the reuse of the `Downloader` across invocations with text in-between going to mess with the progress bar?

---

_Comment by @charliermarsh on 2023-12-16 22:34_

Okay, I may push some small follow-ups too but it seems best to get this merged. Let's go!

---

_@charliermarsh reviewed on 2023-12-16 22:34_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:2590 on 2023-12-16 22:34_

This I do need to figure out... blerg.

---

_Merged by @charliermarsh on 2023-12-16 22:37_

---

_Closed by @charliermarsh on 2023-12-16 22:37_

---

_Branch deleted on 2023-12-16 22:37_

---

_@charliermarsh reviewed on 2023-12-16 22:38_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:2590 on 2023-12-16 22:38_

It works without mold (sigh). Not sure why?

---

_@konstin reviewed on 2023-12-17 04:10_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:2590 on 2023-12-17 04:10_

The undefined is a build option we have to set on macos for pyo3 or extension-modules will not compile, afaik because they use libpython symbols while not linking against libpython.

Can you try a build with maturin on the cli? This looks like it could be a bug in maturin or pyo3.

---

_@konstin reviewed on 2023-12-17 04:14_

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:194 on 2023-12-17 04:14_

Yes, the build requirements are pep 508 `Requirement`s which don't have an editable variant.

---

_Comment by @konstin on 2023-12-17 04:15_

Thank you for finishing this!

---

_@charliermarsh reviewed on 2023-12-17 04:19_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:194 on 2023-12-17 04:19_

Phew!

---
