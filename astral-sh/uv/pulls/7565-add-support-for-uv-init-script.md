```yaml
number: 7565
title: "Add support for `uv init --script`"
type: pull_request
state: merged
author: tfsingh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: tfsingh/init-script
created_at: 2024-09-20T00:40:15Z
updated_at: 2024-09-25T23:19:51Z
url: https://github.com/astral-sh/uv/pull/7565
synced_at: 2026-01-12T16:07:53Z
```

# Add support for `uv init --script`

---

_@tfsingh_

This PR adds support for ```uv init --script```, as defined in issue #7402 (started working on this before I saw jbvsmo's PR). Wanted to highlight a few decisions I made that differ from the existing PR:

1. ```--script``` takes a path, instead of a path/name. This potentially leads to a little ambiguity (I can certainly elaborate  in the docs, lmk!), but strictly allowing ```uv init --script path/to/script.py``` felt a little more natural than allowing for ```uv init --script path/to --name script.py``` (which I also thought would prompt more questions for users, such as should the name include the .py extension?)
2. The request is processed immediately in the ```init``` method, sharing logic in resolving which python version to use with ```uv add --script```. This made more sense to me — since scripts are meant to operate in isolation, they shouldn't consider the context of an encompassing package should one exist (I also think this decision makes the relative codepaths for scripts/packages easier to follow).
3. No readme — readme felt a little excessive for a script, but I can of course add it in!

@jbvsmo – hope it's okay I'm using the documentation you wrote!

---

_@tfsingh reviewed on 2024-09-20 00:41_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/init.rs`:650 on 2024-09-20 00:41_

Using try_exists here in line with a comment on the previous PR, but should we potentially use ```fs_err::tokio::metadata``` so it's async? (Although I don't suspect it's an expensive operation)

---

_Marked ready for review by @tfsingh on 2024-09-20 01:14_

---

_Converted to draft by @tfsingh on 2024-09-20 01:37_

---

_Marked ready for review by @tfsingh on 2024-09-20 01:58_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2335 on 2024-09-24 01:59_

```suggestion
    /// A script is a standalone file which adheres to the PEP 723 specification.
```

---

_@charliermarsh reviewed on 2024-09-24 01:59_

---

_@charliermarsh reviewed on 2024-09-24 02:00_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:47 on 2024-09-24 02:00_

A bit more natural as a `let-else`:

```rust
let Some(script) = explicit_path else { anyhow::bail!(...) };
```

---

_@charliermarsh reviewed on 2024-09-24 02:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:644 on 2024-09-24 02:01_

I sort of think it could be okay to accept an existing file. And if the file exists, and doesn't contain a PEP 723 script tag, we add it, but otherwise don't modify the script.

---

_@charliermarsh reviewed on 2024-09-24 02:01_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:118 on 2024-09-24 02:01_

Let's add `-> None` here.

---

_@charliermarsh reviewed on 2024-09-24 02:02_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:98 on 2024-09-24 02:02_

Lets call this `create` and rename the method above to `init`?

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:198 on 2024-09-24 02:03_

Lets include the script path in this error message.

---

_@charliermarsh reviewed on 2024-09-24 02:03_

---

_@charliermarsh reviewed on 2024-09-24 02:03_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:51 on 2024-09-24 02:03_

I'd suggest moving this to `project/mod.rs`. We can also omit `new` from the name, I think.

---

_@charliermarsh reviewed on 2024-09-24 02:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:668 on 2024-09-24 02:04_

Same feedback as in the other PR around `&Option<T>`. In this case, best would be `Option<&str>`, since you can always go from `Option<&String>` to `Option<&str>`, but not the other way around.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:427 on 2024-09-24 02:05_

I think we should use two nested enums here, to make the `anyhow::bail!("Error during script initialization")` below impossible. So, something like:

```rust
#[derive(Debug, Copy, Clone, Default)]
pub(crate) enum InitKind {
    #[default]
    Project(InitProjectKind),
    Script,
}

