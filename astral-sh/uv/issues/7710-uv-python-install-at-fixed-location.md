---
number: 7710
title: uv python install at fixed location?
type: issue
state: closed
author: mikeckennedy
labels: []
assignees: []
created_at: 2024-09-26T14:38:32Z
updated_at: 2025-02-18T15:49:30Z
url: https://github.com/astral-sh/uv/issues/7710
synced_at: 2026-01-10T01:24:18Z
---

# uv python install at fixed location?

---

_Issue opened by @mikeckennedy on 2024-09-26 14:38_

Hi team,

I'm looking to rework the Talk Python installing Python guide to recommend uv rather than the myriad of options needed for all the platforms now.

One thing that seems clunky for `uv python install` is that I don't see a way to make a particular Python version the default one. 

I'm not suggesting taking over the system Python or anything like that. But since each install managed by uv has a very specific path, e.g.:

`.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/`

Any update to the version (even build number) means whatever path you set in your system will become invalid. 

For example, if we could `uv python install 3.12.6 --primary` or something, and have uv put a link or copy in some well known location such as `.local/share/uv/python/primary` then we can add that to the top of the path if we wish and when we run `python`/`python3` it'll always find this one we designate as primary.

Any thinking along these lines?

BTW, yes, I know uv is venv-first and that's great. But if we are using it get Python on machines for super beginners, having the option to just type `python` to get a REPL and that sort of thing would be nice instead of full blown installers.

---

_Comment by @zanieb on 2024-09-26 14:49_

I think you're sort of looking for https://github.com/astral-sh/uv/issues/6265

---

_Comment by @mikeckennedy on 2024-09-26 15:36_

Hi @zanieb Yes, sort of. As long as we can get the shim at the very tip of the path. Xcode put Python in my path (3.9), Homebrew put 3.12 in my path. But I don't want either of those as the authoritative `python` for me. I'd like whatever I do from `uv python install` to be the one.

---

_Comment by @zanieb on 2024-09-26 16:24_

We can't necessarily control what the tip of your path is, that's up to how you've configured your shell. We can warn if it's not going to be at the front though.

---

_Comment by @mikeckennedy on 2024-09-26 18:47_

Hey @zanieb 

Of course you can't control our paths, that's what ever we do to our machines. 

My request here is that uv provide some mechanism to use a stable directory structure for a selected version of Python that it installed. In this way, we can put something fixed, rather than something that changes bi-monthly, into rc files or windows path settings that will stay working as Python evolves (3.13.0 -> 3.13.1, etc).

---

_Comment by @Shackelford-Arden on 2024-10-02 17:39_

From reading around a bit, I'm getting the sense that a number of folks (myself included) seem to be generally in favor of getting the functionality/purpose that `pyenv` serves for a lot of us added to uv so that we can fully replace the need of `pyenv`. 

Could still be in my own little dreamland though 😅 

---

_Comment by @zanieb on 2024-10-02 17:51_

We're working on it! See #7677 

---

_Comment by @Shackelford-Arden on 2024-10-02 17:53_

Ahh sweet. Hadn't found that one yet! Subscribed! 😏 Thanks

---

_Comment by @zanieb on 2024-11-07 00:01_

Hey @mikeckennedy, this is sort of available in preview now via #8458 and https://github.com/astral-sh/uv/pull/8650

---

_Assigned to @zanieb by @zanieb on 2024-11-07 00:01_

---

_Comment by @mikeckennedy on 2024-11-07 07:44_

Thanks @zanieb Yes, `uv python install --default` sounds like what I was really searching for. I'm looking forward to playing with it soon!

---

_Comment by @mikeckennedy on 2024-11-07 07:44_

Also, feel free to close this issue if you also think those features cover this use-case. I do think it will.

---

_Closed by @zanieb on 2025-02-18 15:49_

---

_Referenced in [vllm-project/vllm#15302](../../vllm-project/vllm/pulls/15302.md) on 2025-03-21 19:54_

---
