```yaml
number: 8420
title: "Use XDG (i.e. `~/.local/bin`) instead of the Cargo home directory in the installer"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
  - releases
assignees: []
merged: true
base: tracking/050
head: zb/install-location
created_at: 2024-10-21T17:19:03Z
updated_at: 2024-10-24T20:14:41Z
url: https://github.com/astral-sh/uv/pull/8420
synced_at: 2026-01-10T12:54:09Z
```

# Use XDG (i.e. `~/.local/bin`) instead of the Cargo home directory in the installer

---

_Pull request opened by @zanieb on 2024-10-21 17:19_

Reviving https://github.com/astral-sh/uv/pull/2236

Basically implements https://github.com/axodotdev/cargo-dist/issues/287

---

_Label `breaking` added by @zanieb on 2024-10-21 17:19_

---

_Comment by @zanieb on 2024-10-21 17:59_

I don't really know how to test this.

---

_Comment by @zanieb on 2024-10-21 18:09_

Okay so... testing this out


```
cargo dist build
npx http-server -o ./target/distrib
```

then edit the installer to use `ARTIFACT_DOWNLOAD_URL="http://127.0.0.1:8080/target/distrib/"`

give a happy installation at the new location

```
❯ ./target/distrib/uv-installer.sh
downloading uv 0.4.25 aarch64-apple-darwin
installing to /Users/zb/.local/bin
  uv
  uvx
everything's installed!
```

unfortunately uv is not at the front of the path

```
❯ which uv
/Users/zb/.cargo/bin/uv
```

after restarting my shell it is though

```
# Restart shell
❯ which uv
/Users/zb/.local/bin/uv
```

using `XDG_BIN_HOME` works

```
❯ XDG_BIN_HOME=/tmp/test ./target/distrib/uv-installer.sh
downloading uv 0.4.25 aarch64-apple-darwin
installing to /tmp/test
  uv
  uvx
everything's installed!

To add /tmp/test to your PATH, either restart your shell or run:

    source /tmp/test/env (sh, bash, zsh)
    source /tmp/test/env.fish (fish)
❯ ls /tmp/test
env		env.fish	uv		uvx
```

using `XDG_DATA_HOME` works

```
❯ XDG_DATA_HOME=/tmp/test ./target/distrib/uv-installer.sh
downloading uv 0.4.25 aarch64-apple-darwin
installing to /tmp/test/../bin
  uv
  uvx
everything's installed!

To add /tmp/test/../bin to your PATH, either restart your shell or run:

    source /tmp/test/../bin/env (sh, bash, zsh)
    source /tmp/test/../bin/env.fish (fish)
❯ ls /tmp/test
❯ ls /tmp/bin
env		env.fish	uv		uvx
```


---

_Comment by @zanieb on 2024-10-21 18:13_

Some pain points:

1. There's no warning if the newly installed `uv` is not first on the path, i.e., another uv binary will be used instead of the one I just installed.
2. The installer adds `env` files to the bin — is that what we want? They're not overridden, which is nice, but I presume that could also lead to confusing behavior?

    ```
    ❯ touch /tmp/test/env
    ❯ XDG_BIN_HOME=/tmp/test ./target/distrib/uv-installer.sh
    downloading uv 0.4.25 aarch64-apple-darwin
    installing to /tmp/test
      uv
      uvx
    everything's installed!
    ❯ cat /tmp/test/env
    ```

3. Relative paths don't display as absolute, i.e., for `XDG_DATA_HOME` the installer displays `/tmp/test/../bin/env`

---

_Label `releases` added by @zanieb on 2024-10-21 18:15_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-21 18:15_

---

_Comment by @zanieb on 2024-10-21 18:19_

Requires some docs changes still

---

_Added to milestone `v0.5.0` by @zanieb on 2024-10-22 02:38_

---

_Review requested from @konstin by @zanieb on 2024-10-22 12:12_

---

_@konstin approved on 2024-10-22 12:14_

---

_@charliermarsh reviewed on 2024-10-24 19:10_

---

_Review comment by @charliermarsh on `Cargo.toml`:334 on 2024-10-24 19:10_

Are these checked in order?

---

_Comment by @charliermarsh on 2024-10-24 19:12_

Agree, per Discord, that (3) seems fixable in cargo-dist. I could PR it if needed.

---

_Comment by @charliermarsh on 2024-10-24 19:13_

For my own understanding, where does `$XDG_DATA_HOME/../bin` come from? (I know we check this already.)

---

_@charliermarsh reviewed on 2024-10-24 19:14_

---

_Review comment by @charliermarsh on `Cargo.toml`:334 on 2024-10-24 19:14_

Nevermind, all clear! https://opensource.axo.dev/cargo-dist/book/reference/config.html#install-path

