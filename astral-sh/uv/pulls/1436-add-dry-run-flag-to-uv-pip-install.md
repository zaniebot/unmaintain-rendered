```yaml
number: 1436
title: "Add `--dry-run` flag to `uv pip install`"
type: pull_request
state: merged
author: JacobCoffee
labels:
  - enhancement
assignees: []
merged: true
base: main
head: 1244-pip-install-dry-run-flag
created_at: 2024-02-16T05:42:24Z
updated_at: 2024-03-12T06:19:45Z
url: https://github.com/astral-sh/uv/pull/1436
synced_at: 2026-01-10T14:49:08Z
```

# Add `--dry-run` flag to `uv pip install`

---

_Pull request opened by @JacobCoffee on 2024-02-16 05:42_

## What

Adds a `--dry-run` flag that ejects out of the installation process early (but after resolution) and displays only what *would have* installed

## Closes

Closes #1244 

## Out of Scope

I think it may be nice to include a `dry-run` flag for `uninstall` even though `pip` doesn't implement this... thinking `Would uninstall X packages: ...` 

---

_Converted to draft by @JacobCoffee on 2024-02-16 05:42_

---

_Comment by @JacobCoffee on 2024-02-16 07:17_

<img width="545" alt="image" src="https://github.com/astral-sh/uv/assets/45884264/7c9b6837-85a2-4a10-9cf4-9404d048174d">

```
➜ ./target/debug/uv pip install litestar --dry-run
Resolved 22 packages in 584ms
Would install 22 packages
 - msgspec==0.18.6
 - rich-click==1.7.3
 - pyyaml==6.0.1
 - python-dateutil==2.8.2
 - httpcore==1.0.3
 - polyfactory==2.14.1
 - httpx==0.26.0
 - click==8.1.7
 - certifi==2024.2.2
 - anyio==4.2.0
 - h11==0.14.0
 - six==1.16.0
 - sniffio==1.3.0
 - rich==13.7.0
 - faker==23.2.0
 - litestar==2.6.1
 - mdurl==0.1.2
 - pygments==2.17.2
 - typing-extensions==4.9.0
 - multidict==6.0.5
 - idna==3.6
 - markdown-it-py==3.0.0
```

---

_Comment by @JacobCoffee on 2024-02-16 07:17_

Sorry if this is a bad way to get the version? It felt bad, anyway...
I'm not sure if we do or can store version at resolution time and use it in `dry_run`, `install`, `validate`, etc.

---

_Comment by @JacobCoffee on 2024-02-16 07:41_

Might be cool to have a `dry-run` flag for uninstalling although not native to `pip`

---

_@JacobCoffee reviewed on 2024-02-16 07:54_

---

_Review comment by @JacobCoffee on `crates/uv/src/commands/pip_install.rs`:261 on 2024-02-16 07:54_

```suggestion
                    "+".blue(),
```
Not sure how this should look? `+` and `-` didn't seem to fit, though.. so I tried `~`

---

_Comment by @JacobCoffee on 2024-02-16 07:59_

On the failing CI, I had some failures do to order in stdout there... but I'm wondering how we keep it consistent?

---

_Marked ready for review by @JacobCoffee on 2024-02-16 07:59_

---

_Review requested from @zanieb by @zanieb on 2024-02-16 17:09_

---

_Assigned to @zanieb by @zanieb on 2024-02-16 17:09_

---

_Comment by @zanieb on 2024-02-17 04:24_

Hey @JacobCoffee, thanks for the pull request! I think we'll actually want to implement this a bit differently e.g. by passing the dry run flag into `install`

https://github.com/astral-sh/uv/blob/896ab1c54f4f4052555fa910d523f6899685eed2/crates/uv/src/commands/pip_install.rs#L477

then we can use our existing display which will also show which versions we would remove (and has a sorting implementation for deterministic output)

https://github.com/astral-sh/uv/blob/896ab1c54f4f4052555fa910d523f6899685eed2/crates/uv/src/commands/pip_install.rs#L642

We'll just need to tweak the `install` function to avoid doing any downloads, uninstalls, builds, or installs when the flag is set.

---

_Converted to draft by @zanieb on 2024-02-17 04:25_

