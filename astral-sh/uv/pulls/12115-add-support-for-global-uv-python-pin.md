```yaml
number: 12115
title: "Add support for global `uv python pin`"
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
assignees: []
merged: true
base: main
head: jtfm/python-pin-global
created_at: 2025-03-11T14:52:22Z
updated_at: 2025-03-13T12:48:39Z
url: https://github.com/astral-sh/uv/pull/12115
synced_at: 2026-01-12T16:10:08Z
```

# Add support for global `uv python pin`

---

_@jtfmumm_

These changes add support for

```
uv python pin 3.12 --global 
```

This adds the specified version to a `.python-version` file in the user-level config directory. uv will now use the user-level version as a fallback if no version is found in the project directory or its ancestors.

Open question: is `--global` the correct parameter here? `--user` is more precise, but might not communicate the intent as clearly.

TODO: User documentation.

Closes #4972


---

_Review requested from @zanieb by @jtfmumm on 2025-03-11 14:52_

---

_@zanieb reviewed on 2025-03-11 17:41_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:620 on 2025-03-11 17:41_

Should we just set `USERPROFILE` to `self.home_dir` instead? Do we need to set `XDG_CONFIG_HOME` specifically?

---

_@zanieb reviewed on 2025-03-11 17:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_pin.rs`:207 on 2025-03-11 17:42_

Why do we need to create and set this if you're setting `XDG_CONFIG_HOME` in the test context? Can't we just pull the directory from there? (maybe you just added that after?)

It's important that the temporary directories use the context's root temporary directory — we want to avoid creating temporary directories in arbitrary locations during tests.

---

_@zanieb reviewed on 2025-03-11 17:44_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_pin.rs`:237 on 2025-03-11 17:44_

Should this just be a test case in the subsequent test? The above "No pinned Python version found" case is duplicated and this case seems testable there?

---

_@zanieb reviewed on 2025-03-11 17:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_pin.rs`:287 on 2025-03-11 17:45_

We might want to assert specific file contents after this snapshot? Like the global one is unchanged and the local one is present?

---

_@zanieb reviewed on 2025-03-11 17:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:243 on 2025-03-11 17:47_

We probably don't need repeated coverage for things like this here, we just need to test that the Python version is respected and this adds a lot of noise.

---

_@zanieb reviewed on 2025-03-11 17:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:272 on 2025-03-11 17:48_

Similarly, we probably don't need to install multiple tools

---

_@zanieb reviewed on 2025-03-11 17:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:260 on 2025-03-11 17:48_

We don't capture the Python version in the receipt. This is actually relevant. What happens if the global Python version changes then you reinstall or upgrade? Test coverage for that would good. I think it's fine to respect the changed global default, personally.

---

_@zanieb reviewed on 2025-03-11 17:50_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:94 on 2025-03-11 17:50_

This doesn't need to be `pub`, does it?

---

_@zanieb reviewed on 2025-03-11 17:56_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4681 on 2025-03-11 17:56_

Can you adjust this to match the wording and style of the top-level pin description?

https://github.com/astral-sh/uv/blob/7340ff72daca2e837b3383dcd861e63f9c77a477/crates/uv-cli/src/lib.rs#L4425-L4433

I might say something like..

> Update the global Python version pin.
>
> Writes the pinned Python version to a `.python-version` file in the `XDG_CONFIG_HOME` instead of the working directory.
>
> When a local Python version pin is not found in the working directory or an ancestor directory, this version will be used instead.
>
> Unlike local version pins, this version is used as the default for commands that mutate global state, like `uv tool install`.

The line wrap here also doesn't match the rest of the docs.


---

_@zanieb reviewed on 2025-03-11 17:57_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4681 on 2025-03-11 17:57_

(my doc might need an update to account for platform differences)

---

_@zanieb reviewed on 2025-03-11 17:59_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:75 on 2025-03-11 17:59_

I wonder if we should co-locate this with the `uv.toml`? Like, if https://github.com/astral-sh/uv/blob/ba74b9ea93b676e7d0b84c386b87e69017a4c27b/crates/uv-dirs/src/lib.rs#L140 exists and a user-level config does not should we prefer that? I'm leaning away from it right now, but wanted to put the idea out there. Maybe can can consider a `--system` flag separately in the future once we have more feedback.

---

_Comment by @samypr100 on 2025-03-12 02:05_

> These changes add support for
> 
> ```
> uv python pin 3.12 --global 
> ```
> 
> This adds the specified version to a `.python-version` file in the user-level config directory. uv will now use the user-level version as a fallback if no version is found in the project directory or its ancestors.
> 
> Open question: is `--global` the correct parameter here? `--user` is more precise, but might not communicate the intent as clearly.
> 
> TODO: User documentation.
> 
> Closes #4972

I do lean towards `--global`. For example, git uses `--global`, `--local`, and `--system`.

---

_@jtfmumm reviewed on 2025-03-12 09:36_

---

_Review comment by @jtfmumm on `crates/uv-python/src/version_files.rs`:94 on 2025-03-12 09:36_

It's used in `pin.rs` in `uv`.

---

_@jtfmumm reviewed on 2025-03-12 13:42_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/common/mod.rs`:620 on 2025-03-12 13:42_

No we don't. Updated.

---

_@jtfmumm reviewed on 2025-03-12 13:42_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_pin.rs`:207 on 2025-03-12 13:42_

Yeah this came first. Updated.

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_pin.rs`:237 on 2025-03-12 13:43_

Cleaned these tests up to narrow what each one does.

---

_@jtfmumm reviewed on 2025-03-12 13:43_