#[derive(Debug, Copy, Clone, Default)]
pub(crate) enum InitProjectKind {
    #[default]
    Application,
    Library,
}
```


---

_@charliermarsh reviewed on 2024-09-24 02:05_

---

_@charliermarsh reviewed on 2024-09-24 02:05_

---

_Review comment by @charliermarsh on `docs/guides/scripts.md`:130 on 2024-09-24 02:05_

We stylize as `Python` (here, in the body, etc.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-24 02:05_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-24 02:05_

---

_Label `enhancement` added by @charliermarsh on 2024-09-24 02:05_

---

_Label `cli` added by @charliermarsh on 2024-09-24 02:05_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:650 on 2024-09-24 02:07_

Yeah let's do that as a best practice.

---

_@charliermarsh reviewed on 2024-09-24 02:07_

---

_Comment by @charliermarsh on 2024-09-24 02:07_

Thanks! Left some feedback, not far off though.

---

_@tfsingh reviewed on 2024-09-24 18:56_

---

_Review comment by @tfsingh on `crates/uv-cli/src/lib.rs`:2335 on 2024-09-24 18:56_

Done

---

_@tfsingh reviewed on 2024-09-24 18:58_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/init.rs`:47 on 2024-09-24 18:58_

Didn't make this change as I switched the outer expression to a match given the new enum (and each arm requires custom logic depending on the value of explicit_path)

---

_@tfsingh reviewed on 2024-09-24 19:00_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/init.rs`:644 on 2024-09-24 19:00_

That makes sense, implemented this + modified the test to reflect the new behavior

---

_@tfsingh reviewed on 2024-09-24 19:00_

---

_Review comment by @tfsingh on `crates/uv-scripts/src/lib.rs`:118 on 2024-09-24 19:00_

Done

---

_@tfsingh reviewed on 2024-09-24 19:01_

---

_Review comment by @tfsingh on `crates/uv-scripts/src/lib.rs`:98 on 2024-09-24 19:01_

Yes sounds great, done

---

_@tfsingh reviewed on 2024-09-24 19:01_

---

_Review comment by @tfsingh on `crates/uv-scripts/src/lib.rs`:198 on 2024-09-24 19:01_

Done

---

_@tfsingh reviewed on 2024-09-24 19:03_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/add.rs`:51 on 2024-09-24 19:03_

Done

---

_@tfsingh reviewed on 2024-09-24 19:03_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/init.rs`:668 on 2024-09-24 19:03_

Done, thanks for the tip!

---

_@tfsingh reviewed on 2024-09-24 19:04_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/init.rs`:427 on 2024-09-24 19:04_

Agreed, this change made many things much cleaner :)

---

_@tfsingh reviewed on 2024-09-24 19:04_

---

_Review comment by @tfsingh on `docs/guides/scripts.md`:130 on 2024-09-24 19:04_

Fixed, won't happen again

---

_Comment by @tfsingh on 2024-09-24 19:17_

This is ready for another pass, thanks for the review!

---

_@charliermarsh approved on 2024-09-25 22:41_

---

_@charliermarsh reviewed on 2024-09-25 22:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:155 on 2024-09-25 22:43_

You generally want to avoid `&PathBuf`, similar to how you would avoid `&String`. The reasoning is similar to the previous comments around `&Option<T>` vs. `Option<&T>`. If you have a `PathBuf`, you can always get an `&Path`; but if you have a `Path`, you can't convert to `&PathBuf` without allocating.

---

_@charliermarsh reviewed on 2024-09-25 22:44_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:204 on 2024-09-25 22:44_

I refactored this to do a single I/O operation -- so, we try to read the script, and then we handle appropriate errors. It goes against the idea of using error handling for control flow somewhat, but it avoids race conditions are [TOCTOU](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use) errors: in the previous implementation, between the time you checked the file metadata and the time you actually read the file, the file could be removed or replaced.

---

_Renamed from "Add support for uv init --script" to "Add support for `uv init --script`" by @charliermarsh on 2024-09-25 22:44_

---

_Merged by @charliermarsh on 2024-09-25 22:48_

---

_Closed by @charliermarsh on 2024-09-25 22:48_

---

_Comment by @charliermarsh on 2024-09-25 22:48_

Thanks @tfsingh! I also added @jbvsmo as a co-author.

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/init.rs`:155 on 2024-09-25 23:19_

I see, ```Path``` is to ```PathBuf``` what ```&str``` is to ```String``` clarifies this well, thanks!

---

_@tfsingh reviewed on 2024-09-25 23:19_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/init.rs`:204 on 2024-09-25 23:19_

This makes sense — this operation felt a little like a "conversational" transaction of sorts, the new implementation is much cleaner

---

_@tfsingh reviewed on 2024-09-25 23:19_

---
