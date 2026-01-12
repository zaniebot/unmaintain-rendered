```yaml
number: 7687
title: "uv run: List available scripts when a script is not specified"
type: pull_request
state: merged
author: kakkoyun
labels:
  - enhancement
assignees: []
merged: true
base: main
head: list_available_scripts_and_executables
created_at: 2024-09-25T15:46:30Z
updated_at: 2024-10-08T19:36:08Z
url: https://github.com/astral-sh/uv/pull/7687
synced_at: 2026-01-12T16:07:57Z
```

# uv run: List available scripts when a script is not specified

---

_@kakkoyun_

Signed-off-by: Kemal Akkoyun <kakkoyun@gmail.com>
## Summary

This PR adds the ability to list available scripts in the environment
when `uv run` is invoked without any arguments.
It somewhat mimics the behavior of `rye run` command
(See https://rye.astral.sh/guide/commands/run).

This is an attempt to fix #4024.

## Test Plan

I added test cases. The CI pipeline should pass.

### Manuel Tests

```shell
❯ uv run
Provide a command or script to invoke with `uv run <command>` or `uv run script.py`.

The following scripts are available:

normalizer
python
python3
python3.12

See `uv run --help` for more information.
```


---

_@zanieb reviewed on 2024-09-25 16:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:776 on 2024-09-25 16:58_

In what cases can this occur? Should it be user-facing?

---

_@zanieb reviewed on 2024-09-25 16:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:785 on 2024-09-25 16:58_

Should we handle symlinks like we do in `uv python list`? i.e. displaying "/opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12"

---

_@zanieb reviewed on 2024-09-25 16:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:749 on 2024-09-25 16:59_

We should probably just display the name of the file, not the full path since we're making suggestions for `uv run <name>`.

---

_@zanieb reviewed on 2024-09-25 17:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:726 on 2024-09-25 17:00_

Note we use `user_display()` for all paths.

I'm not sure if we need this here though, we might just be able to say "Available commands:"?

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:726 on 2024-09-25 17:00_

We also use `writeln!(printer.stderr(), ...)` instead of `println!` to respect things like the `--quiet` flag.

---

_@zanieb reviewed on 2024-09-25 17:00_

---

_@zanieb reviewed on 2024-09-25 17:02_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:137 on 2024-09-25 17:02_

How is `Some(RunCommand::Empty)` different than `None`?

---

_@kakkoyun reviewed on 2024-09-25 17:11_

---

_Review comment by @kakkoyun on `crates/uv/src/commands/project/run.rs`:785 on 2024-09-25 17:11_

I think we should show the filename. We don't need to be super descriptive here. Propabaly this usage flow will be like:

```shell
❯ cargo run -q -- run
Provide a command or script to invoke with `uv run <command>` or `uv run script.py`.

The following scripts are available:

python3
python3.12
python
normalizer

See `uv run --help` for more information.
```
```shell
❯ cargo run -q -- run python
No entry for terminal type "xterm-256color";
using dumb terminal settings.
Python 3.12.1 (main, Jan  8 2024, 05:57:25) [Clang 17.0.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

---

_@kakkoyun reviewed on 2024-09-25 17:12_

---

_Review comment by @kakkoyun on `crates/uv/src/commands/project/run.rs`:726 on 2024-09-25 17:12_

This was just for the debugging purposes.

---

_@kakkoyun reviewed on 2024-09-25 17:14_

---

_Review comment by @kakkoyun on `crates/uv/src/lib.rs`:137 on 2024-09-25 17:14_

I'm just following the existing function signatures here. I preferred to use `Empty` since it was available rather than changing to `Option<RunCommand>`.

I'm not sure if there is a general convention/style to follow here.

---

_@zanieb reviewed on 2024-09-25 17:20_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:137 on 2024-09-25 17:20_

Isn't `run_command` already `Option<RunCommand`> though?

---

_@kakkoyun reviewed on 2024-09-25 17:22_

---

_Review comment by @kakkoyun on `crates/uv/src/commands/project/run.rs`:776 on 2024-09-25 17:22_

This wasn't intended to land on the final version of the PR. It makes sense to keep it for debugging purposes. I will change the level.

Underlying could be various things that are propagated from the filesystem (e.g. insufficient permissions).

---

_@kakkoyun reviewed on 2024-09-25 17:33_

---

_Review comment by @kakkoyun on `crates/uv/src/lib.rs`:137 on 2024-09-25 17:33_

Not in this particular context https://github.com/astral-sh/uv/blob/26a355220d88292110f07b71269c682b33503c54/crates/uv/src/commands/project/run.rs#L52-L77

---

_Marked ready for review by @kakkoyun on 2024-09-25 17:44_

---

_Review requested from @zanieb by @kakkoyun on 2024-09-25 19:55_

---

_@zanieb reviewed on 2024-09-25 19:55_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:137 on 2024-09-25 19:55_

Ah but it is at

https://github.com/astral-sh/uv/blob/106633a5e5b6c056525df92cd26d05e60fbc2e55/crates/uv/src/lib.rs#L133

and

https://github.com/astral-sh/uv/blob/106633a5e5b6c056525df92cd26d05e60fbc2e55/crates/uv/src/lib.rs#L1149

then we unwrap at 

https://github.com/astral-sh/uv/blob/106633a5e5b6c056525df92cd26d05e60fbc2e55/crates/uv/src/lib.rs#L1230

The `Empty` variant is stale code i.e. we actually don't do this anymore

https://github.com/astral-sh/uv/blob/77c2496f4706a99ada9b449072a38fb4b9e1f816/crates/uv/src/commands/project/run.rs#L846-L847

I'd think I'd prefer to just make it an `Option<...>` and remove the `expect` call, but I don't feel strongly. I guess if it's an `Option` someone could make a mistake upstream while developing and we wouldn't notice? Regardless we should update the `RunCommand::Empty` documentation.

---

_@zanieb reviewed on 2024-09-25 20:05_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:258 on 2024-09-25 20:05_

I think we'll want to exclude all the shell activation scripts

---

_@zanieb reviewed on 2024-09-25 20:06_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:261 on 2024-09-25 20:06_

We'll probably also want to exclude the `.exe` suffix since it's implied during invocation.

---

_@zanieb reviewed on 2024-09-25 20:07_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:226 on 2024-09-25 20:07_

```suggestion
    Provide a command or script to invoke with `uv run <command>` or `uv run <script>.py`.
```

---

_@zanieb reviewed on 2024-09-25 20:07_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:228 on 2024-09-25 20:07_

We should probably say "commands" here not "scripts" since above you suggest running a script as `script.py` and it risks user's conflating the concepts.

---

_@zanieb reviewed on 2024-09-25 20:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:767 on 2024-09-25 20:09_

Should we exit with a non-zero exit code? I'm not sure but I think so? 

---

_@zanieb reviewed on 2024-09-25 20:10_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:232 on 2024-09-25 20:10_

I think I have a minor preference for omitting the leading empty newline and listing these with the prefix `- `.



---

_@zanieb reviewed on 2024-09-25 20:10_

---

_Review comment by @zanieb on `crates/uv-python/src/which.rs`:6 on 2024-09-25 20:10_

We might want to move this to `uv-fs` instead of `uv-python` now.

---

_@kakkoyun reviewed on 2024-09-26 07:24_

---

_Review comment by @kakkoyun on `crates/uv/src/commands/project/run.rs`:767 on 2024-09-26 07:24_

Let me check what we do for the `uv tool run`. But it makes sense to return an unsuccessful exit code. I favor consistency, so whatever we decide should be the same all over the board. What do you think?

---

_@kakkoyun reviewed on 2024-09-26 07:25_

---

_Review comment by @kakkoyun on `crates/uv/tests/run.rs`:261 on 2024-09-26 07:25_

👍 I didn't know that. I need a Windows setup to test it. I will check the codebase or take your word for it 😄 

---

_@konstin reviewed on 2024-09-26 11:09_

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:776 on 2024-09-26 11:09_

We should raise an error here and exit imho, or at least not make it silent: If we can't read an entry from the scripts directory, something is seriously broken about the venv.

---

_Label `enhancement` added by @konstin on 2024-09-26 11:09_

---

_@kakkoyun reviewed on 2024-09-26 11:54_

---

_Review comment by @kakkoyun on `crates/uv/src/lib.rs`:137 on 2024-09-26 11:54_

Ok, if it's deprecated. Let me check how I can refactor things. Thanks a lot for the context.

---

_@kakkoyun reviewed on 2024-09-26 11:55_

---

_Review comment by @kakkoyun on `crates/uv/src/commands/project/run.rs`:776 on 2024-09-26 11:55_

Thanks a lot for the input.

Alright, this aligns with other parts of the codebase. I will surface to error.

---

_@kakkoyun reviewed on 2024-09-26 13:30_

---

_Review comment by @kakkoyun on `crates/uv/src/lib.rs`:137 on 2024-09-26 13:30_

Okay, I will change the signature and utilize the `Option.` However, I would prefer to address/explore the deprecation of `RunCommnad::Empty` in another PR. I need to make sure that the existing mechanisms need it to function internally.

---

_@zanieb reviewed on 2024-09-26 14:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:767 on 2024-09-26 14:19_

Showing the help menu exits with code 2 right now — I think we should be consistent with that.

---

_@kakkoyun reviewed on 2024-09-26 14:45_

---

_Review comment by @kakkoyun on `crates/uv/src/commands/project/run.rs`:767 on 2024-09-26 14:45_

👍 Done. I've also updated https://github.com/astral-sh/uv/pull/7641

---

_@zanieb reviewed on 2024-09-26 16:58_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:137 on 2024-09-26 16:58_

No problem.

---

_@zanieb approved on 2024-09-27 15:11_

LGTM!

---

_Converted to draft by @kakkoyun on 2024-09-27 15:48_

---

_Marked ready for review by @kakkoyun on 2024-09-27 16:56_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-27 17:16_

---

_Merged by @zanieb on 2024-10-08 19:34_

---

_Closed by @zanieb on 2024-10-08 19:34_

---

_Comment by @zanieb on 2024-10-08 19:36_

Thanks @kakkoyun!

---