---

_@jtfmumm reviewed on 2025-03-12 13:44_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/tool_install.rs`:272 on 2025-03-12 13:44_

Removed one

---

_Label `enhancement` added by @jtfmumm on 2025-03-12 16:33_

---

_@zanieb reviewed on 2025-03-12 17:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4677 on 2025-03-12 17:28_

We try to keep the first line short, because it renders in the short help. We should cover details like `XDG_CONFIG_HOME` in a subsequent line (that's why my suggestion was written that way).

We might want to say "in the uv user configuration directory" then subsequently explain that's `XDG_CONFIG_HOME/uv` and whatever the Windows analogue is.

---

_@zanieb reviewed on 2025-03-12 17:29_

---

_Review comment by @zanieb on `crates/uv-dirs/src/lib.rs`:101 on 2025-03-12 17:29_

Reminder to remove the dbg statements

---

_@zanieb reviewed on 2025-03-12 17:30_

---

_Review comment by @zanieb on `crates/uv-dirs/src/lib.rs`:101 on 2025-03-12 17:30_

(Looks like there's other commented out code so I'll hold off on reviewing more)

---

_@jtfmumm reviewed on 2025-03-12 19:04_

---

_Review comment by @jtfmumm on `crates/uv-dirs/src/lib.rs`:101 on 2025-03-12 19:04_

Removed the commented dbg statements. 

---

_@jtfmumm reviewed on 2025-03-12 19:04_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4677 on 2025-03-12 19:04_

Updated.

---

_@jtfmumm reviewed on 2025-03-12 19:05_

---

_Review comment by @jtfmumm on `crates/uv-python/src/version_files.rs`:75 on 2025-03-12 19:05_

That's an interesting question. But I agree we should consider it separately from this PR.

---

_Review comment by @jtfmumm on `crates/uv/tests/it/tool_install.rs`:243 on 2025-03-12 19:06_

Updated

---

_@jtfmumm reviewed on 2025-03-12 19:06_

---

_@jtfmumm reviewed on 2025-03-12 19:09_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/tool_install.rs`:260 on 2025-03-12 19:09_

I have a FIXME with some commented out new test code in this test for the reinstall case. This test code fails: currently uv doesn't respect the global on reinstall (and just uses the version originally used). Do we want this functionality for this PR?

---

_@jtfmumm reviewed on 2025-03-12 19:10_

---

_Review comment by @jtfmumm on `crates/uv-dirs/src/lib.rs`:101 on 2025-03-12 19:10_

There is still some commented out code under a FIXME. I explain why above (open question about reinstall behavior).

---

_@jtfmumm reviewed on 2025-03-12 19:46_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_pin.rs`:287 on 2025-03-12 19:46_

Added

---

_@zanieb reviewed on 2025-03-12 20:10_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:260 on 2025-03-12 20:10_

Ah okay, thanks for clarifying.

I would expect

- If I requested a version explicitly, e.g., `uv tool install --python <version>` I'd expect it to be recorded in the receipt and reused on reinstall
- If I did not, I would expect the version to change on reinstall if the global default changed

However, I don't feel strongly about the latter point and if it's an entirely different model than what we're doing today then let's just leave it as-is and consider it separately (I'd open an issue to track it).

Either way, test coverage seems great.

---

_@zanieb reviewed on 2025-03-12 20:12_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4680 on 2025-03-12 20:12_

This still needs to explain the behavior of this flag.

Just copying the previous suggestion:

```suggestion
    /// Update the global Python version pin.
```

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4689 on 2025-03-12 20:13_

We wrap commands in backticks

```suggestion
    /// global state, like `uv tool install`.
```

---

_@zanieb reviewed on 2025-03-12 20:13_

---

_@zanieb reviewed on 2025-03-12 20:13_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4682 on 2025-03-12 20:13_

I think we tend to wrap this file name in backticks to

```suggestion
    /// Writes the pinned Python version to a `.python-version` file in the uv user configuration
```

---

_@zanieb reviewed on 2025-03-12 20:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:367 on 2025-03-12 20:16_

Does this mean we'll be missing test coverage for cases where we need to create a file and the parent config directory does not exist yet? Can we skip creating this here?

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:354 on 2025-03-12 20:18_

Needs docs, missing in the generated reference

---

_@zanieb reviewed on 2025-03-12 20:18_

---

_@zanieb reviewed on 2025-03-12 20:19_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_pin.rs`:209 on 2025-03-12 20:19_

```suggestion
    // Without arguments, we attempt to read the current pin (which does not exist yet)
```

---

_@zanieb reviewed on 2025-03-12 20:20_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_pin.rs`:209 on 2025-03-12 20:20_

```suggestion
    // Without arguments, we attempt to read the current pin (which does not exist yet)
```

---

_@zanieb reviewed on 2025-03-12 20:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_pin.rs`:246 on 2025-03-12 20:22_

I'd still just recommend combining this test with the one above and calling it `python_pin_global` since we tend to discuss the specific behavior we are testing in a comment above each invocation. This isn't blocking feedback though

---

_@zanieb approved on 2025-03-12 20:23_

I think this should only require some brief docs in https://docs.astral.sh/uv/concepts/python-versions/#python-version-files

---

_@jtfmumm reviewed on 2025-03-13 12:22_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/common/mod.rs`:367 on 2025-03-13 12:22_

I've removed this

---

_Merged by @jtfmumm on 2025-03-13 12:48_

---

_Closed by @jtfmumm on 2025-03-13 12:48_

---

_Branch deleted on 2025-03-13 12:48_

---
