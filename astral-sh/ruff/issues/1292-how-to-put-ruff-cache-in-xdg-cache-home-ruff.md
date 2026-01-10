```yaml
number: 1292
title: how to put .ruff_cache in $XDG_CACHE_HOME/ruff
type: issue
state: closed
author: jessebot
labels:
  - configuration
assignees: []
created_at: 2022-12-19T19:29:21Z
updated_at: 2025-05-20T08:53:20Z
url: https://github.com/astral-sh/ruff/issues/1292
synced_at: 2026-01-10T11:09:43Z
```

# how to put .ruff_cache in $XDG_CACHE_HOME/ruff

---

_Issue opened by @jessebot on 2022-12-19 19:29_

I would like to have ruff cache go in `$XDG_CACHE_HOME/ruff` if it's configured (by default it could go in `$HOME/.cache/ruff`).

This is according to the [XDG Base Directory Spec](https://wiki.archlinux.org/title/XDG_Base_Directory#User_directories) for user directories.

Is this already an option and I missed it in the README? (recently started the journey to clean up my home directory)

Thank you for your work on ruff :) 🙏 

---

_Comment by @charliermarsh on 2022-12-19 20:28_

That makes sense! It's not supported right now, but it could be.

I need to see how other tools handle this. I'm mostly wondering if the configuration setting should be, like, `ruff_cache_dir = /path/to/dir`, or `global_cache = true` (which would then use `$XDG_CACHE_HOME/ruff` or equivalent on other platforms). Any opinion or guidance on that?

(I prefer the former since it's more flexible and could enable things like shared caching on CI.)


---

_Label `configuration` added by @charliermarsh on 2022-12-19 20:28_

---

_Comment by @jessebot on 2022-12-19 23:08_

Thanks for the quick response! :)

## Suggestion

In my opinion, you can still make the cache directory independently configurable, but go in this order of preference:

1. `ruff_cache_dir`
Use if `$RUFF_CACHE_DIR` env var exists (or configured in cli option/a config file)
Use **even if `$XDG_CACHE_HOME` is present**

2. `$XDG_CACHE_HOME/ruff`
Use if the `$XDG_CACHE_HOME` env var exists, but `ruff_cache_dir` is not set in the ways in number 1.

3. `$HOME/.cache/ruff`
Fall back to this if neither `ruff_cache_dir` nor `$XDG_CACHE_HOME` exists

Side note: What I can't figure out is if windows has different defaults for `$XDG` variables 🤷, but macOS and Linux defaults seem to be `$HOME/.cache/$application`

## References
I haven't started with rust yet, but in Python, I use this simple module ([srstevenson/xdg](https://github.com/srstevenson/xdg)) in my projects, which might be helpful to take a peak at.

I linked the Arch wiki, because it's a bit easier to read, and has more info, but this is the official spec... I think (freedesktop.org is hard to navigate 🤷):
https://specifications.freedesktop.org/basedir-spec/latest/ar01s03.html

Finally, if you want to look at other tools, [this list](https://wiki.archlinux.org/title/XDG_Base_Directory#Supported) shows many common programs that adhere to the spec and gives the reference of the code or issue or docs and then includes notes on easy workarounds or compromises that were taken, which might be useful. It's been really helpful in the journey to clean up my home directory. Feels like tidying my office desk junk drawer. 😌 

---

_Comment by @charliermarsh on 2022-12-19 23:23_

Awesome, thank you!

Definitely agree on checking `RUFF_CACHE_DIR `.

We use the [`dirs`](https://docs.rs/dirs/4.0.0/dirs/fn.cache_dir.html) crate right now which has a `cache_dir` method, which returns a platform-specific cache directory. So in theory we would get a cache directory on ~every OS. Only thing I'm unsure of is whether we should use that user cache directory by default for all operating systems, or only when `XGD_CACHE_HOME` is set as suggested above (which seems reasonable).


---

_Comment by @charliermarsh on 2022-12-19 23:36_

I'm not seeing `XDG_CACHE_HOME` support in Mypy (though there is this issue from a few years ago: https://github.com/python/mypy/issues/8790), or ESLint, or Prettier, which would suggest they expect you to set the cache dir as an env var or a user setting. Trying to find other examples of similar tools and how they make these decisions!

---

_Comment by @charliermarsh on 2022-12-20 00:05_

My current plan is to follow Mypy and support:

1. A `--cache-dir` command-line flag, with highest precedence.
2. A `RUFF_CACHE_DIR` environment variable, with second-highest precedence.
3. A `cache-dir` setting in `pyproject.toml`, with lowest precedence.

`cache-dir` will support environment variable and home directory expansion.


---

_Comment by @jessebot on 2022-12-20 11:14_

Okay, yeah, I think that's good. I mostly use ruff via the [ALE](https://github.com/dense-analysis/ale) plugin for vim/neovim, so if ruff will read from `$RUFF_CACHE_DIR`, then I can just add `export RUFF_CACHE_DIR=$XDG_CACHE_HOME/ruff` in my .bashrc and call it done. If the shell env variable can't do variable expansion though (since it was only mentioned in number 3 of the list), we should just make sure to document that, so users know they need to do this instead:

```bash
#  /home/username can be whatever $HOME is for that OS (for instance /Users/username/ on macOS)
export RUFF_CACHE_DIR=/home/username/.cache/ruff
```

If I'm understanding everything here, then I think this looks like a good plan :)

---

_Comment by @charliermarsh on 2022-12-21 05:11_

Okay cool, sounds good! Meant to get to this today but some other things came up. If I can't do the full refactor tomorrow, I'll at least ship Item 2 on that list (env var), since the CLI argument and `pyproject.toml` setting are purely additive anyway.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-21 19:07_

---

_Closed by @charliermarsh on 2022-12-21 19:24_

---

_Comment by @charliermarsh on 2022-12-21 19:24_

Okay, `RUFF_CACHE_DIR` will go out in a sec. (The other improvements I'm tracking in #1313.)

---

_Comment by @WhyNotHugo on 2025-02-24 13:32_

Any thoughts on adding support for `XDG_CACHE_HOME`, which is pretty standard?

It's kind of annoying when applications need a bespoke variable to reaffirm an existing and explicit global setting.

---

_Comment by @tuttle on 2025-05-20 08:53_

Ok, thank you for the env var, @charliermarsh .
I'm setting `RUFF_CACHE_DIR=~/.cache/ruff` in my `.bashrc`.
As I have `.cache` symlinked to a ramdisk, this does not fill my SSD.

It was not previously explicitly stated, but can I assume the cache data will always bear a project distinction somehow in them, meaning I can **share the cache directory between unrelated projects**?
Thanks.

---
