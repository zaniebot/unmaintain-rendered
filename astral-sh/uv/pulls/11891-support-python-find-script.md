```yaml
number: 11891
title: "Support `python find --script`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - enhancement
assignees: []
merged: true
base: main
head: python-find-script
created_at: 2025-03-02T12:55:39Z
updated_at: 2025-03-21T06:33:19Z
url: https://github.com/astral-sh/uv/pull/11891
synced_at: 2026-01-12T16:10:02Z
```

# Support `python find --script`

---

_@InSyncWithFoo_

## Summary

Resolves #11794.

When `uv python find` is given a `--script` option, either the existing environment for that script or the Python executable that would be used to create it will be returned. If neither are found, the command exits with exit code 1.

`--script` is incompatible with all other options to the same command.

## Test Plan

Unit tests.


---

_Assigned to @zanieb by @zanieb on 2025-03-02 15:50_

---

_Marked ready for review by @InSyncWithFoo on 2025-03-02 19:30_

---

_Label `enhancement` added by @konstin on 2025-03-03 11:46_

---

_Comment by @zanieb on 2025-03-05 21:10_

How did you choose to exit with a non-zero code if the environment does not exist rather than showing the interpreter we would use to create the environment? I think there's probably a use case for `uv python find --script <path>` that shows the interpreter that fulfills the `requires-python` range, right? The downside here is that a user then needs to do more analysis to understand if the interpreter is for the synced environment or not.

Separately, if we're going to exit with a non-zero code, we need to show a message indicating why we failed.

---

_Comment by @InSyncWithFoo on 2025-03-05 21:41_

@zanieb I was thinking about my use case: An <em>existing</em> environment is needed to power language features. Showing the base interpreter would be misleading, since it won't have access to the dependencies of the script. Additionally, if an environment doesn't exist, the user should have a chance to decide whether it should be created or not.

As for the exit code and error messages, I don't have a strong opinion. Maybe exit code 0 should be used even when an interpreter cannot be found.



---

_Comment by @zanieb on 2025-03-05 21:46_

For your use-case, why wouldn't you always run a `sync` operation first?

The problem with failing if the environment doesn't exist is that it's inconsistent with how this interface normally behaves, e.g.:

```
❯ uv python find
/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
❯ uv sync
❯ uv python find
/Users/zb/workspace/uv/example/.venv/bin/python3
```

We could consider `--script` "opt-in" to requiring the script's environment to be found, but then there's no way for users to recover the behavior I described where you want to inspect what interpreter would be used for a given script.


---

_Comment by @InSyncWithFoo on 2025-03-05 22:18_

`sync` is a potentially expensive process; running it "just" to retrieve the interpreter is very likely not desirable.

---

_Comment by @charliermarsh on 2025-03-05 22:21_

If the environment doesn't exist, I would probably expect `uv python find --script` to show me where the script environment _would be_, rather than the interpreter that would be used to create it.

---

_Comment by @InSyncWithFoo on 2025-03-05 22:23_

@charliermarsh That is also undesirable: Tools might run the interpreter to retrieve its version. If I recall correctly, both PyCharm and Pyright do this.

---

_Comment by @charliermarsh on 2025-03-05 22:25_

Either way they have to handle the environment not existing, right?

---

_Comment by @zanieb on 2025-03-05 22:33_

> Either way they have to handle the environment not existing, right?

👍 that's my point. 

>  Tools might run the interpreter to retrieve its version.

If all you want is the interpreter for metadata, `uv python find` makes sense. It'd be bad if it showed you the path to an interpreter that does not exist. You don't need the virtual environment to know what Python interpreter uv would use.

> sync is a potentially expensive process; running it "just" to retrieve the interpreter is very likely not desirable.

If you are trying to provide up-to-date type information, this seems like what you need though? As Charlie said, you need to handle the case where it doesn't exist regardless.

> If the environment doesn't exist, I would probably expect uv python find --script to show me where the script environment would be, rather than the interpreter that would be used to create it.

That's not how it works for projects, though I'd be willing to consider changing that. I think it's more like `uv env find` (or `uv python find --env`) at that point? I worry quite a bit about returning the path to an interpreter that does not exist. We don't have a good way to signal that to the user.

---

_Comment by @InSyncWithFoo on 2025-03-06 00:14_

> If you are trying to provide up-to-date type information, this seems like what you need though? As Charlie said, you need to handle the case where it doesn't exist regardless.

In my opinion, such expensive processes should not be run without explicit user consent. Here's how I think the logic should be:

