```yaml
number: 11779
title: "[red-knot] Include vendored typeshed stubs as a zipfile in the Ruff binary"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: zip-typeshed
created_at: 2024-06-06T19:31:57Z
updated_at: 2024-06-07T15:16:19Z
url: https://github.com/astral-sh/ruff/pull/11779
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] Include vendored typeshed stubs as a zipfile in the Ruff binary

---

_@AlexWaygood_

## Summary

Work towards https://github.com/astral-sh/ruff/issues/11653.

This PR adds a `build.rs` script to the `red_knot` crate that compresses our vendored typeshed stubs into a zip that is then included in the Ruff binary via `include_bytes!()` in `module.rs`. This will enable us to resolve modules to vendored stdlib stubs from typeshed in the future.

Details:
- The `build.rs` script is run before anything else in the crate is built
- We print a `cargo:rerun-if-changed` directive to stdout to let Cargo know that the `build.rs` only needs to be rerun if the contents of `crates/red_knot/vendor/typeshed` changes, or if the `build.rs` script itself changes. Docs here: https://doc.rust-lang.org/cargo/reference/build-scripts.html#rerun-if-changed. We need to use a syntax for this directive that's officially deprecated, due to the fact that our pinned minimum Rust version is old enough that it doesn't support the new version. (The deprecated syntax is `cargo:rerun-if-changed=`, with one colon. The new syntax uses `cargo::rerun-if-changed=`, with two colons.)
- The zipfile that is created during the build step is `.gitignored`. Unfortunately there will be a need to manually keep that `.gitignore` entry in sync with the hardcoded path in `build.rs`.
- The PR adds a new Ruff dependency on the `zip` crate. Following the precedent in https://github.com/astral-sh/uv/issues/3642, I pinned to an old version of the `zip` crate, and added a Renovate rule to make sure that we don't get dependency-update PRs for that crate.

## Test Plan

Some basic tests have been added that show that the zip archive containing typeshed is available from the Ruff after it has been built, and that the zip archive has some stdlib stubs in it.

---

_Review requested from @carljm by @AlexWaygood on 2024-06-06 19:31_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-06 19:31_

---

_Label `red-knot` added by @AlexWaygood on 2024-06-06 19:32_

---

_@carljm approved on 2024-06-06 19:40_

---

_Comment by @carljm on 2024-06-06 19:41_

Looks good! Did you (manually) verify that changing a file in the vendored typeshed directory triggers a rebuild of the zip and the changed contents show up in the binary? (Eg delete `update_wrapper` from the functools stub and see the added test fail.)

---

_Comment by @AlexWaygood on 2024-06-06 19:50_

> Looks good! Did you (manually) verify that changing a file in the vendored typeshed directory triggers a rebuild of the zip and the changed contents show up in the binary? (Eg delete `update_wrapper` from the functools stub and see the added test fail.)

Tried that out, and it looks good!

![image](https://github.com/astral-sh/ruff/assets/66076021/90cd58b2-4cae-4050-9114-c0cdea5e5d18)

Now to make it work on Windows...

---

_Comment by @github-actions[bot] on 2024-06-06 19:53_

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

_Comment by @carljm on 2024-06-06 20:35_

🎉 

---

_Review comment by @MichaReiser on `.github/renovate.json5`:47 on 2024-06-07 06:43_

Can you add a note on that issue to mention that `red_knot` now also uses the `zip` crate. Or maybe this is something we can discuss in today's standup? The main concern seems to be with 0.66 and 1 but we're now at version 2 already. 

---

_Review comment by @MichaReiser on `.gitignore`:220 on 2024-06-07 06:44_

Can we create the `zip` in the `target` directory where cargo stores all its build artifacts? This way, we can avoid adding an additional ignore, and it also makes it clear that this is a build-time-created artifact.

---

_Review comment by @MichaReiser on `Cargo.lock`:3626 on 2024-06-07 06:46_

Uff, so many new dependencies. Would you mind having a look at https://docs.rs/crate/zip/0.6.6/features and removing all features that we don't need? We probably only need a very specific subset 

---

_Review comment by @MichaReiser on `crates/red_knot/build.rs`:81 on 2024-06-07 06:49_

I don't think we want to filter here. Instead we want to error if we can't read a file. It could otherwise happen that we don't include a file because of a read error and we would only learn about it post release.

I think I would also move the `WalkDir` call into `zip_dir` and instead change `zip_dir` to take a path as an argument. That removes the ugly `impl Iterator` ;)

---

_Review comment by @MichaReiser on `crates/red_knot/build.rs`:51 on 2024-06-07 06:50_

Can we remove the println statements? Especially if they clutter the CLI output

---

_Review comment by @MichaReiser on `crates/red_knot/build.rs`:56 on 2024-06-07 06:54_

I wonder if you could use `std::io::copy` here, assuming that `f` implements `Read` and `zip` implements `Write`

https://doc.rust-lang.org/std/io/fn.copy.html

---

_Review comment by @MichaReiser on `crates/red_knot/build.rs`:42 on 2024-06-07 06:55_

Maybe rename `name` to `relative_path`. Because I think it isn't just the file's name, it's actually the file paths, just relative to the `root` directory.

---

_Review comment by @MichaReiser on `crates/red_knot/build.rs`:67 on 2024-06-07 06:56_

```suggestion
    zip.finish()
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:20 on 2024-06-07 06:57_

