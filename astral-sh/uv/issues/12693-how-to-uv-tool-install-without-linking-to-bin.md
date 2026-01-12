```yaml
number: 12693
title: "How to `uv tool install` without linking to bin?"
type: issue
state: closed
author: mathomp4
labels:
  - question
assignees: []
created_at: 2025-04-06T17:54:51Z
updated_at: 2025-07-16T13:49:11Z
url: https://github.com/astral-sh/uv/issues/12693
synced_at: 2026-01-12T16:01:10Z
```

# How to `uv tool install` without linking to bin?

---

_@mathomp4_

### Question

All,

I help manage a package ([`mepo`](https://github.com/GEOS-ESM/mepo)) and I install it on various systems. On those systems, I manage which version of `mepo` is used via modulefiles and I currently install via `pipx`, but I'm learning `uv`.

Reading through the `uv` docs/help, I found out about `UV_TOOL_DIR` so I thought "Oh, I can use that":

```
❯ which mepo
mepo not found
❯ UV_TOOL_DIR=/Users/fortran/versioned-mepo/2.3.0 uv tool install mepo==2.3.0
Resolved 3 packages in 0.80ms
Installed 3 packages in 2ms
 + colorama==0.4.6
 + mepo==2.3.0
 + pyyaml==6.0.2
Installed 1 executable: mepo
❯ lt /Users/fortran/versioned-mepo/2.3.0/mepo/bin/mepo
Permissions Size User    Group Date Modified    Name
.rwxr-xr-x   330 fortran staff 2025-04-06 13:48  /Users/fortran/versioned-mepo/2.3.0/mepo/bin/mepo*
```

But, I was surprised (though maybe I shouldn't have been) that a symlink was added in the usual `~/.local/bin` space:
```
❯ which mepo
/Users/fortran/.local/bin/mepo
```
And then let's say a new version comes along (though here I'll use an older one):
```
❯ UV_TOOL_DIR=/Users/fortran/versioned-mepo/2.2.0 uv tool install mepo==2.2.0
Resolved 3 packages in 1ms
Installed 3 packages in 3ms
 + colorama==0.4.6
 + mepo==2.2.0
 + pyyaml==6.0.2
error: Executable already exists: mepo (use `--force` to overwrite)
```

Again, on the systems I'd use this on, I'd write a Lua modulefile and it would add to `PATH`. So I'm wondering, is there a way I can `uv tool install` but *NOT* have the symlink made in `~/.local/bin`. I mean, there is `link-mode` which maybe controls this (not sure) but there isn't a `none` method.

I fully understand this might be a use case `uv` isn't really meant for, but...maybe?

### Platform

Darwin 24.4.0 arm64

### Version

uv 0.6.12 (Homebrew 2025-04-02)

---

_Label `question` added by @mathomp4 on 2025-04-06 17:54_

---

_Comment by @charliermarsh on 2025-04-06 18:02_

You might be looking for https://docs.astral.sh/uv/configuration/environment/#uv_tool_bin_dir?

---

_Comment by @mathomp4 on 2025-04-06 18:23_

> You might be looking for [docs.astral.sh/uv/configuration/environment#uv_tool_bin_dir](https://docs.astral.sh/uv/configuration/environment/#uv_tool_bin_dir)?

@charliermarsh I guess my thought was that the binary already at:

```
/Users/fortran/versioned-mepo/2.3.0/mepo/bin/mepo
```

if I install with:
```
UV_TOOL_DIR=/Users/fortran/versioned-mepo/2.3.0 uv tool install mepo==2.3.0
```

Then in my Lua modulefile I can just do:
```
prepend_path("PATH","/Users/fortran/versioned-mepo/2.3.0/mepo/bin")
```
and get it. 

Though I guess I could "double up" a la:
```
❯ UV_TOOL_DIR=/Users/fortran/versioned-mepo/envs/2.3.0 UV_TOOL_BIN_DIR=/Users/fortran/versioned-mepo/bins/2.3.0 uv tool install mepo==2.3.0
Resolved 3 packages in 1ms
Installed 3 packages in 2ms
 + colorama==0.4.6
 + mepo==2.3.0
 + pyyaml==6.0.2
Installed 1 executable: mepo
warning: `/Users/fortran/versioned-mepo/bins/2.3.0` is not on your PATH. To use installed tools, run `export PATH="/Users/fortran/versioned-mepo/bins/2.3.0:$PATH"` or `uv tool update-shell`.
```
I mean, it does work. Though seems a bit ... extra.

---

_Comment by @mathomp4 on 2025-04-06 18:24_

ETA: I just tried the "obvious" thing but `uv` was *not* happy:

```
❯ UV_TOOL_DIR=/Users/fortran/versioned-mepo/envs/2.3.0 UV_TOOL_BIN_DIR=/dev/null uv tool install mepo==2.3.0
Resolved 3 packages in 1ms
Installed 3 packages in 3ms
 + colorama==0.4.6
 + mepo==2.3.0
 + pyyaml==6.0.2
error: Failed to create executable directory
  Caused by: failed to create directory `/dev/null`: File exists (os error 17)
```
I mean, I didn't expect it to work, but...I mean, isn't `/dev/null` the answer to all problems. 😄 


---

_Comment by @mathomp4 on 2025-04-06 18:30_

I did just try one more thing thinking this might work but:
```
❯ UV_TOOL_BIN_DIR=/Users/fortran/versioned-mepo/bins/2.3.0 uv tool install mepo==2.3.0
Resolved 3 packages in 1ms
Installed 3 packages in 2ms
 + colorama==0.4.6
 + mepo==2.3.0
 + pyyaml==6.0.2
Installed 1 executable: mepo
warning: `/Users/fortran/versioned-mepo/bins/2.3.0` is not on your PATH. To use installed tools, run `export PATH="/Users/fortran/versioned-mepo/bins/2.3.0:$PATH"` or `uv tool update-shell`.
~/modulefiles with zsh at 02:28:23 PM
❯ UV_TOOL_BIN_DIR=/Users/fortran/versioned-mepo/bins/2.2.0 uv tool install mepo==2.2.0
Resolved 3 packages in 1ms
Uninstalled 1 package in 3ms
Installed 1 package in 2ms
 - mepo==2.3.0
 + mepo==2.2.0
Installed 1 executable: mepo
warning: `/Users/fortran/versioned-mepo/bins/2.2.0` is not on your PATH. To use installed tools, run `export PATH="/Users/fortran/versioned-mepo/bins/2.2.0:$PATH"` or `uv tool update-shell`.
```

Seems like `uv` is too smart. It uninstalled one version when I tried to install another.

But the `UV_TOOL_DIR` + `UV_TOOL_BIN_DIR` combo does seem to work. I'll work on it and figure out a directory tree that is aesthetically pleasing...

---

_Closed by @charliermarsh on 2025-07-16 13:49_

---
