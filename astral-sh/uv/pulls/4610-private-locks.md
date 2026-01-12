```yaml
number: 4610
title: Private locks
type: pull_request
state: closed
author: idlsoft
labels: []
assignees: []
base: main
head: private_locks
created_at: 2024-06-28T04:52:34Z
updated_at: 2025-04-28T12:26:32Z
url: https://github.com/astral-sh/uv/pull/4610
synced_at: 2026-01-12T16:06:20Z
```

# Private locks

---

_@idlsoft_

## Summary

A basic implementation for #4574.

This introduces `private-members` to `[tool.uv.workspace]`.
Private members are not included in the workspace `uv.lock` and `.venv`, but have their own instead.

The code could use some polish, and we'd need to make sure pip commands and workspace commands work consistently.

Otherwise I think it's good enough to review.

The basic implementation is in [workspace.rs](https://github.com/idlsoft/uv/blob/private_locks/crates/uv-workspace/src/workspace.rs).
There is a new `private_lock` field in `WorkspaceMember`.
The following functions moved from `Workspace` to `VirtualProject` and depend on `private_lock`:
```rust
pub fn venv(&self) -> PathBuf
pub fn lockfile(&self) -> PathBuf
pub fn members_as_requirements(&self) -> Vec<Requirement>
pub fn constraints(&self) -> Vec<Requirement>
pub fn overrides(&self) -> Vec<Requirement>
```
The rest is some refactoring to make the calls possible.
These include changing the `Workspace` argument to `VirtualProject` for the following functions in [mod.rs](https://github.com/idlsoft/uv/blob/private_locks/crates/uv/src/commands/project/mod.rs):
- `find_environment`
- `FoundInterpreter::discover`
- `get_or_init_environment`

I split it in a few commits to make following the changes easier.


## Test Plan

A sample workspace and unittest.


---

_Marked ready for review by @idlsoft on 2024-06-28 10:57_

---

_Comment by @charliermarsh on 2024-07-21 23:38_

Sorry for not engaging with this sooner (apart from our conversation on the originating issue). I think something like this is interesting but I likely won't prioritize exploring it until after we get the initial workspace support out.

In the meantime... the intent is that you should be able to create a project outside of a workspace, and then use a path dependency to point to a member of a workspace that's it _not_ part of, and it should "just work" -- which IIUC should achieve the same thing here? Since that separate project would get its own lock, venv, etc. Though not certain if we finished implementing that yet.


---

_Comment by @charliermarsh on 2024-07-21 23:41_

> Though not certain if we finished implementing that yet.

I just confirmed that this _does_ work by creating `foo/pyproject.toml` in the uv root with:

```toml
[project]
name = "foo"
version = "0.0.0"
dependencies = [
  "bird-feeder",
]
[tool.uv.sources]
"bird-feeder" = { path = "../scripts/workspaces/albatross-root-workspace/packages/bird-feeder" }
```

Which does correctly include the workspace dependencies.

---

_Comment by @idlsoft on 2024-07-22 00:57_

> In the meantime... the intent is that you should be able to create a project outside of a workspace, and then use a path dependency to point to a member of a workspace that's it _not_ part of, and it should "just work" 

It does work. I actually checked a few days ago, and closed two other PRs, because`{path=... editable=true}` solved the use-case perfectly.
And while you're right, path dependencies is a solution, I would still like to keep this one for future consideration.

I think it's a valid use-case, and I like that `members` and `projects` are defined next to each other without a clear preference which one is the right approach, the choice being left to the developer.

Workspaces, among other things, are a good way to share metadata, like default constraints, default versions etc. It would be nice if projects, even they're independent from each other, could take advantage of that. 

---

_Comment by @zanieb on 2024-08-07 20:51_

Hi! Do you mind if I close this? We'll need more discussion in an issue before we can accept this kind of change and we're paying for these CI runs every time it is updated.

---

_Comment by @idlsoft on 2024-08-07 20:53_

> Hi! Do you mind if I close this? We'll need more discussion in an issue before we can accept this kind of change and we're paying for these CI runs every time it is updated.

Will it still run CI if we mark it as draft?
Also I can stop resolving conflicts for a while, it won't run builds then.

---

_Comment by @zanieb on 2024-08-07 20:56_

Yeah it'll still run CI if marked as a draft.

---

_Comment by @idlsoft on 2024-08-07 20:58_

I'll stop pushing then, so it shouldn't be a problem.
If you'd like to close it, that's fine too.

---

_Comment by @idlsoft on 2024-09-12 18:13_

> Sorry for not engaging with this sooner (apart from our conversation on the originating issue). I think something like this is interesting but I likely won't prioritize exploring it until after we get the initial workspace support out.
> 

@charliermarsh  any chance to giving this another look?

I've been resolving conflicts periodically in another branch, so can bring the PR up to date easily.

A couple of notes:
- about the user-facing naming conventions: `projects` was first neutral term that came to mind, `private_members` may be more consistent/easier to explain?
- the 1st commit can be merged with no side-effects in case you'd like to postpone the actual implementation till later


---

_Comment by @idlsoft on 2024-10-03 17:02_

> I'll stop pushing then

@zanieb Made a minor change, `projects` has been renamed to `private-members`, I think it makes sense to use existing terms.

Also the implementation will support intersecting globs for `members` and `private-members`. Private takes precedence.



---

_Closed by @konstin on 2025-04-28 12:26_

---