---

_Comment by @JacobCoffee on 2024-02-20 23:25_

Does this look okay? also not 100% sure how to trigger re-downloads to test that bit.
Also, if it works out I can write the tests for it 

```
uv on  unst [📝🤷] via 🐋 colima via 🎁 v0.1.1 via  pyenv via ⚙️ v1.75.0
➜ ./target/debug/uv pip install litestar --dry-run
Resolved 22 packages in 48ms
DRY RUN The following packages would be installed from local cache:
 ~ litestar==2.6.1

uv on  unst [📝🤷] via 🐋 colima via 🎁 v0.1.1 via  pyenv via ⚙️ v1.75.0
➜ ./target/debug/uv pip install litestar --dry-run -n
Resolved 22 packages in 3.60s
DRY RUN The following packages would be downloaded:
 ~ litestar==2.6.1
```

<img width="590" alt="image" src="https://github.com/astral-sh/uv/assets/45884264/cba82728-5ba3-47d8-b406-7dcac3b2a655">


---

_Comment by @zanieb on 2024-02-22 02:13_

I'll give this a try tomorrow!

---

_Comment by @zanieb on 2024-02-23 00:28_

Hey! I got a chance to play with this today. I'm thinking we should match our normal output much more closely.

<img width="893" alt="Screenshot 2024-02-22 at 18 27 31" src="https://github.com/astral-sh/uv/assets/2586601/1f203b09-e49d-4936-8752-f11cb80ea055">

I put up a commit for you to build off of at https://github.com/astral-sh/uv/pull/1890

We still need more test coverage i.e. install a dependency of a package first then dry run install what do we show? or install an old version then dry run install with `--upgrade`




---

_Comment by @JacobCoffee on 2024-02-25 19:46_

Just for my notes, i am running into an issue where in testing it passes with 2 `$package==$version`, but in CLI usage i get 4 `====`
```
➜ ./target/debug/uv pip install litestar==2.0.0 --dry-run --strict
Resolved 17 packages in 974ms
Would download 2 packages
Would install 17 packages
 + anyio====4.3.0
 + litestar====2.0.0
 + faker====23.2.1
 + fast-query-parsers==1.0.3
```
But removing the `==` printer config from the `+`/`-` sections remove it from the testing :|

---

_Comment by @zanieb on 2024-02-28 18:35_

@JacobCoffee I'm on vacation this week but let me know when this is ready for review again

---

_Comment by @JacobCoffee on 2024-03-01 03:54_

 sorry ive not wrapped this - i think my remaining issue is the 4 ====, but ive just had surgery so im down for a bit on my hard thinking 😅 

---

_Comment by @zanieb on 2024-03-03 03:35_

I can give it a look! Get better :)

---

_Comment by @zanieb on 2024-03-04 23:10_

The issue is that `InstalledVersion` which comes from cached distributions includes `==` already (fixed by https://github.com/astral-sh/uv/pull/1436/commits/fee46b34aafd1fe400f38fa7ed0517885e2a332b).

---

_@zanieb reviewed on 2024-03-04 23:44_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:2306 on 2024-03-04 23:44_

Need to avoid panicking here

---

_Comment by @zanieb on 2024-03-06 02:19_

I think this is vaguely ready for review

---

_Review requested from @charliermarsh by @zanieb on 2024-03-06 02:19_

---

_@zanieb reviewed on 2024-03-06 02:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_install.rs`:749 on 2024-03-06 02:21_

I believe this is redundant as it is handled before this function is called

---

_@zanieb reviewed on 2024-03-06 02:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_install.rs`:801 on 2024-03-06 02:21_

Formatting needs fix

---

_Marked ready for review by @zanieb on 2024-03-07 17:43_

---

_@charliermarsh approved on 2024-03-12 00:37_

---

_Renamed from "feat(pip-install): add `--dry-run` flag" to "Add `--dry-run` flag to `uv pip install`" by @zanieb on 2024-03-12 06:19_

---

_Merged by @zanieb on 2024-03-12 06:19_

---

_Closed by @zanieb on 2024-03-12 06:19_

---

_Label `enhancement` added by @zanieb on 2024-03-12 06:19_

---
