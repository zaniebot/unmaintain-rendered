```yaml
number: 10861
title: "Add dependency-group cli flags to `uv pip install` and `uv pip compile` (`--group`, `--no-group`, `--only-group`, `--all-groups`)"
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/depgroups
created_at: 2025-01-22T14:55:18Z
updated_at: 2025-01-31T15:51:15Z
url: https://github.com/astral-sh/uv/pull/10861
synced_at: 2026-01-10T11:10:34Z
```

# Add dependency-group cli flags to `uv pip install` and `uv pip compile` (`--group`, `--no-group`, `--only-group`, `--all-groups`)

---

_Pull request opened by @Gankra on 2025-01-22 14:55_

Ultimately this is a lot of settings plumbing and a couple minor pieces of Actual Logic (which are so simple I have to assume there's something missing, but maybe not!).

Note this "needlessly" use DevDependencyGroup since it costs nothing, is more futureproof, and lets us maintain one primary interface (we just pass `false` for all the dev arguments).

Fixes #8590
Fixes #8969

---

_Comment by @Gankra on 2025-01-22 14:57_

<comment uplifted to main comment>

---

_@Gankra reviewed on 2025-01-22 15:06_

---

_Review comment by @Gankra on `crates/uv-settings/src/settings.rs`:1075 on 2025-01-22 15:06_

Whether these were supposed to pass through the shared PipOptions and not a side-thing was an uncertainty for me. Overall I assumed they're equivalentish to the `extra` settings and those are here, so, seems fine?

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1033 on 2025-01-22 17:02_

Do we respect `default-groups` in the `uv pip` interface? I would think not (without thinking about it too hard)

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1035 on 2025-01-22 17:02_

Presumably just copy/paste, but we should swap these lines (throughout).

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:172 on 2025-01-22 17:05_

I'm not entirely sure about the correctness of this. Is this relying on some assumptions about the construction of `GroupsSpecification`?

---

_Review comment by @zanieb on `crates/uv-requirements/src/sources.rs`:185 on 2025-01-22 17:05_

I don't think groups can be defined in `setup.py` or `setup.cfg`

---

_Review comment by @zanieb on `crates/uv-requirements/src/sources.rs`:185 on 2025-01-22 17:06_

(Requires some documentation updates too)

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:1075 on 2025-01-22 17:07_

Seems right to me, though the option abstractions are not my forte.

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:8375 on 2025-01-22 17:10_

What happens if multiple `pyproject.toml` files are provided? What happens if a group is present in one but not the other? If a group is present in both?

---

_Review comment by @zanieb on `docs/reference/settings.md`:1726 on 2025-01-22 17:13_

Honestly I'm not sure why we support requesting extras in settings file like this. It seems correct to match the extras options for groups though?

Perhaps @charliermarsh understands the use-case?

---

_@zanieb reviewed on 2025-01-22 17:13_

---

_@Gankra reviewed on 2025-01-22 17:23_

---

_Review comment by @Gankra on `crates/uv-configuration/src/dev.rs`:172 on 2025-01-22 17:23_

It is, but it's a relatively mundane assumption imo.

---

_@Gankra reviewed on 2025-01-22 17:24_

---

_Review comment by @Gankra on `crates/uv-requirements/src/sources.rs`:185 on 2025-01-22 17:24_

Which docs? Happy to update 'em.

---

_@zanieb reviewed on 2025-01-22 17:24_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:172 on 2025-01-22 17:24_

Might be worth a comment saying what the assumption is, but don't feel strongly.

---

_@zanieb reviewed on 2025-01-22 17:25_

---

_Review comment by @zanieb on `crates/uv-requirements/src/sources.rs`:185 on 2025-01-22 17:25_

Basically everywhere it says "Only applies to `pyproject.toml`, `setup.py`, and `setup.cfg` sources."

---

_@zanieb reviewed on 2025-01-22 17:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:8375 on 2025-01-22 17:26_

Similarly, what about `uv pip install <dir> --group <name>`?

---

_@Gankra reviewed on 2025-01-22 18:00_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_install.rs`:8375 on 2025-01-22 18:00_

I believe as currently implemented we:
* don't complain if we fail to find a group in the toml
* would independently match and include groups from every toml

This falls out of the implementation basically being "for each sourcefile, for each group in that sourcefile, if the selector built from cli flags matches the name of this group, append that group to the requirements".

not sure about the dir usecase

---

_@zanieb reviewed on 2025-01-22 18:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:8375 on 2025-01-22 18:06_

I think we should fail (or at the very least, warn) if we cannot find a group.

I don't mind including groups from every toml, but we should have test coverage.

---

_@zanieb reviewed on 2025-01-22 18:07_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:8375 on 2025-01-22 18:07_

We fail in `uv sync` fwiw

```
❯ uv sync --group test
error: Group `test` is not defined in the project's `dependency-group` table
```

---

_@Gankra reviewed on 2025-01-22 18:57_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1033 on 2025-01-22 18:57_

I pushed a commit removing this again, assuming you're right.

---

_@Gankra reviewed on 2025-01-22 18:57_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1035 on 2025-01-22 18:57_

Done.

---

_@Gankra reviewed on 2025-01-22 19:00_

---

_Review comment by @Gankra on `crates/uv-requirements/src/sources.rs`:185 on 2025-01-22 19:00_

Pushed a commit restricting to toml and updating all the doc strings, assuming you're right.

---

_@Gankra reviewed on 2025-01-22 19:03_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1033 on 2025-01-22 19:03_

Holy hell github's UI is haunted and won't let me unselect this textbox or cancel it, and constantly scrolls me back here, even if I refresh. I'm going to submit this comment here to see if that frees me.

---

_@Gankra reviewed on 2025-01-22 19:03_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1033 on 2025-01-22 19:03_

Oh my god no it keeps making a new one it DEMANDS ELABORATION on this critical discussion.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1033 on 2025-01-22 19:09_

:D

---

_@zanieb reviewed on 2025-01-22 19:09_

---

_@zanieb reviewed on 2025-01-22 19:09_

---

_Review comment by @zanieb on `crates/uv-requirements/src/sources.rs`:185 on 2025-01-22 19:09_

They definitely cannot be defined outside a `pyproject.toml` — the spec is just for that file.

---

_@Gankra reviewed on 2025-01-22 20:35_

---

_Review comment by @Gankra on `crates/uv/tests/it/pip_install.rs`:8375 on 2025-01-22 20:35_

Added warnings.

---

_Renamed from "Add dependency-group cli flags to `uv pip install` and `uv pip compile` (`--group`, `--no-group`, `--no-default-groups`, `--only-group`, `--all-groups`)" to "Add dependency-group cli flags to `uv pip install` and `uv pip compile` (`--group`, `--no-group`, `--only-group`, `--all-groups`)" by @Gankra on 2025-01-23 03:00_

---

_Merged by @Gankra on 2025-01-23 13:47_

---

_Closed by @Gankra on 2025-01-23 13:47_

---

_Branch deleted on 2025-01-23 13:47_

---

_Comment by @qvalentin on 2025-01-23 14:12_

Thanks @Gankra 

---

_Comment by @henryiii on 2025-01-23 16:54_

Not that this is does not match the proposed API for `pip`, so `pip` and `uv pip` would diverge. https://github.com/pypa/pip/pull/13065 is going to be merged soon.

---

_Comment by @Gankra on 2025-01-23 17:06_

Wow incredible timing. This won't get released in uv until we're happy with our interop story (will back this out if need be).

---

_Comment by @zanieb on 2025-01-23 17:08_

This is intended to be compatible with the proposed `pip` interface, i.e., we can implement it once it's released. We have to deal with a bit more complexity because we allow `-r pyproject.toml` which `pip` has said they will not support.

---

_Comment by @Filimoa on 2025-01-31 15:19_

Is there another way of compiling development groups dependencies into a requirements.txt if this is no longer planned? I don't see anything in the docs or issues so just wondering if it's possible.

---

_Comment by @charliermarsh on 2025-01-31 15:24_

If you use `uv lock`, you can then `uv export` into a `requirements.txt` today. (We still plan to ship this, we just deferred it for a bit.)

---

_Comment by @henryiii on 2025-01-31 15:51_

And https://github.com/pypa/pip/pull/13065 should be about ready to go in, it's already got an approval. You can also use `uvx dependency-groups > requirements.txt` if you want them unlocked.

---
