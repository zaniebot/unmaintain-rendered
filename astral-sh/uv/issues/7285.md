```yaml
number: 7285
title: " Failed to hardlink files: Issue with ruff cache"
type: issue
state: closed
author: PhantomSpike
labels:
  - question
assignees: []
created_at: 2024-09-11T09:13:52Z
updated_at: 2025-12-04T07:03:02Z
url: https://github.com/astral-sh/uv/issues/7285
synced_at: 2026-01-10T03:23:52Z
```

#  Failed to hardlink files: Issue with ruff cache

---

_Issue opened by @PhantomSpike on 2024-09-11 09:13_

When I run `uv add` or `uv pip install` I get this warning:
```
░░░░░░░░░░░░░░░░░░░░ [0/6] Installing wheels...                                                                                                                                            warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
  ```
  and uv is very slow. What could be causing this?
  
  Could it be that the uv cache is somewhere different from my current directory? If so, how do I move/link it properly?
  
  Thanks!



---

_Comment by @charliermarsh on 2024-09-11 13:18_

You can set `UV_CACHE_DIR` to some directory on the same drive as your project. You can run `uv cache dir` to view the current cache location too.

---

_Label `question` added by @charliermarsh on 2024-09-11 13:18_

---

_Comment by @notatallshaw-gts on 2024-09-11 16:27_

FYI, I need to set my cache to a different drive than my Python environment (for reasons), I use `UV_LINK_MODE=symlink` and it works great for me.

---

_Closed by @charliermarsh on 2024-09-14 00:43_

---

_Comment by @Quba1 on 2024-11-08 12:20_

Sorry for reviving this issue. Does `This may lead to degraded performance` part of the warning message refer to the uv performance or installed package performance?

---

_Comment by @konstin on 2024-11-08 12:21_

This only affects uv's performance, not the installed packages.

---

_Comment by @pplmx on 2025-02-27 08:33_

Hi @konstin,

I noticed the use of `$env:UV_LINK_MODE="symlink"`. Is this the configuration officially recommended by uv on Windows? If so, could you explain why it isn’t enabled by default?

Thanks for your insights!

---

_Comment by @charliermarsh on 2025-02-27 13:50_

No, symlinks aren’t recommended. We recommend hard links, which is the default.

The issue with symlinks is that if you remove files from the cache or clear the cache, your environment will break. Hard links don’t have that problem.

---

_Comment by @kamiertop on 2025-03-14 13:24_

On windows, I had the same problem, I used `uv cache dir` and found that the cache was in `C:Users\xxx\AppDataloca\pip`, and then I set the environment variables `UV_CACHE_DIR`, putting the cache directory and the project directory in a **same disk partition**. Also, I use `zsh` on windows, so I write env to `~/.vimrc`, hope it helps.

---

_Comment by @rsconsuegra on 2025-04-14 00:50_

Sorry to open again this thread, but I am using uv in windows and, for reasons, I have multiple projects in different locations (E:, F: and G:, and cache is currently in C:\Users\xxx\AppData\Local\uv\cache), so right now I get the warning ` Failed to hardlink files;` in all the projects, and certainly I don't think having a cache in each disk would be the intended solution... or is it? 

---

_Comment by @pengyou200902 on 2025-05-27 01:51_

> Sorry to open again this thread, but I am using uv in windows and, for reasons, I have multiple projects in different locations (E:, F: and G:, and cache is currently in C:\Users\xxx\AppData\Local\uv\cache), so right now I get the warning ` Failed to hardlink files;` in all the projects, and certainly I don't think having a cache in each disk would be the intended solution... or is it?

same situation here

---

_Comment by @charliermarsh on 2025-05-27 02:16_

Yeah, if you want to use hardlinks, you'd need a separate cache for each drive, since you can't hardlink across drives. Alternatively, if you want to use a single cache, you can set `UV_LINK_MODE=copy`. This _could_ be better depending on your use-case, since you can avoid re-downloading shared dependencies; however, it will be _less_ space efficient than using a separate cache per-disk with hardlinks, since the hardlink solution only requires one copy of each file per disk, while `UV_LINK_MODE=copy` will require one copy for every installation.

---

_Comment by @charliermarsh on 2025-05-27 02:17_

(If you have further questions, please redirect them to Discord or open a new issue; we prefer that folks don't comment on closed issues.)

---

_Comment by @dima333750 on 2025-12-04 07:03_

I fixed one warning by moving to disk C:


---
