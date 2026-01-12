```yaml
number: 14309
title: "Require `uv venv --clear` before removing an existing directory"
type: pull_request
state: merged
author: jtfmumm
labels:
  - virtualenv
  - breaking
assignees: []
merged: true
base: release/080
head: jtfm/venv-clear
created_at: 2025-06-27T10:02:58Z
updated_at: 2025-07-16T19:25:50Z
url: https://github.com/astral-sh/uv/pull/14309
synced_at: 2026-01-12T16:11:08Z
```

# Require `uv venv --clear` before removing an existing directory

---

_@jtfmumm_

By default, `uv venv <venv-name>` currently removes the `<venv-name`> directory if it exists. This can be surprising behavior: not everyone expects an existing environment to be overwritten. This PR updates the default to fail if a non-empty `<venv-name>` directory already exists and neither `--allow-existing` nor the new `-c/--clear` option is provided (if a TTY is detected, it prompts first). If it's not a TTY, then uv will only warn and not fail for now — we'll make this an error in the future. I've also added a corresponding `UV_VENV_CLEAR` env var.

I've chosen to use `--clear` instead of `--force` for this option because it is used by the `venv` module and `virtualenv` and will be familiar to users. I also think its meaning is clearer in this context than `--force` (which could plausibly mean force overwrite just the virtual environment files, which is what our current `--allow-existing` option does).

Closes #1472. 


---

_Label `virtualenv` added by @jtfmumm on 2025-06-27 10:03_

---

_Label `breaking` added by @jtfmumm on 2025-06-27 10:03_

---

_Label `do-not-merge` added by @jtfmumm on 2025-06-27 10:18_

---

_Added to milestone `v0.8.0` by @jtfmumm on 2025-07-03 06:48_

---

_Label `do-not-merge` removed by @jtfmumm on 2025-07-03 06:48_

---

_@zanieb reviewed on 2025-07-03 17:34_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1045 on 2025-07-03 17:34_

This is a good example of the breakage we'd cause with this change. I wonder if we can craft a GitHub search that captures if a second `uv venv` is common?

---

_@zanieb reviewed on 2025-07-14 14:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2614 on 2025-07-14 14:28_

Why does this conflict with `allow_existing` rather than override?

---

_@zanieb reviewed on 2025-07-14 14:28_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:295 on 2025-07-14 14:28_

This seems a bit off for the top-level line in this context?

---

_@zanieb reviewed on 2025-07-14 14:29_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:295 on 2025-07-14 14:29_

For reference, other flag variables have documentation like

```
    /// Equivalent to the `--no-verify-hashes` argument. Disables hash verification for
    /// `requirements.txt` files.
```

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:96 on 2025-07-14 14:30_

Can we use a `match` here instead so we have to handle new variants if they're added?

---

_@zanieb reviewed on 2025-07-14 14:30_

---

_@zanieb reviewed on 2025-07-14 14:32_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:122 on 2025-07-14 14:32_

We use `stdin().is_terminal()` elsewhere, is there a reason you preferred this?

---

_@zanieb reviewed on 2025-07-14 14:33_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:122 on 2025-07-14 14:33_

Interesting, I did a search here and we use `stderr().is_term()` in quite a few places. I don't know which is correct, but we should have a consistent approach. We can discuss that separately.

---

_@zanieb reviewed on 2025-07-14 14:34_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:122 on 2025-07-14 14:34_

https://github.com/astral-sh/uv/issues/14607

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:2614 on 2025-07-14 14:34_

I was being conservative. I think we'd want `--allow-existing` to override if we took that approach? The presence of the flag might indicate they expect those files to be retained

---

_@jtfmumm reviewed on 2025-07-14 14:34_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:123 on 2025-07-14 14:36_

It's a little weird that this is separated from the confirmation prompt, it makes it hard to tell what the logic is. Could it be restructured to be clearer?

---

_@zanieb reviewed on 2025-07-14 14:36_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:122 on 2025-07-14 14:36_

I think this way is actually more common in the codebase. I was imitating the way it was done

---

_@jtfmumm reviewed on 2025-07-14 14:36_

---

_@zanieb reviewed on 2025-07-14 14:37_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:141 on 2025-07-14 14:37_

