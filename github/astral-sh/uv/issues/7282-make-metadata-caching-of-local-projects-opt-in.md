---
number: 7282
title: Make metadata caching of local projects opt-in when it is dynamic?
type: issue
state: open
author: sbidoul
labels:
  - needs-design
  - needs-decision
assignees: []
created_at: 2024-09-11T06:15:56Z
updated_at: 2024-10-16T19:38:33Z
url: https://github.com/astral-sh/uv/issues/7282
synced_at: 2026-01-07T13:12:17-06:00
---

# Make metadata caching of local projects opt-in when it is dynamic?

---

_Issue opened by @sbidoul on 2024-09-11 06:15_

I regularly hit caching gotchas with projects using dynamic dependencies.

The last one, I think, was  `uv lock` not detecting changes even though `tool.uv.resinstall-package` was set. I had to use `uv lock --refresh-package`, and I could not set `tool.uv.refresh-package`.

These surprises drive my ask for e.g. https://github.com/astral-sh/uv/pull/7268
But even with that we'll still need to think about setting cache-keys in the first place.

I was wondering if it would make sense to cache less aggressively if any metadata is dynamic (either because the project table is absent, or `project.dynamic` is set)? Or not cache at all in ~such cases~ presence of dynamic metadata, unless cache-keys is set?

---

_Renamed from "Make metadata caching opt-in when version or dependencies are dynamic?" to "Make metadata caching opt-in when metadata is dynamic?" by @sbidoul on 2024-09-11 08:17_

---

_Renamed from "Make metadata caching opt-in when metadata is dynamic?" to "Make metadata caching opt-in when it is dynamic?" by @sbidoul on 2024-09-11 08:17_

---

_Renamed from "Make metadata caching opt-in when it is dynamic?" to "Make metadata caching of local projects opt-in when it is dynamic?" by @sbidoul on 2024-09-11 08:40_

---

_Comment by @charliermarsh on 2024-09-11 13:23_

The breaking point for me on this was `uv run`, which becomes very hard to use with dynamic projects if we're rebuilding and reinstalling them (typically needlessly) on every command. I don't want to have different caching semantics for `uv run` vs. other commands, so I decided to shift the burden towards requiring users to explicitly mark packages that they want to "always rebuild, no matter what".

An alternative is that we could consider adding a dedicated setting to the `pyproject.toml` like `tool.uv.dynamic` to opt a given package into this behavior (rather than having to add `reinstall-package` somewhere else).

---

_Comment by @sbidoul on 2024-09-11 14:09_

Or could `uv run` warn that 'the project is being rebuilt because there is dynamic metadata, consider setting cache-keys`?

So it would be correct by default, with an opt-in mechanism for caching.

Most people, who don't use dynamic metadata, would never see the warning.
And for people using dynamic metadata that would be a friendly hint to configure their project for correct caching.

---

_Comment by @sbidoul on 2024-09-11 14:22_

> `tool.uv.dynamic`

UX-wise is it not a bit strange, since there is already `project.dynamic` ?

---

_Comment by @sbidoul on 2024-09-11 14:37_

And by the way, sorry to appear insistent on this. The thing is that I'm a heavy user of dynamic metadata, but not a packaging newbie, and since I regularly fall in the traps, I suspect there is some UX issue that might be important.

I may not have said it yet, but I hugely appreciate the fantastic work you are doing!

---

_Comment by @charliermarsh on 2024-09-11 15:42_

Yeah, the answer here may indeed be to have different semantics for `uv run` vs. other commands. I'm worried that it would be confusing for users, but perhaps it's more confusing as-is. (Thank you for the kind words and no worries at all. I'm not happy with the state of dynamic metadata in uv either :))

---

_Label `needs-design` added by @charliermarsh on 2024-09-12 01:52_

---

_Label `needs-decision` added by @charliermarsh on 2024-09-12 01:52_

---

_Referenced in [astral-sh/uv#2844](../../astral-sh/uv/issues/2844.md) on 2024-09-13 12:56_

---

_Comment by @mmerickel on 2024-10-16 18:18_

As the author of #2844 which is pulled into here - I'm just frankly surprised that `uv run` has so many side effects such that it would be a blocker for decisions here. My focus is on `uv sync` and it's "obvious" to me that `uv sync` should reinstall packages that are installed in editable mode every single time `uv sync` is executed. That's what editable means, and how `pip install -e` has worked forever. I'm completely fine with `uv run` having different semantics here because it's simply not a command that should guarantee that everything is perfectly up to date in my mind. It's cool if it can provide some sanity checks or basic things, but `uv sync` is the "one true thing that should get the env in the right state" and that means that it should reinstall editable packages.

---

_Comment by @mmerickel on 2024-10-16 18:21_

To extend on that previous point a little bit with some anecdotes... the current recommended solution is for me to define `tool.uv.cache-keys` on each project, and in my case that means basically a cache-key of `**/*` which is going to run into performance problems eventually for `uv run` as well so we're kind of back where we started where `uv run` is just dictating too much of the contract here imo.

---

_Comment by @charliermarsh on 2024-10-16 18:21_

Is it that you want _all_ editables to be reinstalled every time? Or editables with dynamic metadata?

---

_Comment by @charliermarsh on 2024-10-16 18:22_

But in that case, why not just use `reinstall-package` for each of those packages?

---

_Comment by @mmerickel on 2024-10-16 18:25_

All editable packages imo. Python doesn't offer a separation for this aspect of the problem to claim it's just metadata. The issue here is with MANIFEST.in that is compiled down at build-time into the list of assets to copy into the project, and that only is matched AFTER the build happens and all the files are generated such that MANIFEST.in can match on them.

In my concrete example we have a `webassets` folder living externally to the python package. We have a custom setuptools hook that will compile the assets and put them into the python package in the right location. Then we have a MANIFEST.in that says to pull those assets in as package-data. And our devs need that to happen every time they run `uv sync` after a git pull or changing the files themselves locally.

---

_Comment by @mmerickel on 2024-10-16 18:28_

I can do it with cache-keys to be clear but it's globbing a lot of files that 1) I have to tediously maintain and 2) could cause performance problems for things like `uv run` as those globs get too large. I'd rather a default where I didn't need cache-keys, and `uv run` did what it does today (where it can be slightly out of sync because I didn't define cache-keys) but uv sync reinstalled editable packages and always left me in a good spot.

---

_Comment by @mmerickel on 2024-10-16 19:38_

I confirmed that `tool.uv.cache-keys` does work as we thought it would. It does have the downsides I mentioned above and does require each project to define how it should work with uv or the wrong thing happens. My desired behavior would be a different default. Basically if you don't define `tool.uv.cache-keys` on a project and it's editable then `uv sync` should reinstall it. If you do define `cache-keys` then that is a fast-path for uv to use.

---

_Referenced in [astral-sh/uv#12038](../../astral-sh/uv/issues/12038.md) on 2025-03-07 10:52_

---