I recommend to move the const to the `tests` module for now so that you don't need the `dead_code` suppression

---

_@MichaReiser reviewed on 2024-06-07 06:57_

Nice work. I've some questions before approving

* What's the binary size increase? 
* What's the build time increase on an incremental (change to a rust file) and clean build. You can get the timings with `cargo build --timings -p red_knot`
* Did you open a created archive. Does it look correct?

---

_@AlexWaygood reviewed on 2024-06-07 07:05_

---

_Review comment by @AlexWaygood on `crates/red_knot/build.rs`:81 on 2024-06-07 07:05_

> I don't think we want to filter here.

Yeah, I wondered about that... In the end I guess I was lazy and didn't make too many changes to the recipe Carl and I found at https://github.com/zip-rs/zip-old/blob/5d0f198124946b7be4e5969719a7f29f363118cd/examples/write_dir.rs. But this makes sense; I'll make the change.

---

_@AlexWaygood reviewed on 2024-06-07 09:25_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:47 on 2024-06-07 09:25_

Left a note in https://github.com/astral-sh/uv/issues/3642#issuecomment-2154452853

---

_Review comment by @AlexWaygood on `crates/red_knot/build.rs`:56 on 2024-06-07 09:40_

Nice, thank you!

---

_@AlexWaygood reviewed on 2024-06-07 09:40_

---

_@AlexWaygood reviewed on 2024-06-07 10:22_

---

_Review comment by @AlexWaygood on `.gitignore`:220 on 2024-06-07 10:22_

