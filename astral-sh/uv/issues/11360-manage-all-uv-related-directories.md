---
number: 11360
title: Manage all uv-related directories
type: issue
state: open
author: Ma-Shell
labels:
  - enhancement
assignees: []
created_at: 2025-02-09T19:05:27Z
updated_at: 2025-10-10T08:45:47Z
url: https://github.com/astral-sh/uv/issues/11360
synced_at: 2026-01-10T01:25:04Z
---

# Manage all uv-related directories

---

_Issue opened by @Ma-Shell on 2025-02-09 19:05_

### Summary

Note: This issue is somewhat related to #6067, especially this [comment](https://github.com/astral-sh/uv/issues/6067#issuecomment-2291319522), mentioning that the `UV_PYTHON_INSTALL_DIR` option might be exposed to `uv.toml`, as well as #6506 but extends it to further directories, adds some further reasoning and has a generally wider scope, so I opted for creating a new issue.

UV uses multiple directories. The documentation refers to these 5 but there might be more which I missed.

| Directory  | command               | environment variable    | uv.toml/pyproject.toml |
| ---------- | --------------------- | ----------------------- | ---------------------- |
| cache      | `uv cache dir`        | `UV_CACHE_DIR`          | cache-dir              |
| python     | `uv python dir`       | `UV_PYTHON_INSTALL_DIR` | -                      |
| python bin | `uv python dir --bin` | `UV_PYTHON_BIN_DIR`     | -                      |
| tools      | `uv tool dir`         | `UV_TOOL_DIR`           | -                      |
| tools bin  | `uv tool dir --bin`   | `UV_TOOL_BIN_DIR`       | -                      |


There are several improvements for managing these:
- Add a command which lists all of them for more transparency with the user
- For the ones, where there is no .toml-entry available, one should be added. This would help especially when using a global `uv.toml`, to allow to define one base-directory for everything, without relying on environment variables.
- While the documentation mentions that there is a platform-dependent location for a default `uv.toml`, it would be beneficial to have uv also check the uv-executable's directory for a `uv.toml`, which again would help with having everything uv-related in one directory.

### Example

The proposed features allow having a portable installation giving the user more control about what paths are touched by UV.

Being able to manage the directories in the toml-file without relying on environment variables also helps for having globally installed projects in multi-user environments as the default-locations refer to directories within the user's profile (like %APPDATA% or ~/.local) which are not readable by all users.

---

_Label `enhancement` added by @Ma-Shell on 2025-02-09 19:05_

---

_Comment by @eabase on 2025-02-25 07:50_

This is a great idea and would align better with my proposal of also managing venv's here:
#11773 


---