---

_Comment by @charliermarsh on 2024-10-24 19:17_

It's interesting that the env files are added _within_ the install directory? For Cargo it looks like it's `/Users/crmarsh/.cargo/bin/uv` and then `/Users/crmarsh/.cargo/env` (one level up).

---

_Comment by @charliermarsh on 2024-10-24 19:19_

The `env` file thing seems like the most unfortunate to me... Should we ask for a way to disable that?

---

_Comment by @charliermarsh on 2024-10-24 19:20_

It seems kind of problematic because now `env` will be a binary on `PATH`?

---

_Comment by @zanieb on 2024-10-24 19:26_

> For my own understanding, where does `$XDG_DATA_HOME/../bin` come from? (I know we check this already.)

`XDG_BIN_HOME` isn't officially a part of the standard. So it's sort of a "next best" approximation of where we should put binaries.

---

_Comment by @zanieb on 2024-10-24 19:28_

Regarding `env` — yeah it's not great, but it doesn't shadow the `env` executable because it's not executable

```
❯ PATH="/tmp/bin:$PATH" which env
/usr/bin/env
❯ /tmp/bin/env
zsh: permission denied: /tmp/bin/env
```

I think it's ~okay.

---

_Comment by @charliermarsh on 2024-10-24 19:30_

Is it worth trying to get it removed?

---

_Comment by @zanieb on 2024-10-24 19:31_

In favor of what? It still needs to go somewhere for activation purposes right? My main concern is that it could collide with another `env` file in that directory, but at least it doesn't overwrite it. I think that probably won't be common anyway.

---

_Comment by @charliermarsh on 2024-10-24 19:32_

Sorry, why is it necessary?

---

_Comment by @charliermarsh on 2024-10-24 19:33_

Is it sourced from `.zshrc` or similar?

---

_Comment by @zanieb on 2024-10-24 19:34_

It's what they use for adding the install directory to the `PATH` in shells without duplicating the entry, e.g., in my `.zshrc`:

```
. "$HOME/.cargo/env"
```


---

_Comment by @charliermarsh on 2024-10-24 19:35_

So if you run `XDG_BIN_HOME=/tmp/test ./target/distrib/uv-installer.sh`, does it add `. /tmp/test/env` to your `.zshrc`? (Just making sure I understand.)

---

_Comment by @charliermarsh on 2024-10-24 19:36_

Does it always do that, or only if it's not on `PATH` already?

---

_Comment by @zanieb on 2024-10-24 19:54_

As far as I understand, it's the same as the existing behavior (just in a flat directory, which honestly makes more sense to me anyway).

```
❯ export XDG_BIN_HOME=/tmp/example-bin-home
❯ ./target/distrib/uv-installer.sh
downloading uv 0.4.25 aarch64-apple-darwin
installing to /tmp/example-bin-home
  uv
  uvx
everything's installed!

To add /tmp/example-bin-home to your PATH, either restart your shell or run:

    source /tmp/example-bin-home/env (sh, bash, zsh)
    source /tmp/example-bin-home/env.fish (fish)
❯ cat /tmp/example-bin-home/env
#!/bin/sh
# add binaries to PATH if they aren't added yet
# affix colons on either side of $PATH to simplify matching
case ":${PATH}:" in
    *:"/tmp/example-bin-home":*)
        ;;
    *)
        # Prepending path in case a system-installed binary needs to be overridden
        export PATH="/tmp/example-bin-home:$PATH"
        ;;
esac
❯ cat ~/.zshrc | grep "example-bin-home"
. "/tmp/example-bin-home/env"
```

---

_@charliermarsh approved on 2024-10-24 20:09_

---

_Comment by @charliermarsh on 2024-10-24 20:09_

Sorry to repeat, but does it add to `.zshrc` if the directory is already on PATH?

---

_Comment by @zanieb on 2024-10-24 20:12_

Ah I thought you meant did the `env` script duplicate the entry. No, it looks skips creating the `env` files and modifying the shell config at all:

```
❯ export XDG_BIN_HOME="/tmp/example-3"
❯ export PATH="/tmp/example-3:$PATH"
❯ ./target/distrib/uv-installer.sh
downloading uv 0.4.25 aarch64-apple-darwin
installing to /tmp/example-3
  uv
  uvx
everything's installed!
❯ cat ~/.zshrc | grep "example-3"
❯ ls /tmp/example-3
uv	uvx
```


---

_Comment by @charliermarsh on 2024-10-24 20:13_

Thanks!

---

_Merged by @zanieb on 2024-10-24 20:14_

---

_Closed by @zanieb on 2024-10-24 20:14_

---

_Branch deleted on 2024-10-24 20:14_

---
