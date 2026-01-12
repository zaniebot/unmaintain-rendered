```yaml
number: 11613
title: "Close #9871: add uv self uninstall command"
type: pull_request
state: open
author: loic-lescoat
labels: []
assignees: []
draft: true
base: main
head: uv-self-uninstall-v2
created_at: 2025-02-19T03:16:36Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11613
synced_at: 2026-01-12T16:09:55Z
```

# Close #9871: add uv self uninstall command

---

_@loic-lescoat_

## Summary

Close #9871: add `uv self uninstall` command

Mimics behavior listed on [uv's official uninstallation instructions](https://docs.astral.sh/uv/getting-started/installation/#uninstallation)

## Test Plan

### How was this tested?

#### Using a smoke test in `.github/workflows/ci.yml`

#### Manually

This was tested manually on Linux as follows:

I ran `cargo build`, then:
I placed the following content into `test-linux-Dockerfile`

```test-linux-Dockerfile
FROM ubuntu:24.04

RUN apt update
RUN apt install -y curl
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

COPY target/debug/uv .
CMD bash
```

then started the container using:

```
docker build -t test-linux -f test-linux-Dockerfile . && docker run -it test-linux
```

Once inside the container, I ran the `uninstall` command
on empty directories to make sure the command succeeds:

```
$ /uv self uninstall --remove-data
No cache found at: /root/.cache/uv
$ echo $?
0
$ ls $HOME/.local/bin
env  env.fish
$ ls $HOME/.local/share
ls: cannot access '/root/.local/share': No such file or directory
$ ls $HOME/.local/
bin
```

I also tried calling `uv run python` and `uv tool run` at least once
so that the `uv tool dir` and `uv python dir` are emptied by `uv self uninstall --remove-data`

```
$ /uv run python -c 'print("hi")'
Python 3.13.2 (main, Feb  5 2025, 19:11:32) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
$ uv tool run jupyter --version
$ ls `/uv tool dir`
$ ls -a `/uv tool dir`
.  ..  .gitignore  .lock
$ ls -a `/uv python dir`
.  ..  .gitignore  .lock  .temp  cpython-3.13.2-linux-x86_64-gnu
$ ls -a `/uv cache dir`
.  ..  .gitignore  CACHEDIR.TAG  archive-v0  builds-v0  environments-v2  interpreter-v4  sdists-v7  sdists-v8  simple-v15  wheels-v4
$ /uv self uninstall --remove-data
Clearing cache at: /root/.cache/uv
Removed 18176 files (408.0MiB)
$ ls -a `/uv tool dir`
ls: cannot access '/root/.local/share/uv/tools': No such file or directory
$ ls -a `/uv python dir`
ls: cannot access '/root/.local/share/uv/python': No such file or directory
$ ls -a `/uv cache dir`
ls: cannot access '/root/.cache/uv': No such file or directory
```


---

_Renamed from "Close #9871: add uv self uninstall command" to "Close #9871: add `uv self uninstall` command" by @loic-lescoat on 2025-02-19 03:17_

---

_Renamed from "Close #9871: add `uv self uninstall` command" to "Close #9871: add uv self uninstall command" by @loic-lescoat on 2025-02-19 03:17_

---

_Comment by @zanieb on 2025-02-19 03:42_

We use `fs_err` instead of `std::fs` for better error messages.

---

_@zanieb reviewed on 2025-02-19 03:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/self_uninstall.rs`:38 on 2025-02-19 03:43_

Note instead of `.exe` you'll want the `std::env::consts` executable suffix.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:533 on 2025-02-19 03:44_

Can we call this something else? Maybe just `remove_data`?

---

_@zanieb reviewed on 2025-02-19 03:44_

---

_Comment by @zanieb on 2025-02-19 03:45_

Cool, thanks for contributing!

I'm not really sure how we'd write unit tests for this, but we can test it in CI, e.g., in the `smoke-test-linux` job in `ci.yml` 

---

_@zanieb reviewed on 2025-02-19 03:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/self_uninstall.rs`:39 on 2025-02-19 03:47_

Also note, the install path might not be `.local/bin`. Should we use the `current_exe()` method instead and assume `uvx` is next to `uv`?


---

_@zanieb reviewed on 2025-02-19 03:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/self_uninstall.rs`:13 on 2025-02-19 03:48_

I wonder if we should require the uv installer to have been used to enable this? Like we do in `self_update`

If so, that'd give us another way to determine the canonical install path.

---

_@loic-lescoat reviewed on 2025-02-19 07:05_

---

_Review comment by @loic-lescoat on `crates/uv/src/commands/self_uninstall.rs`:38 on 2025-02-19 07:05_

Thanks for the input! In [here](https://github.com/loic-lescoat/uv/blob/a057630db284857267d4f17d2270ea7988d86878/crates/uv/src/commands/self_uninstall.rs?plain=1#L38) I tried checking the value of `cfg!(windows)` but this is more succinct, so I'll try this

---

_@loic-lescoat reviewed on 2025-02-19 07:20_

---

_Review comment by @loic-lescoat on `crates/uv-cli/src/lib.rs`:533 on 2025-02-19 07:20_

done in 4104fc8a0

---

_Review comment by @loic-lescoat on `crates/uv/src/commands/self_uninstall.rs`:38 on 2025-02-19 07:20_

done in 4104fc8a0

---

_@loic-lescoat reviewed on 2025-02-19 07:20_

---

_@loic-lescoat reviewed on 2025-02-19 07:25_

---

_Review comment by @loic-lescoat on `crates/uv/src/commands/self_uninstall.rs`:39 on 2025-02-19 07:25_

Got it. While the [official un-installation instructions](https://docs.astral.sh/uv/getting-started/installation/#uninstallation) imply it is in `.local/bin`, this is fair, so I'll look into this

---

_Comment by @loic-lescoat on 2025-02-19 07:29_

TODO:

- [x] Relax assumption that binaries are in `.local/bin`
- [x] Prototype test of this feature in `smoke-test-linux job in ci.yml`
- [x] Evaluate whether we should `require the uv installer to have been used` to use `uv self uninstall`

@zanieb , instead of using `std::fs`, in a057630 I used `rm_rf` which ultimately calls `fs_err`. Is this OK? 

Thanks for your input. I'll address these items soon

---

_@loic-lescoat reviewed on 2025-02-19 07:31_

---

_Review comment by @loic-lescoat on `crates/uv/src/commands/self_uninstall.rs`:13 on 2025-02-19 07:31_

Good point. Is it fair to say that the uv installer was used iff `uv` was built with the `self-update` feature? In other words, is it reasonable to enable the `uv self uninstall` command iff the `uv self update` command is enabled?

Thanks.

---

_@zanieb reviewed on 2025-02-19 14:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/self_uninstall.rs`:39 on 2025-02-19 14:40_

That's the default install location, which is sufficient for documentation but not an automated process.

---

_@zanieb reviewed on 2025-02-19 14:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/self_uninstall.rs`:13 on 2025-02-19 14:40_

I think you can check for a receipt like the `update` command does.

> is it reasonable to enable the uv self uninstall command iff the uv self update command is enabled?

This also makes sense to me.


---

_@loic-lescoat reviewed on 2025-02-19 16:24_

---

_Review comment by @loic-lescoat on `crates/uv/src/commands/self_uninstall.rs`:39 on 2025-02-19 16:24_

Addressed in e96b0f081

Thanks!

---

_Comment by @loic-lescoat on 2025-02-21 06:57_

@zanieb as of [d4965af](https://github.com/astral-sh/uv/pull/11613/commits/d4965af3b14c146f345e74e53240e302a3af0bb9) I added a smoke test which runs `self-uninstall` in `ci.yml`; this [fails](https://github.com/astral-sh/uv/actions/runs/13451643537/job/37587044368?pr=11613#step:4:80) because the binary is built (e.g. [here](https://github.com/loic-lescoat/uv/blob/d4965af3b14c146f345e74e53240e302a3af0bb9/.github/workflows/ci.yml?plain=1#L431)) using `cargo build` rather than using `cargo build --features self-update`.

Which would you prefer?

1. Change the build from `cargo build` to `cargo build --features self-update`
2. Add a separate `build` process, that runs `cargo build --features self-update`

On the one hand, option 1. is an easier, less verbose fix and option 2. will lengthen the actions file and increase the number of Github actions.

On the other hand, the binary built using `cargo build` is used by other unrelated tests, e.g. [integration tests](https://github.com/loic-lescoat/uv/blob/d4965af3b14c146f345e74e53240e302a3af0bb9/.github/workflows/ci.yml?plain=1#L806-L822)

---

_Comment by @loic-lescoat on 2025-02-26 07:37_

> Which would you prefer?

@zanieb I tried going with option 1. - but the `cargo dev generate-all` test [fails](https://github.com/astral-sh/uv/actions/runs/13538593476/job/37834646801?pr=11613#step:4:62) with the message:

```
uv-dev failed
  Caused by: cli.md changed, please run `cargo dev generate-cli-reference`:
```

I tried running this command locally, as well as `cargo dev generate-all`, but this does not change the contents of cli.md, and instead prints `Up-to-date: cli.md`. Do you know why this is? Do you have an idea of how to make this test pass?

Thanks.

---

_@zanieb reviewed on 2025-02-26 19:18_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:433 on 2025-02-26 19:18_

I think it's fine to just enable `self-update` on the normal binary we are building / testing instead of creating a dedicated one. 

However, won't this testing approach fail if you add reading of the uv install receipt to verify that it was installed by our installer? I guess we could allow removing the _data_ regardless of whether or not uv is being managed by us? I think that's suggestive that we might not want to gate this behind the self-update feature at all?

---

_Comment by @zanieb on 2025-02-26 19:19_

> tried running this command locally, as well as cargo dev generate-all

Did you try merging the latest `main` into your branch? CI runs on the merge-commit which can be different than your local commit.

---

_@loic-lescoat reviewed on 2025-03-01 02:08_

---

_Review comment by @loic-lescoat on `.github/workflows/ci.yml`:433 on 2025-03-01 02:08_

> won't this testing approach fail if you add reading of the uv install receipt to verify that it was installed by our installer?

I don't know. I did not use an install receipt to check whether uv was installed by our installer.

> I guess we could allow removing the data regardless of whether or not uv is being managed by us?

I suppose so.

> I think that's suggestive that we might not want to gate this behind the self-update feature at all?

~I am OK with this. I will adapt accordingly, e.g. by reverting to [d354167 - the parent of `
can self uninstall iff can self update`](https://github.com/astral-sh/uv/pull/11613/commits/d3541676431066a6a2f6f184a0f65fc36e980738)~

Since I'm not using the "uv install receipt", I figure it's OK to gate this behind the self-update feature.

I added a self-update feature flag.

**Next steps**

- [x] Figure out workaround so that executable can remove itself on windows, see complications described in [self-delete](https://docs.rs/self-replace/latest/self_replace/#self-deletion)
- [x] ask for review

---

_Comment by @loic-lescoat on 2025-03-04 05:45_

Hi @zanieb, can you please review this again? Here are my updates:

1. On windows, instead of removing the `uv.exe` executable, we print a message to prompt the user to do this manually. This is because on Windows, an executable cannot remove itself, as mentioned [here](https://docs.rs/self-replace/latest/self_replace/) - this previously caused the windows smoke test to [fail](https://github.com/astral-sh/uv/actions/runs/13613457231/job/38053541522#step:3:104). A google search suggests that workarounds are not very robust (e.g. renaming the executable and scheduling its removal at a later date - see [this crate](https://github.com/mitsuhiko/self-replace) or [this stack overflow answer](https://stackoverflow.com/a/1606189)). As a result, I think a more robust solution is to prompt the user to remove the `uv.exe` binary manually on windows. This is why we prompt the user to manually remove the `uv.exe` binary rather than removing it.
2. The `cargo dev generate-all` test passes

Are you OK with merging this? I think it is ready.

Thanks

---

_Comment by @loic-lescoat on 2025-03-12 04:16_

@zanieb do you have a moment to review this PR? As mentioned in my [comment above](https://github.com/astral-sh/uv/pull/11613#issuecomment-2696285967), I would like to hear if you think this is ready to merge. I believe it is ready to be helpful to users.

Thank you.

---
