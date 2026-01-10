```yaml
number: 1180
title: "Prompt user for missing `-r` and `-e` flags in `pip install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/r2
created_at: 2024-01-30T01:40:41Z
updated_at: 2024-01-30T19:41:10Z
url: https://github.com/astral-sh/uv/pull/1180
synced_at: 2026-01-10T15:39:03Z
```

# Prompt user for missing `-r` and `-e` flags in `pip install`

---

_Pull request opened by @charliermarsh on 2024-01-30 01:40_

## Summary

If the user runs a command like `pip install requirements.txt`, we now prompt them to ask if they meant to include the `-r` flag:

![Screenshot 2024-01-29 at 8 38 29 PM](https://github.com/astral-sh/puffin/assets/1309177/82b9f7a2-2526-4144-b200-a5015e5b8a4b)

![Screenshot 2024-01-29 at 8 38 33 PM](https://github.com/astral-sh/puffin/assets/1309177/bd8ebb51-2537-4540-a0e0-718e66a1c69c)

The specific logic is: if the requirement ends in `.txt` or `.in`, and the file exists locally, prompt the user for `-r`. If the requirement contains a directory separator, and the directory exists locally, prompt the user for `-e`.

Closes #1166.

---

_@charliermarsh reviewed on 2024-01-30 01:41_

---

_Review comment by @charliermarsh on `Cargo.toml`:32 on 2024-01-30 01:41_

We already depend on `console` via `indicatif`, so this doesn't actually add any new dependencies.

---

_@charliermarsh reviewed on 2024-01-30 01:41_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 01:41_

(This package will be invalid anyway, it'll get rejected when we try to parse it.)

---

_Review requested from @zanieb by @charliermarsh on 2024-01-30 01:42_

---

_Review requested from @konstin by @charliermarsh on 2024-01-30 01:42_

---

_Label `enhancement` added by @charliermarsh on 2024-01-30 01:43_

---

_Review comment by @charliermarsh on `crates/puffin/src/main.rs`:674 on 2024-01-30 01:51_

I got rid of the `From<PathBuf>` implementation for this, it felt a bit magical.

---

_@charliermarsh reviewed on 2024-01-30 01:51_

---

_Review comment by @charliermarsh on `crates/puffin/src/confirm.rs`:7 on 2024-01-30 01:51_

We could pull in `dialoguer` for this but it added 70 KB to the binary and it felt like overkill. It's the exact same code as below though (modulo more configuration options).

---

_@charliermarsh reviewed on 2024-01-30 01:51_

---

_@zanieb reviewed on 2024-01-30 02:54_

---

_Review comment by @zanieb on `crates/puffin/src/requirements.rs`:54 on 2024-01-30 02:54_

Should we say "looks like a requirements file but was passed as a package name" ? Otherwise what I did wrong is not immediately obvious (same for the directory case)

---

_@zanieb reviewed on 2024-01-30 02:56_

---

_Review comment by @zanieb on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 02:56_

Uh... `pip` allows this. It installs the local directory without making it editable e.g. `pip install ../../zanieb/packse` works fine.

---

_@charliermarsh reviewed on 2024-01-30 03:00_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 03:00_

Okay, should we support it?

---

_@charliermarsh reviewed on 2024-01-30 03:03_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 03:03_

(We obviously can't support it today since we require a package name.)

---

_@zanieb reviewed on 2024-01-30 03:07_

---

_Review comment by @zanieb on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 03:07_

Let's see if we can avoid supporting it to start. I'm not sure what the use case is, I guess maybe to test installation from what is basically a source distribution instead of having the full package source available? Maybe to pin the package from a temporary checkout? I use it locally sometimes but not with a great deal of intention.

Presuming we fix the bug in #1181 will it work if a package name is provided?

---

_@charliermarsh reviewed on 2024-01-30 03:10_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 03:10_

Sounds good. Yeah should be supported: https://github.com/astral-sh/puffin/pull/1182. Do you think it's worth including this prompt, or no?

---

_@zanieb reviewed on 2024-01-30 03:16_

---

_Review comment by @zanieb on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 03:16_

Yeah I think so. It might be worth including a note about the other syntax too?

---

_@zanieb reviewed on 2024-01-30 03:16_

---

_Review comment by @zanieb on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 03:16_

Maybe this one could be an error since it's ambiguous... but `-e` seems _much_ more common

---

_Review requested from @zanieb by @charliermarsh on 2024-01-30 05:08_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:54 on 2024-01-30 05:09_

Changed it. My only hesitation is that it's a pretty long message now.

---

_@charliermarsh reviewed on 2024-01-30 05:09_

---

_@konstin reviewed on 2024-01-30 08:10_

---

_Review comment by @konstin on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 08:10_

What happens if you do `pip install <dir>` with a package that doesn't support editable builds, do we error? In that we case we should support `pip install <dir>`. My example use case is you clone the repo, patch something and want to know if it fixes your bug, so you do with `pip install ../foo && python ./tests/reproducer.py` 


---

_@konstin approved on 2024-01-30 08:23_

---

_@charliermarsh reviewed on 2024-01-30 15:37_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 15:37_

We can't support `pip install <dir>` right now because we need a package name. We could add a similar workflow whereby we prompt for a package name, what do you think of that? Same for `pip install https://...`.

---

_@zanieb reviewed on 2024-01-30 15:43_

---

_Review comment by @zanieb on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 15:43_

I think we should try to avoid expanding scope with more prompts. I feel like a suggestion that you can provide the package name as an alternative should be sufficient? Do you want me to try to actually come up with a message I like?

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 16:01_

Yes feel free!

---

_@charliermarsh reviewed on 2024-01-30 16:01_

---

_@zanieb approved on 2024-01-30 16:23_

---

_Merged by @charliermarsh on 2024-01-30 18:58_

---

_Closed by @charliermarsh on 2024-01-30 18:58_

---

_Branch deleted on 2024-01-30 18:58_

---

_@AlexWaygood reviewed on 2024-01-30 19:18_

---

_Review comment by @AlexWaygood on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 19:18_

FTR, I use `pip install <dir>` without `-e` sometimes to check I've got my pyproject.toml setup correctly. End users downloading my package from PyPI won't be getting editable installs, and editable installs have access to files hanging around on my local filesystem that non-editable installs don't have -- so to accurately test what the end user will be getting, I have to use a non-editable install. Editable installs also sometimes behave differently for `--version` etc.

I think some GitHub actions to check packaging stuff might do a similar thing as well

---

_@charliermarsh reviewed on 2024-01-30 19:39_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 19:39_

Yeah, we should support it, but we currently don't (independent of this change), because we require a package name for all dependencies.


---

_@AlexWaygood reviewed on 2024-01-30 19:41_

---

_Review comment by @AlexWaygood on `crates/puffin/src/requirements.rs`:65 on 2024-01-30 19:41_

Makes sense!

---
