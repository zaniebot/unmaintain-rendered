```yaml
number: 17054
title: "Error instead of `panic!` when running Ruff from a deleted directory (#16903)"
type: pull_request
state: merged
author: maxmynter
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2025-03-28T23:07:33Z
updated_at: 2025-04-01T12:17:08Z
url: https://github.com/astral-sh/ruff/pull/17054
synced_at: 2026-01-10T19:40:36Z
```

# Error instead of `panic!` when running Ruff from a deleted directory (#16903)

---

_Pull request opened by @maxmynter on 2025-03-28 23:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes #16903

## Summary
Check if the current working directory exist. If not, provide an error instead of panicking.

Fixed a stale comment in `resolve_default_files`.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
I added a test to the `resolve_files.rs`. 

Manual testing follows steps of #16903 :
- Terminal 1
```bash
mkdir tmp
cd tmp
```
- Terminal 2
```bash
rm -rf tmp
```
- Terminal 1
```bash
ruff check
```

## Open Issues / Questions to Reviewer
All tests pass when executed with `cargo nextest run`.

However, with `cargo test` the parallelization makes the other tests fail as we change the `pwd`.

Serial execution with `cargo test` seems to require [another dependency or some workarounds](https://stackoverflow.com/questions/51694017/how-can-i-avoid-running-some-tests-in-parallel).

Do you think an additional dependency or test complexity is worth testing this small edge case, do you have another implementation idea, or should i rather remove the test?
  
---
P.S.: I'm currently participating in a batch at the [Recurse Center](https://www.recurse.com/) and would love to contribute more for the next six weeks to improve my Rust. Let me know if you're open to mentoring/reviewing and/or if you have specific areas where help would be most valued.

---

_Label `bug` added by @ntBre on 2025-03-29 16:41_

---

_Review comment by @ntBre on `crates/ruff/src/resolve.rs`:26 on 2025-03-29 16:46_

I think we could use [`bail!`](https://docs.rs/anyhow/latest/anyhow/macro.bail.html) here instead of `Err(anyhow!(...))`.

I also think we might as well do something like this:

```rust
let Ok(cwd) = std::env::current_dir() else {
    bail!("Working directory does not exist");
};
```

instead of using `path_dedot::CWD` below, which is just a `LazyLock` around `std::env::current_dir`. That makes it a bit more clear what this check is for, I think.

We may also want to move this check into the `if config_arguments.isolated` check if that's the only place `CWD` is used.

---

_Review comment by @ntBre on `crates/ruff/tests/resolve_files.rs`:111 on 2025-03-29 16:59_

nit: I would just use `drop` directly without qualifying it.

---

_Review comment by @ntBre on `crates/ruff/tests/resolve_files.rs`:115 on 2025-03-29 17:00_

I don't think you actually need these filters and could leave them out. Paths only cause problems when they appear in the snapshot output.

---

_@ntBre reviewed on 2025-03-29 17:00_

Thanks for working on this! I took a look at this earlier in the week, and I was wondering how to go about testing it too.

I think if you move the new integration test to its own file, it will be built and run separately by `cargo test` and avoid the parallelization issues you ran into. Then you can also skip saving the `original_dir` and restoring it since the test should be run on its own.

Alternatively, we could possibly skip testing this to avoid changing the cwd at all, which feels a little scary. What do you think @MichaReiser?

---

_Comment by @github-actions[bot] on 2025-03-29 17:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@maxmynter reviewed on 2025-03-30 01:52_

---

_Review comment by @maxmynter on `crates/ruff/tests/resolve_files.rs`:111 on 2025-03-30 01:52_

Agree that it reads nicer. 
I will also just import from `std::env` so we can use directory operations without qualifiers.

---

_@maxmynter reviewed on 2025-03-30 02:30_

---

_Review comment by @maxmynter on `crates/ruff/src/resolve.rs`:26 on 2025-03-30 02:30_

Agree on your proposal for rewriting. It makes the intentions of the check clear and we can get rid of the comment.

`path_dedot::CWD` is used a couple of times throughout the function so I will keep it outside of the if clause.

---

_Comment by @maxmynter on 2025-03-30 03:02_

Having the test in a dedicated file now works for both `cargo test` and `cargo nextest`. 

---

_Review requested from @ntBre by @maxmynter on 2025-03-30 03:03_

---

_Review comment by @MichaReiser on `crates/ruff/src/resolve.rs`:24 on 2025-03-30 12:33_

It feels off that the path isn't used here. I think we should change the code to call `cwd.parse_dot()` and use it instead of `&path_dedot::CWD` below

Edit: This is the same as what @ntBre was proposing

---

_Review comment by @MichaReiser on `crates/ruff/tests/direxist_guard.rs`:21 on 2025-03-30 12:35_

I agree with @ntBre that changing the working directory is problematic. Let's only override the current working directory for the spanned CLI command.

```suggestion
    assert_cmd_snapshot!(Command::new(get_cargo_bin(BIN_NAME)).arg("check").current_dir(&temp_path), @r###"
```

---

_@MichaReiser requested changes on 2025-03-30 12:35_

---

_@ntBre reviewed on 2025-03-30 14:37_

---

_Review comment by @ntBre on `crates/ruff/tests/direxist_guard.rs`:21 on 2025-03-30 14:37_

I think I tried `current_dir` when I looked at this, and it caused a different panic in the `Command` invocation, separate from the one we're trying to test. But it would be good to double check, and maybe I missed something.

---

_@maxmynter reviewed on 2025-03-31 04:33_

---

_Review comment by @maxmynter on `crates/ruff/tests/direxist_guard.rs`:21 on 2025-03-31 04:33_

Yes, the `assert_cmd_snapshot` or `current_dir` calls an `unwrap()` somewhere. The test fails with: 

```bash
[...] called `Result::unwrap()` on an `Err` value: Os { code: 2, kind: NotFound, message: "No such file or directory"
```



---

_@MichaReiser reviewed on 2025-03-31 08:12_

---

_Review comment by @MichaReiser on `crates/ruff/tests/direxist_guard.rs`:21 on 2025-03-31 08:12_

Hmm I see. Let's add some module level documentation (or at least test level) explaining why this test *has to be its own module*. 

---

_Review comment by @MichaReiser on `crates/ruff/src/resolve.rs`:65 on 2025-03-31 08:14_

Is it intentional that you're only using `parse_dot` in some paths? 

I think I'd change the method to 
```rust
    let Ok(cwd) = std::env::current_dir() else {
        bail!("Working directory does not exist")
    };
		let cwd = cwd.parse_dot()?;
```

---

_@MichaReiser reviewed on 2025-03-31 08:14_

---

_@maxmynter reviewed on 2025-03-31 19:47_

---

_Review comment by @maxmynter on `crates/ruff/src/resolve.rs`:65 on 2025-03-31 19:47_

It was born out of me wrestling with the typechecker, using the previous implementation as reference.
I agree that a more consistent, less verbose usage is preferable.


Is the change in 27a8964 what you had in mind? I'm a bit out of my depth with `Cow` and the path handling.

---

_@ntBre reviewed on 2025-03-31 20:03_

---

_Review comment by @ntBre on `crates/ruff/src/resolve.rs`:65 on 2025-03-31 20:03_

That looks right to me. I *think* we could skip calling `parse_dot` at all since `path_dedot::CWD` doesn't:

https://github.com/magiclen/path-dedot/blob/36c197f922d4c8779594ce25b8ffc6cb0315c6a8/src/lib.rs#L329-L330

but I think Micha's point was that we should be consistent one way or the other.

---

_Review requested from @MichaReiser by @maxmynter on 2025-03-31 20:16_

---

_Review comment by @ntBre on `crates/ruff/src/resolve.rs`:67 on 2025-03-31 20:24_

nit: You do need `as_ref` currently, but `stdin_filename` is already an `Option<&Path>`, so you can actually simplify both of these cases (here and below on line 137) to this:

```suggestion
        pyproject::find_settings_toml(stdin_filename.unwrap_or(&cwd))?
```



---

_Review comment by @ntBre on `crates/ruff/tests/direxist_guard.rs`:6 on 2025-03-31 20:27_

I think the Windows CI failure is real. I think Windows will keep the directory around as long as a process is using it. Maybe we need to exclude both `wasm` and `windows` targets? I always forget the syntax for this, but I think it's something like this:

```suggestion
#![cfg(not(any(target_family = "wasm", target_family = "windows")))]
```

---

_@ntBre reviewed on 2025-03-31 20:30_

Thanks for following up on this, it's looking good. Just one more nit to avoid `as_ref` calls and an idea for fixing Windows CI.

I'll defer to Micha for the final approval, but this looks good to me once CI passes.

---

_@maxmynter reviewed on 2025-04-01 03:24_

---

_Review comment by @maxmynter on `crates/ruff/src/resolve.rs`:65 on 2025-04-01 03:24_

Interesting. I used it because the first line on the `path_dedot` crate [docs](https://lib.rs/crates/path-dedot) say it's purpose is to handle filepaths that contain dots. 

But if we only use(d) it as a global variable to access the cwd, I think we can drop it.

Addressed in [98c7711](https://github.com/astral-sh/ruff/pull/17054/commits/98c7711395a68da11b4d93c7c71e6fc0e84f182f)

---

_@maxmynter reviewed on 2025-04-01 03:30_

---

_Review comment by @maxmynter on `crates/ruff/tests/direxist_guard.rs`:6 on 2025-04-01 03:30_

I'll leave the Windows CI failures to you, if that's fine... I don't have access to a Windows machine.

---

_Label `cli` added by @MichaReiser on 2025-04-01 07:12_

---

_@MichaReiser approved on 2025-04-01 12:17_

---

_Merged by @MichaReiser on 2025-04-01 12:17_

---

_Closed by @MichaReiser on 2025-04-01 12:17_

---