* If the interpreter exists and it is up-to-date, that's great.
* If the interpreter exists but not up-to-date, the user will be asked if they want to synchronize it (e.g., in the form of a quick fix to an "unresolved import" error).
* If the interpreter does not exist, the user will be asked if they want to create it.

If the installation is known to take a long time, the user might not want to create an environment on their development machine (especially if they don't intend to do any significant changes). Even if they <em>do</em> want to, prompting gives them a chance to check the script metadata block for any potential problems, like a typo in a dependency specifier.

The interpreter given must be one that belongs to the environment of that script. Otherwise, tools might have to do things twice, one for the global/base interpreter and one for the new interpreter (PyCharm would reindex the standard library, for example).

---

_Comment by @zanieb on 2025-03-06 00:31_

> In my opinion, such expensive processes should not be run without explicit user consent

This is roughly the antithesis of the design of this tool. We do these things transparently in the background all the time. We invest in making them fast enough that this is not expensive.

Regardless... if we return to your use-case, I think all you need is `uv sync --dry-run`?

```
❯ uv init --script foo.py
Initialized script at `foo.py`
❯ uv sync --script foo.py --dry-run
Would create script environment at: /Users/zb/.cache/uv/environments-v2/foo-2a05ce447f85797e
```

---

_Comment by @InSyncWithFoo on 2025-03-06 00:57_

> We invest in making them fast enough that this is not expensive.

That's just uv itself. Downloading takes time, and I have seen many complaints about how PyCharm's indexing takes seemingly forever.

Using `uv sync --dry-run` has two disadvantages:

* The path is that of the virtual environment directory, rather than the interpreter. Monkeypatching client-side is simple enough, but I'd rather not to.
* It would happily return the path to a potentially non-existent directory, which is undesired. The messages can be used to differentiate the two cases, but this is rather fragile.

---

_Comment by @zanieb on 2025-03-06 04:29_

> The path is that of the virtual environment directory, rather than the interpreter. Monkeypatching client-side is simple enough, but I'd rather not to.

I think this is well within scope for a consumer, but we could definitely add the "supposed" interpreter path to a JSON output format.

> It would happily return the path to a potentially non-existent directory, which is undesired. The messages can be used to differentiate the two cases, but this is rather fragile.

I think you'd use a JSON output mode to differentiate between these cases, so it's not fragile.

---

_Comment by @InSyncWithFoo on 2025-03-06 12:52_

> I think you'd use a JSON output mode to differentiate between these cases, so it's not fragile.

That would be right, except `sync` doesn't support `--format`. This would be within the scope of #3199.

Return to the PR at hand: The hypothetical `sync --dry-run --format=json` doesn't exist just yet (and tackling that would require even more design), so I'm inclined to keep this PR as it is, save for the exit code and such. What do you say?

---

_Comment by @zanieb on 2025-03-07 14:37_

I don't think I'm wiling to compromise on the consistency of the `uv python find` behavior to fulfill a use-case that would better filled in another interface.

---

_Comment by @zanieb on 2025-03-20 15:46_

Thanks for updating the branch. I'll review again today so it doesn't go stale again.

---

_@zanieb reviewed on 2025-03-20 18:46_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:783 on 2025-03-20 18:46_

Oh it's actually quite surprising we'd pull the Python from the virtual environment here. I wonder if we should avoid selecting active virtual environments during script Python discovery? That's a separate issue from this PR though.

---

_@zanieb reviewed on 2025-03-20 18:46_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:820 on 2025-03-20 18:46_

There's `TestContext::with_filtered_python_sources` for this

---

_@zanieb reviewed on 2025-03-20 18:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:748 on 2025-03-20 18:47_

There's `TestContext::with_filtered_exe_suffix`, `with_filtered_virtualenv_bin`, and `with_filtered_python_names` for this.

---

_@zanieb approved on 2025-03-20 18:48_

Thanks for your patience here

---

_Merged by @zanieb on 2025-03-21 01:49_

---

_Closed by @zanieb on 2025-03-21 01:49_

---

_Comment by @zanieb on 2025-03-21 01:50_

w.r.t. adding an `--output-format` to `uv sync` (ref https://github.com/astral-sh/uv/pull/11891#issuecomment-2703765832) — at some point I want a holistic design for https://github.com/astral-sh/uv/issues/3199 but I don't think that needs to block commands where JSON output will unblock a concrete use-case. If you're interested in adding that, I'm supportive.

---

_Branch deleted on 2025-03-21 06:33_

---