I probably wouldn't mention `--allow-existing`, it's kind of a niche use-case. 

---

_@zanieb reviewed on 2025-07-14 14:37_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:138 on 2025-07-14 14:37_

At this point, do we know that the directory is a virtual environment?

---

_@jtfmumm reviewed on 2025-07-14 14:37_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:122 on 2025-07-14 14:37_

But I don't have a reason to prefer one or the other

---

_@zanieb reviewed on 2025-07-14 14:38_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1036 on 2025-07-14 14:38_

Could this be a `VenvCreationPolicy::from_args` method to match other constructors?

---

_@zanieb reviewed on 2025-07-14 14:38_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:34 on 2025-07-14 14:38_

This comment is stale

---

_@zanieb reviewed on 2025-07-14 14:39_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2614 on 2025-07-14 14:39_

I think you'd probably just have them both override each other?

---

_@jtfmumm reviewed on 2025-07-14 14:43_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:138 on 2025-07-14 14:43_

We should be able to check it by this point

---

_@zanieb reviewed on 2025-07-14 14:46_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:138 on 2025-07-14 14:46_

In that case, I think we can craft a bit more specific of a message. e.g., I'd say

> A virtual environment already exists at `{}`. In the future, ...

---

_@jtfmumm reviewed on 2025-07-14 14:49_

---

_Review comment by @jtfmumm on `crates/uv-static/src/env_vars.rs`:295 on 2025-07-14 14:49_

I was attempting to parallel the `--allow-existing` top line, which is

```
/// Preserve any existing files or directories at the target path.
```

---

_@zanieb reviewed on 2025-07-14 14:50_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:295 on 2025-07-14 14:50_

This is a comment on the environment variable reference.

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:295 on 2025-07-14 14:51_

(It's fine as-is for the `--clear` flag)

---

_@zanieb reviewed on 2025-07-14 14:51_

---

_Review comment by @jtfmumm on `crates/uv-static/src/env_vars.rs`:295 on 2025-07-14 14:52_

Ah, on that one I was paralleling `UV_VENV_SEED`:

```
/// Install seed packages (one or more of: `pip`, `setuptools`, and `wheel`) into the virtual environment
/// created by `uv venv`.
 ```
 
 I'll update

---

_@jtfmumm reviewed on 2025-07-14 14:52_

---

_@zanieb reviewed on 2025-07-14 14:54_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:295 on 2025-07-14 14:54_

I don't mind that style either.

---

_@jtfmumm reviewed on 2025-07-14 14:58_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:2614 on 2025-07-14 14:58_

Given that the motivation for this is to avoid surprising people when removing the directory, I'm a little hesitant to have `--clear` override in the last position. But on the other hand, you did mark it with `--clear`. If you still think it's ok, I can update

---

_@zanieb reviewed on 2025-07-14 15:09_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2614 on 2025-07-14 15:09_

Yeah I think since it's opt-in that should be fine. It's valuable for something like `UV_VENV_CLEAR=1 uv venv --allow-existing`.


---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:138 on 2025-07-14 15:29_

Updated

---

_@jtfmumm reviewed on 2025-07-14 15:29_

---

_@zanieb reviewed on 2025-07-14 15:34_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:96 on 2025-07-14 15:34_

I mean in this area overall. It may require a bit more restructuring?

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:96 on 2025-07-14 16:02_

I think I've figured it out. Pushing soon

---

_@jtfmumm reviewed on 2025-07-14 16:02_

---

_@jtfmumm reviewed on 2025-07-14 16:11_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:96 on 2025-07-14 16:11_

I've restructured it into a match, which also addresses your concern below

---

_@jtfmumm reviewed on 2025-07-14 16:11_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:123 on 2025-07-14 16:11_

Addressed by restructuring

---

_@jtfmumm reviewed on 2025-07-14 16:11_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:2614 on 2025-07-14 16:11_

Done

---

_Renamed from "Require `uv venv --clear` before clearing an existing directory" to "Require `uv venv --clear` before removing an existing directory" by @zanieb on 2025-07-16 19:17_

---

_Merged by @zanieb on 2025-07-16 19:25_

---

_Closed by @zanieb on 2025-07-16 19:25_

---

_Branch deleted on 2025-07-16 19:25_

---