To clarify: do you mean `$ROOT/target` (which already exists), or do you mean creating a new `$ROOT/crates/red_knot/target` directory for redknot-specific build artifacts? (I'm happy to do either)

---

_@AlexWaygood reviewed on 2024-06-07 10:24_

---

_Review comment by @AlexWaygood on `crates/red_knot/build.rs`:51 on 2024-06-07 10:24_

Because this is a build script, these aren't actually printed to stdout during the build process unless the build actually fails -- it's more like logging, really. But if the build fails, then the entire captured stdout from the build is printed in a summary, e.g.

![image](https://github.com/astral-sh/ruff/assets/66076021/45b61f59-998b-4460-a2be-4bf154d3ed5f)


---

_@AlexWaygood reviewed on 2024-06-07 10:29_

---

_Review comment by @AlexWaygood on `.gitignore`:220 on 2024-06-07 10:29_

Reading through https://github.com/rust-lang/cargo/issues/9661, it sounds like getting the location of the `target` directory reliably from a build script is a deliberately unsolved problem, and that build scripts are not meant to put artifacts there. https://github.com/rust-lang/cargo/issues/9661 says:

> `OUT_DIR` — If the package has a build script, this is set to the folder where the build script should place its output. See below for more information. (Only set during compilation.)

That implies to me that I should be putting the zip there, rather than where I have it in this PR currently or in `target/`

---

_Comment by @AlexWaygood on 2024-06-07 11:48_

> * What's the binary size increase?

On `main`, after running `cargo clean && cargo build --all-features`:
- The executable created for `red_knot` is 9.565 MB
- The executable created for `ruff` is 77.481 MB

With this PR branch:
- The executable created for `red_knot` is 9.565 MB
- The executable created for `ruff` is 77.487 MB

The `TYPESHED_ZIP_BYTES` array in `module.rs` appears to be of length 617,342.

> * What's the build time increase on an incremental (change to a rust file) and clean build. You can get the timings with `cargo build --timings -p red_knot`

On `main`, `cargo clean && cargo build --timings -p red_knot` reports the total time as being 9.7s. With this PR branch, it increases to 10.5s. The time taken for an incremental build after a change to a Rust file (that isn't `build.rs`) seems noisy/unchanged -- it took 0.95s for me on `main` and 0.81s on this PR branch.

---

_Comment by @AlexWaygood on 2024-06-07 12:17_

> Did you open a created archive. Does it look correct?

We're definitely able to inspect the contents of files in the zip at runtime using the `zip` crate that we used to create the zip. From what I can tell, everything is as we'd expect it to be. I assert as such in the tests I added, and the tests correctly fail if I change the contents of the vendored `stdlib/functools.pyi` file.

I also managed to inspect the filenames in the zip using Python's `zipfile` module, and everything was as I would expect it to be. (Except that on my machine, the zip includes a `.DS_STORE` file... but probably not a huge issue, and unlikely to appear in the binaries we publish to PyPI.)

I've been struggling to extract the zip archive or inspect the contents of any of the files in the zip from outside Rust -- most tools for unzipping zip archives don't seem to expect the `zstd` algorithm to be used, and I havne't succeeded in using `zstandard` tools to extract the zip either. I don't know if that's a concern or not.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-07 12:17_

---

_@MichaReiser reviewed on 2024-06-07 12:27_

---

_Review comment by @MichaReiser on `.gitignore`:220 on 2024-06-07 12:27_

Yeah sorry, I'm not very familiar with build scripts. Out dir seems to be what I was looking for.

---

_Comment by @MichaReiser on 2024-06-07 12:29_

> What's the binary size increase?

Sorry, I should have been more explicit. Can you compare the release build numbers. 

Also, The red knot binary size is identical. I think the typeshed files should be larger than 0 bytes? Ohhh, maybe the compiler optimizes them away (or isn't part of the executable anymore if you moved it to tests)


---

_Comment by @AlexWaygood on 2024-06-07 12:31_

> Also, The red knot binary size is identical. I think the typeshed files should be larger than 0 bytes? Ohhh, maybe the compiler optimizes them away (or isn't part of the executable anymore if you moved it to tests)

Yeah, this confused me as well, since the length of the `TYPESHED_ZIP_BYTES` array is definitely not zero. I deliberately passed `--tests` so I think it should be included in the binary. Not sure.

> Sorry, I should have been more explicit. Can you compare the release build numbers.

No worries, I'll do that!

---

_@MichaReiser approved on 2024-06-07 13:00_

---

_Comment by @AlexWaygood on 2024-06-07 14:11_

With `cargo build --release --all-features --all-targets`.

`main`:
- `red_knot` binary: 3.221 MB
- `ruff` binary: 25.195 MB

PR branch:
- `red_knot` binary: 3.221 MB
- `ruff` binary: 25.196 MB

So, still not showing up in the size of the binary (even though it's definitely compiling all the test features as part of that command.

Still, we know how large the zip is from this:

> The `TYPESHED_ZIP_BYTES` array in `module.rs` appears to be of length 617,342.

---

_Comment by @AlexWaygood on 2024-06-07 14:29_

In https://github.com/astral-sh/ruff/pull/11779/commits/c5ad832102cc9cd16da3c8035c0fb9d413da6890, I switched to setting the location of the zip (relative to cargo's `OUT_DIR` environment variable) via an environment variable set at compile time in `.cargo/config.toml`. This feels slightly nicer than having to hardcode the location manually in both `crates/red_knot/build.rs` and `crates/red_knot/src/module.rs`. @MichaReiser, are you okay with that change?

---

_Comment by @MichaReiser on 2024-06-07 14:49_

@AlexWaygood what's the benefit of the environment variable for the name of the zip? It feels like unnecessary complexity to me, considering that we don't have a use case for a different name. 

If it's just to avoid the path repetition, then I think adding a comment to `build.rs` or the other way around would be sufficient.

Edit: Like I'm not against it, but it makes the code harder to follow. I then need to search for where the environment variable comes from and understand if there's actually a use case where we want to change the path. Having it hard coded makes it very explicit and failing to update the path in one file will also give you compile errors right away.

---

_Comment by @AlexWaygood on 2024-06-07 14:52_

Yes, it was just to avoid the repetition, and to make it so that there was a "single source of truth" for the location. It felt more "principled" to me. But I also wasn't sure if it was worth the new complexity, so I'll revert it if you find it less readable 👍

---

_Comment by @MichaReiser on 2024-06-07 14:57_

> Yes, it was just to avoid the repetition, and to make it so that there was a "single source of truth" for the location. It felt more "principled" to me. But I also wasn't sure if it was worth the new complexity, so I'll revert it if you find it less readable

I think it's just slightly over-engineered ;)

---

_Comment by @AlexWaygood on 2024-06-07 14:57_

Thanks both for the reviews!

---

_Merged by @AlexWaygood on 2024-06-07 15:00_

---

_Closed by @AlexWaygood on 2024-06-07 15:00_

---

_Branch deleted on 2024-06-07 15:00_

---
