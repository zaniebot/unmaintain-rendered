---
number: 6965
title: Installation of uv fails when $HOME variable is not set
type: issue
state: closed
author: kishaningithub
labels:
  - external
  - releases
assignees: []
created_at: 2024-09-03T10:22:27Z
updated_at: 2025-06-09T15:49:27Z
url: https://github.com/astral-sh/uv/issues/6965
synced_at: 2026-01-10T01:24:08Z
---

# Installation of uv fails when $HOME variable is not set

---

_Issue opened by @kishaningithub on 2024-09-03 10:22_

When I Tried running the following command (mentioned in the docs) to install uv in amazon linux box where $HOME directory is unset for reasons unknown i get a failure saying $HOME is unset

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Example - https://github.com/kishaningithub/setup-python-amazon-linux/issues/14#issuecomment-2325627058



---

_Renamed from "Installation of uv when $HOME variable is not set" to "Installation of uv fails when $HOME variable is not set" by @kishaningithub on 2024-09-03 10:25_

---

_Label `release` added by @zanieb on 2024-09-03 17:41_

---

_Label `upstream` added by @zanieb on 2024-09-03 17:41_

---

_Comment by @zanieb on 2024-09-03 17:42_

Thanks for the report. We'll raise this with the upstream dependency that generates the installer.

What do you expect to happen? We install the binary in a directory in `$HOME`.

---

_Comment by @kishaningithub on 2024-09-04 07:21_

I think it would be great if we can pass in an argument to the shell script to explicitly state in which directory the installation should happen. That way the $HOME dependency can disappear.

---

_Comment by @jooon on 2024-09-04 11:01_

uv itself seems to default to /root when $HOME is missing, for its cache, managed python and tools, if I install to /bin in an otherwise empty docker container.

---

_Comment by @janosh on 2024-09-04 21:55_

same problem here. working with `uv` on multiple HPC clusters where users are instructed to create large files under `/scratch/<user>` (which is on a faster FS), not `/home/<user>`

---

_Comment by @zsimic on 2024-09-06 16:52_

I ran into this as well, and ended up defining a HOME env var. My use case is auto installing `uv` in a specific folder, using `CARGO_DIST_FORCE_INSTALL_DIR`, and `HOME` in my case ideally shouldn't be involved at all. I see it's used to generate a receipt file (which is also not needed in this case).

What would be nice for automations like this is if the installer script simply dropped the correct `uv` binary in the specified target folder, and that's it (no PATH modifications, no HOME involved etc)

---