_Referenced in [astral-sh/uv#13605](../../astral-sh/uv/issues/13605.md) on 2025-05-23 15:03_

---

_Comment by @mjnd1 on 2025-06-05 08:29_

Mark it down. When will it be completed?


---

_Comment by @konstin on 2025-06-05 12:08_

Another option here would be an env var named something like `UV_DATA_ROOT` that, if set, defines a directory where uv creates all of its directories (cache, tools, python interpreter, config) below it.

---

_Referenced in [astral-sh/uv#13950](../../astral-sh/uv/issues/13950.md) on 2025-06-10 14:57_

---

_Comment by @LOYINuts on 2025-08-08 14:15_

Yeah we need that desperately. Something like `UV_DATA_ROOT` will be great 

---

_Comment by @konstin on 2025-08-27 13:50_

A more concise approach than setting the `UV_*_DIR` variables is setting the `XDG_*` variables that uv uses. By controlling `XDG_DATA_HOME`, `XDG_CACHE_HOME`, `XDG_CONFIG_HOME`, and `XDG_BIN_HOME`, you can control the non-venv files and directories that uv writes.

| Variable           | Default Path         | Description                                                                       |
|--------------------|----------------------|-----------------------------------------------------------------------------------|
| `$XDG_DATA_HOME`   | `$HOME/.local/share` | Files written by uv that need  to be persisted (tools, python interpreters, etc.) |
| `$XDG_CACHE_HOME`  | `$HOME/.cache`       | Cache, ephemeral files that can be removed when uv isn't running                 |
| `$XDG_CONFIG_HOME` | `$HOME/.config`      | Where uv finds `uv.toml`                                                          |
| `$XDG_BIN_HOME`    | `$HOME/.local/bin`   | Binaries uv adds to `PATH` (uv itself, installed tools, python interpreters)      |

If that approach works for your use cases, we can add it to the docs. We're also working on a general reference documentation of uv directories (https://github.com/astral-sh/uv/pull/13976).

---

_Comment by @Ma-Shell on 2025-08-28 14:11_

Thank you for the response! While it is certainly a good idea to add this information to the documentation, this issue is mostly about the ability to use the config file instead of environment variables and to automatically use the config file in the executable's directory as a first choice, though.

---

_Comment by @LOYINuts on 2025-09-12 05:14_

I mean there are too many variables, just like you mentioned before, one variable like `UV_DATA_ROOT` will be great and much convenient

---

_Comment by @eabase on 2025-09-12 12:29_

For the love of god, please don't default to `$HOME/.random` for UV configuration files. 

**Especially not these locations:** 
```bash
$HOME/.local/*
$HOME/.cache
$HOME/.config
```

These are locations most often used with Nix based tools, like MSYS, Cygwin, WSL etc. or tools that have been ported from there. In those environments they have very specific functions, just like `.git` and `.ssh` has. I'm not sure at what point in time Dev's started thinking that these locations were at their own convenient disposal, and freely available for install/bloat. They are not!

If you wanna default, at least put it in your own place. 
I strongly suggest using: **`$HOME/.uv/*`** instead.

Also, using new environment variables prefixed by `XDG_` is very confusing to most users. The variable doesn't say sh-t what it is about and what software it relates to. At least use a **`UV_`** prefix. 

---

_Comment by @Winand on 2025-09-12 14:54_

> using new environment variables prefixed by `XDG_` is very confusing to most users

@eabase there's a spec for those X Desktop Group variables (except for [`XDG_BIN_HOME`](https://gist.github.com/roalcantara/107ba66dfa3b9d023ac9329e639bc58c)) [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/latest/#variables)

---

_Comment by @eabase on 2025-09-12 15:46_

@Winand 

> there's a spec for those X Desktop Group variables...

**Which is exactly why you shouldn't mix up UV locations with X Windows!**

Remind you that X-Windows has always been a nightmare to configure, and part of the reason why this structure got so messed up in the first place. People rebuilding broken structures based on old habits, because of laziness to rethink and make something more future proof and self contained. (Read the very first [comment](https://gist.github.com/roalcantara/107ba66dfa3b9d023ac9329e639bc58c?permalink_comment_id=4521894#gistcomment-4521894) (from your link) about linking `XDG_` vars to `MacOS` directories. Madness.)

> Freedesktop.org is a project to work on interoperability and shared base technology for free-software desktop environments for the X Window System (X11) and Wayland on Linux and other Unix-like operating systems. 


**So what make you think using X11 variables is a great idea for cross platform use of UV?**

I just fail to see any advantage at all, so please fill us in.

IMO users should never have to (or be forced) to think where their <your-package> customization & settings are located and installed. They should also be safe for cross machine migration and updates. 

You're basing your UX ideology from a fringe Linux PoV. The reality is that Python has exploded across all platforms, but most noticeable on Windows. So pushing a *mixed* file / environment to most non-technical end users, doesn't make sense. And if I was selfish, I would also agree to use the Linux structure, but that is again already broken since long ago. Just google on `/opt`, `/usr/shared/`, `/usr/local` etc etc. and you will get as many different answers as there are nix distros. 

---

<img width="618" height="249" alt="Image" src="https://github.com/user-attachments/assets/9a29bfe5-2c85-4730-bc6f-5b8022d164a6" />


---


The OP suggestion makes full sense, so why are you trying to derail that suggestion to something else? 
(I.e. using completely different variable names, unrelated to UV.)


---

_Comment by @konstin on 2025-09-12 17:44_

We chose the `XDG_` environment variables as they are the only set of configuration that are already well-known to users and that are supported by a wide range of (CLI) applications. For the default values, when no `XDG_` variables are set, we were originally closer to the platform default, but eventually decided to for using a more Linux-style cross-platform for consistency. XDG and the conventions around it map well to the kind of tasks uv does, e.g., Windows does not have a native directory for adding binaries to PATH as non-admin.

(This is kinda separate from the OP about having a portable uv installation with associated `uv.toml` settings)

---

_Comment by @eabase on 2025-09-14 13:02_

@konstin and (*Astral*)

I now see where this is going. (Nowhere)

There are 18 upvotes and even more positive feedback for OP suggestion. Yet, you insist changing the OP suggestion to something completely different, while either not understanding my above arguments at all, or choosing to ignore it. 

> ... but eventually decided to for using a more Linux-style cross-platform for consistency.

I just showed you above that the linux-style is **not** cross-platform consistent...

> Windows does not have a native directory for adding binaries to PATH as non-admin.

This comment make no sense at all. 
- Windows has a SYSTEM and USER path, and the USER path can be edited by normal user. The USER path is added to system, when a new shell is started.
- As normal user you can still install stuff anywhere (except under `C:\Windows`), unless you are on a managed server or locked-down corporate machine. 
- **>99.9 %** of Windows users are on their own machines and a very large proportion of them, have UAC completely turned off as it's a useless annoyance at best, while providing zero additional security.
- Package installed binaries should **never** be kept in your `HOME` directory! This is something which I have mentioned repeatedly in various `uv` threads here, and seem to be a consistently ignored and avoided point. 

*I think I'm done here.*

I have given my best feed-back for keeping uv sane and useful also for the future. As soon as this start to impact my productivity in any way, meaning having to manually tweak environment variables and/or looking for install locations, or bloating my `$HOME` directory, etc., I will stop using uv. 


---

_Comment by @zanieb on 2025-09-14 13:55_

@eabase the tone of your responses is not acceptable here, please take some time to consider how you're communicating.

I've marked the comments as "off-topic", as I don't understand why we're debating whether or not uv should be using the XDG specification here. We already respect these variables, we're not going to drop support for it — @konstin's intent was to share a less verbose way to configure uv's storage directories.

---

_Referenced in [astral-sh/uv#15834](../../astral-sh/uv/issues/15834.md) on 2025-09-14 14:00_

---

_Referenced in [astral-sh/uv#15836](../../astral-sh/uv/issues/15836.md) on 2025-09-14 14:04_

---

_Referenced in [astral-sh/uv#15837](../../astral-sh/uv/issues/15837.md) on 2025-09-14 14:07_

---

_Referenced in [astral-sh/uv#15838](../../astral-sh/uv/issues/15838.md) on 2025-09-14 14:09_

---

_Comment by @zanieb on 2025-09-14 14:10_

I've opened dedicated issues tracking each of your requests here @Ma-Shell, as you've asked for several distinct changes that we can discuss and address separately.

For those following this issue, please upvote the ones relevant to you.

---

_Referenced in [astral-sh/uv#15839](../../astral-sh/uv/issues/15839.md) on 2025-09-14 14:12_

---

_Comment by @Ma-Shell on 2025-09-14 14:52_

Thank you for taking this into consideration! I noticed that the toml-option for `UV_PYTHON_INSTALL_DIR` is missing. Since you split `UV_TOOL_DIR` (#15834) and `UV_TOOL_BIN_DIR` + `UV_PYTHON_BIN_DIR` (#15838) into two separate issues, do you think, it makes sense to add this to one of these two or have a separate issue?

---

_Comment by @zanieb on 2025-09-14 14:54_

The `UV_PYTHON_INSTALL_DIR` request has an existing issue at #6506

---

_Comment by @LOYINuts on 2025-09-14 17:13_

But can we have a big env variable? Setting all of these would be inconvenient. And if this variable can be set in `uv.toml` would be awesome.(But if this cant be done then the separate variables can be set in `uv.toml` and i will accept that 😄 )

---

_Comment by @Elypha on 2025-09-18 03:28_

Many users seem to be encountering issues with directory management, likely because the current documentation lacks clarity on two key points:

1. The number of distinct storage locations used by uv.
2. The hierarchical relationships among these locations (a top-down way, while the current documentation did a good job but only in a bottom-up manner)

For instance, `UV_TOOL_DIR` functions as a subdirectory within `DATA_HOME`, which means for most users, these can be considered a single logical location.

While I am not in favour of the XDG specification, it is not inherently difficult to comprehend. I believe people will have fewer issues if only the relationships between these directories were sorted out and presented clearly, perhaps a simple ASCII tree diagram will just suffice.

---

_Comment by @woutervh on 2025-10-10 08:45_

> But can we have a big env variable? 

Some tools do that, but it is annoying to mix config/cache/state  in a single directory.
Ipython-profiles with hardcoded directories are setting a bad example here.


I would like a setting or env-var to set the location of the build distributions  
from `<project-dir>/dist` to  `<project-dir>/var/dist`
For now you can only set `--out-dir`  on the cli.

---

_Referenced in [alienator88/Pearcleaner#373](../../alienator88/Pearcleaner/pulls/373.md) on 2025-10-12 19:07_

---

_Referenced in [astral-sh/uv#15954](../../astral-sh/uv/pulls/15954.md) on 2025-10-14 10:24_

---
