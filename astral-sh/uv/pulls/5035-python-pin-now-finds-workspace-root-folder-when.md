```yaml
number: 5035
title: "`python pin` now finds workspace root folder when called in subfolder"
type: pull_request
state: closed
author: silvanocerza
labels: []
assignees: []
base: main
head: python-pin-workspace-subfolder
created_at: 2024-07-13T09:23:07Z
updated_at: 2024-09-16T22:21:48Z
url: https://github.com/astral-sh/uv/pull/5035
synced_at: 2026-01-10T12:53:31Z
```

# `python pin` now finds workspace root folder when called in subfolder

---

_Pull request opened by @silvanocerza on 2024-07-13 09:23_

## Summary

Fixes #4971.

This PR changes `python pin` behaviour when called in a workspace subfolder. Previously it would only read or create a `.python-version` file in the current working directory, now instead it will first search for the workspace root folder and read or create the `.python-version` file in that folder. 

We assume the root folder contains a valid `pyproject.toml` file.

If we're not in a workspace use the previous behaviour and creates or reads `.python-version` in the current working directory.

## Test Plan

I added a new test to verify this new behaviour, and run all tests locally.


---

_Comment by @zanieb on 2024-07-13 15:38_

We might want to make this asymmetric per https://github.com/astral-sh/uv/issues/4971#issuecomment-2226958546 and _read_ from the workspace root by default but, when _writing_, require an opt-in via  `--workspace` otherwise we write to the current directory. Or should we require an opt-out via `--isolated` to ignore the workspace and write to the working directory? What do you think? I'm not 100% certain what the best UX is yet.

---

_Comment by @T-256 on 2024-07-13 15:54_

> Or should we use `--isolated` to ignore the workspace and write to the working directory?

IMO this is more correct to me, since pinned python version is usually used for venv creation/recreation and venv is in workspace root.


---

_Comment by @silvanocerza on 2024-07-13 17:36_

I would agree with @T-256 here, it makes sense to me using the `--isolated` option to ignore the workspace. 
Should I got with that?

---

_Comment by @silvanocerza on 2024-07-14 15:08_

Rebased to bring in CI fix from #5034 and remove some commented out code that I missed.

---

_Assigned to @zanieb by @zanieb on 2024-07-14 15:29_

---

_@zanieb reviewed on 2024-07-14 16:18_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:242 on 2024-07-14 16:18_

Can we add a message noting where we pinned when it differs from the pwd?

---

_@zanieb reviewed on 2024-07-14 16:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:32 on 2024-07-14 16:18_

We may want to call this `target_dir` or something to avoid confusion with the current working directory.

---

_@zanieb reviewed on 2024-07-14 16:19_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:105 on 2024-07-14 16:19_

Why this change?

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:71 on 2024-07-14 16:19_

Should we just use `Path::join` here?

---

_@zanieb reviewed on 2024-07-14 16:19_

---

_@zanieb reviewed on 2024-07-14 16:20_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:69 on 2024-07-14 16:20_

This can be `impl AsRef<Path>` or `&Path` instead of `&PathBuf`

---

_@zanieb reviewed on 2024-07-14 16:20_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:105 on 2024-07-14 16:20_

Is this supposed to be `requests_from_versions_file`?

---

_@zanieb reviewed on 2024-07-14 16:20_

---

_Review comment by @zanieb on `crates/uv-python/src/version_files.rs`:54 on 2024-07-14 16:20_

Can you add some simple docstrings to these?

---

_Comment by @zanieb on 2024-07-14 16:22_

Sure let's just make `--isolated` ignore the workspace to start and we can tackle more complex behavior separately?

---

_Review comment by @silvanocerza on `crates/uv-python/src/version_files.rs`:105 on 2024-07-15 08:36_

Yes, the docstring was wrong, `requests_from_version_files` doesn't exist.

---

_@silvanocerza reviewed on 2024-07-15 08:36_

---

_Comment by @silvanocerza on 2024-07-15 15:04_

Ready for review again. 👍 

---

_@silvanocerza reviewed on 2024-07-15 15:06_

---

_Review comment by @silvanocerza on `crates/uv/src/commands/python/pin.rs`:98 on 2024-07-15 15:06_

I'm not sure about this part, would something like this be better? 🤔 

```
    let message = if exists {
        format!("Replaced existing pin with `{output}`")
    } else {
        format!("Pinned to `{output}`")
    };

    let message = if target_dir != std::env::current_dir()? {
        // Print the version file use to pin only
        // if it's not in the current working directory
        format!("{message} in `{}`", version_file.display())
    } else {
        message
    };

    writeln!(printer.stdout(), "{message}")?;
```

---

_Comment by @zanieb on 2024-07-19 18:12_

Going to be lots of conflicts with #5215 and https://github.com/astral-sh/uv/pull/4989 — I'll own resolving them.

---

_Comment by @zanieb on 2024-07-19 19:14_

Okay I resolved the conflicts but then realized we need to make a lot of other changes here, e.g. every other command needs to respect pins in the workspace. Kind of complicated, I need to think about it before moving forward.

---

_Comment by @zanieb on 2024-07-19 19:44_

Here's where I left off https://github.com/astral-sh/uv/compare/zb/pin-find

---

_Comment by @silvanocerza on 2024-07-20 08:45_

If you can give me a list of the commands that actually needs to respect the pin I should be able to handle that, I'm just unsure about all the commands.

Also probably we need to isolate the logic that finds the pin location, it would simplify implementing it for the other commands.

---

_Comment by @zanieb on 2024-07-20 16:10_

I think we need to walk up the directory tree looking for a version file like we do with `pyproject.toml` files for project roots. Then, every command that needs a Python interpreter needs to respect the version file when determining the default. We don't need to do it all at once, I think outside the preview commands only `venv` supports version files right now and it's gated by preview.

We may or may not want to stop searching at a boundary where we find a `pyproject.toml`. If we don't, then we need to check if the version is pinned to something incompatible with the current project's Python requirement. If the pin comes from a directory above the project, then we probably need to warn and use the `Requires-Python` value instead. 

It seems simplest to do something like:

- Add a utility to the version file module that finds the nearest version file. We probably want a struct for this so we can store the path as state and allow read / write.
- Add a utility to the workspace / project abstractions to reconcile the discovered version file with the project, e.g. we can check if it's location is a prefix of the project or if it's inside the project and check compatibility with the project Python requirement.
- Use in `uv venv` under preview mode
- Use in all the new preview commands where we currently respect version files
- Add a tracking issue enumerating the remaining commands

How does that sound? Kind of figuring this out as I go :)

---

_Comment by @zanieb on 2024-09-16 22:21_

I'm taking this on in https://github.com/astral-sh/uv/pull/6370

---

_Closed by @zanieb on 2024-09-16 22:21_

---