_Referenced in [axodotdev/cargo-dist#1400](../../axodotdev/cargo-dist/issues/1400.md) on 2024-09-07 19:29_

---

_Comment by @kishaningithub on 2024-09-09 06:02_

An example proposal of how the installation api can look like ( inspired from [go-releaser installation](https://golangci-lint.run/welcome/install/#other-ci) )

```bash
## Put uv binary in the /bin folder under current directory (defaults to current implementation if not specified)
curl -LsSf https://astral.sh/uv/install.sh | sh -s -- -b $(pwd)/bin

## Reproducible builds by specifying exact version (defaults to latest if not specified)
curl -LsSf https://astral.sh/uv/install.sh | sh -s -- -b $(pwd)/bin v0.4.7
```


---

_Comment by @jooon on 2024-09-09 12:21_

Just noticed that this incantation works to install version 0.4.7 of uv and uvx to /usr/bin without writing any env files.

```
curl -LsSf https://astral.sh/uv/0.4.7/install.sh | UV_INSTALL_DIR=/usr sh
```

Setting UV_INSTALL_DIR=/ so it writes to /bin sort of works, but it appends a slash, so it becomes //bin which is not in $PATH, which makes the installer write /env and /env.fish as well.


---

_Comment by @ashleygwilliams on 2024-09-09 14:39_

hey folks! I work on cargo-dist which is what the astral team uses to distribute `uv`, really appreciate the discussion on this article and i've opened up an issue on our tracker to fully gather requirements for the install mode ya'll are discussing here. 

https://github.com/axodotdev/cargo-dist/issues/1400

would love for you to share ideas there (i'll also continue monitoring this issue for ideas!)

---

_Comment by @charliermarsh on 2024-10-30 20:24_

You can now set `UV_UNMANAGED_INSTALL` to define a standalone installation directory, e.g.:

```
export UV_UNMANAGED_INSTALL=/foo/bar
curl -LsSf https://astral.sh/uv/install.sh | sh -s -- -v --no-modify-path
```

---

_Closed by @charliermarsh on 2024-10-30 20:24_

---

_Comment by @kishaningithub on 2024-10-31 17:07_

@charliermarsh Can this also be updated in the main documentation site?

---

_Comment by @ashleygwilliams on 2024-10-31 17:25_

@charliermarsh with `UV_UNMANAGED_INSTALL` you won't need to pass `--no-modify-path` as that's behavior rolled into the env var

---

_Comment by @zanieb on 2024-10-31 17:44_

@kishaningithub see https://docs.astral.sh/uv/configuration/installer/#unmanaged-installations

---

_Comment by @parthshyara on 2024-12-17 10:04_

@charliermarsh The issue is not yet resolved. Getting the following error.
```
> curl -LsSf https://astral.sh/uv/install.sh | sh

sh: line 53: HOME: unbound variable
```

This is present in the latest install script. The failing code line is `RECEIPT_HOME="${HOME}/.config/uv"` which is unconditionally trying to resolve `$HOME` always.

---

_Comment by @charliermarsh on 2024-12-17 13:11_

@parthshyara -- I believe you need to set `UV_UNMANAGED_INSTALL`.

---

_Comment by @mil-ad on 2025-03-25 15:55_

@charliermarsh @zanieb  Even if you set `UV_UNMANAGED_INSTALL` the script still chokes on line 53 because `$HOME` is not defined:

```
RECEIPT_HOME="${XDG_CONFIG_HOME:-$HOME/.config}/uv"
```

Here's the output of the installer with `set -x`:

```
+ [  = Version JM 93t+ 2010-03-05 ]
+ set -u
+ APP_NAME=uv
+ APP_VERSION=0.6.9
+ [ -n  ]
+ INSTALLER_BASE_URL=https://github.com
+ [ -n  ]
+ ARTIFACT_DOWNLOAD_URL=https://github.com/astral-sh/uv/releases/download/0.6.9
+ PRINT_VERBOSE=0
+ PRINT_QUIET=0
+ [ -n  ]
+ NO_MODIFY_PATH=0
+ [ 0 = 1 ]
+ INSTALL_UPDATER=1
+ UNMANAGED_INSTALL=/usr/local/bin
+ [ -n /usr/local/bin ]
+ NO_MODIFY_PATH=1
+ INSTALL_UPDATER=0
+ read -r RECEIPT
/tmp/i.sh: 55: HOME: parameter not set
```

---

_Comment by @mo18 on 2025-03-26 03:45_

@mil-ad - I'm trying to install into `/usr/local/bin`, so I created the following environment variables to get it to install properly:

```
XDG_CONFIG_HOME=/usr/local/.uvconfig
UV_INSTALL_DIR=/usr/local/bin
```

This gets the installer to complete successfully. I haven't tested uv yet, but I'm thinking the `XDG_CONFIG_HOME` directory will need to persist for the app to work properly.

---

_Comment by @mil-ad on 2025-03-26 08:41_

Same, I just defined a dummy `HOME="foobar"` along with `UV_UNMANAGED_INSTALL` and it runs fine. The only problem is that one line. 

---

_Comment by @cr3ative on 2025-05-28 10:24_

@charliermarsh Just to confirm, this is still an issue - I've applied the fix @mil-ad suggested to get this working, but yeah, on Amazon Linux 2023 (or another OS where $HOME isn't set), the installer cannot run, even in `UV_UNMANAGED_INSTALL` set. Latest version.

---

_Reopened by @konstin on 2025-05-28 10:49_

---

_Referenced in [astral-sh/cargo-dist#38](../../astral-sh/cargo-dist/pulls/38.md) on 2025-05-28 11:04_

---

_Referenced in [kishaningithub/setup-python-amazon-linux#24](../../kishaningithub/setup-python-amazon-linux/pulls/24.md) on 2025-06-04 10:00_

---

_Closed by @Gankra on 2025-06-09 15:49_

---

_Closed by @Gankra on 2025-06-09 15:49_

---

_Referenced in [astral-sh/uv#14156](../../astral-sh/uv/pulls/14156.md) on 2025-06-20 18:06_

---
