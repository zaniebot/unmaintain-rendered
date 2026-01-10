---
number: 1510
title: "Expose `uv build` build frontend (`pypa/build` replacement)"
type: issue
state: closed
author: henryiii
labels:
  - enhancement
  - projects
assignees: []
created_at: 2024-02-16T16:26:50Z
updated_at: 2024-09-04T15:23:48Z
url: https://github.com/astral-sh/uv/issues/1510
synced_at: 2026-01-10T01:23:07Z
---

# Expose `uv build` build frontend (`pypa/build` replacement)

---

_Issue opened by @henryiii on 2024-02-16 16:26_

I'm curious if replacing `pypa/build` is something being considered. It would be a good idea, I think, to avoid implementing `pip wheel`, and instead focus on build. I think most of the components are already here - the ability to make temporary virtual environments is the key one. In fact, the ability to build (`pip install .` though config-settings are missing #1460) is here too. The procedure is simple, though you do need a Python interpreter to call the PEP 517 hooks, it's mostly making a venv, adding packagers, running a hook if it exists and adding those packages, then calling the build backend hook. There are a total of 5 hooks (wheel, sdist, requires for wheel and sdist, and a metadata hook). You can look at pypa/build, there's not that much code there, really. I'd be able to help a bit if needed.

The other direction would also be very, very interesting - build uses `venv` by default, but it can switch to `virtualenv` if that's installed. Is there a plan to provide a Python API for `uv virtualenv`? Then build could just switch to `uv` if that was installed, getting a speedup (I expect). Same would be true for all the other tooling built on `venv` and `virtualenv`, like nox, tox, hatch, pdm, etc., I expect.

---

_Comment by @edgarrmondragon on 2024-02-16 16:37_

> Is there a plan to provide a Python API for `uv virtualenv`? Then build could just switch to `uv` if that was installed, getting a speedup (I expect). Same would be true for all the other tooling built on `venv` and `virtualenv`, like nox, tox, hatch, pdm, etc., I expect.

Correct me if I'm wrong but I think those tools only interact with `venv` and `virtualenv` through the CLI, so `uv venv` could already be a drop-in replacement if it covers the same functionality.

---

_Comment by @zanieb on 2024-02-16 16:51_

We're already a build frontend, I think. We do all of this to build source distributions for users. We don't expose it as a top-level command, but it would make a lot of sense to do so.

Just for fun, there's also this #895 

Regarding a Python API — probably not anytime soon since it introduces more complexity around backwards compatibility.

---

_Label `enhancement` added by @zanieb on 2024-02-16 16:56_

---

_Label `project-management` added by @zanieb on 2024-02-16 16:56_

---

_Renamed from "Build replacement?" to "Expose build frontend (`pypa/build` replacement)" by @zanieb on 2024-02-16 16:56_

---

_Comment by @henryiii on 2024-02-16 22:02_

virtualenv has a very, very simple Python interface based on the CLI:

```python
cmd = [str(path), '--no-setuptools', '--no-wheel', '--activators', '']
result = virtualenv.cli_run(cmd, setup_logging=False)
executable = str(result.creator.exe)
script_dir = str(result.creator.script_dir)
```

I think the main reason it exists to skip the python process startup cost, which uv already skips. The only thing you'd need, actually, is a structured output option (say json?) that describes what it just created, then `uv` would be usable by build. That option could remove the pretty output and add structured output (replacing setup_logging=False).

---

_Referenced in [pypa/build#733](../../pypa/build/issues/733.md) on 2024-02-18 17:08_

---

_Referenced in [astral-sh/uv#1663](../../astral-sh/uv/issues/1663.md) on 2024-02-19 16:07_

---

_Comment by @henryiii on 2024-02-19 16:26_

What about the CLI? Here's how build works, and I'd recommend the same thing for `uv build` unless there's a good reason to do something different (build intentionally deviates from pip where a clear improvement could be made, so doing the same thing to build is valid!)

* `build` by default builds the current folder, and makes an SDist, then **builds the wheel from the SDist**. This helps catch broken SDists and has caught quite a few of them in practice.
* `build --sdist` only builds an SDist
* `build --wheel` only builds a wheel, and it builds it directly from the source.
* `build --sdist --wheel` builds and SDist and a wheel, **both from source**. This is different than not passing either flag.

Key options:

* `PATH`: The source to build, default so `.`
* `--outdir PATH`: defaults to `{srcdir}/dist`
* `--no-isolation, -n`: Like `--no-build-isolation` in pip. Though it does still check deps (see next flag).
* `--skip-dependency-check, -x`: Avoids checking the dependencies if building without isolation. This is the reverse of pip, which requires a flag to do the check.[^1]
* `--config-setting KEY[=VALUE], -C KEY[=VALUE]`: This is similar to pip, though they add an `s` to the long form (and the long form is ugly due to the double equals sign - the short form is better all around). `build` also allows empty settings (no `=VALUE`), while pip does not. I think all other inconsistencies have been removed by pip adopting our additions. Passing the same key more than once creates a list.


[^1]: I'm torn on this one. I like build's method, and pip would have followed build if it weren't for backward compatibility (they shortly did try, but back compat required it change to opt-in). But I also constantly have to argue with distributors explaining that it's valid to pass this flag if you have Python redistributions (like `cmake`, `ninja`, `patchelf`, etc) that aren't required outside of PyPI. A list of deps to ignore would really help, IMO.

---

_Comment by @zanieb on 2024-02-19 17:07_

Thanks for pushing this forward Henry. That makes sense to me, roughly. Personally, I'd be happy to review an experimental addition of this interface. We'll need to talk as a team though to figure out where this fits on our roadmap.

---

_Referenced in [astral-sh/uv#1681](../../astral-sh/uv/issues/1681.md) on 2024-02-19 17:42_

---

_Comment by @konstin on 2024-02-22 11:09_

We have `cargo run --bin uv-dev -- build` (https://github.com/astral-sh/uv/blob/2586f655bbf650a9797c8f88b6d9066eefe7a3dc/crates/uv-dev/src/build.rs) where we can experiment with the CLI. I'm somewhat hesitant to expose this now before we know how this fits into the bigger picture, we might want to have higher level command (let's say `prepare-venv` for docker, `publish` for libraries)

---

_Comment by @henryiii on 2024-02-22 15:34_

The nice thing about "build" is it could be just like "pip", a copy of the PyPA tool that everyone is already familiar with. And it's nicely low level, rather than a higher level concept like "publish". But the name is pretty general.

I assume there should be a way to build SDists in the future too? Also, I'd not use `-w, --wheels` as that's too close to `--wheel` from `build` (which I'd assume you'd want, see description above). Build calls this `--outdir, -o`. I'm not sure you want to expose `-e, --editable`; editable wheels are supposed to be transient and not exposed to the user / stored. We thought long and hard about build's CLI.

It's great that it can take SDists, but I'd name the positional parameter "SOURCE" since if you are building an SDist, it doesn't make sense, and even building a wheel, I think SOURCE would be fine for and SDist, but also include pointing at a source.

Also, I'd recommend emulating build's output, which is excellent and a good balance of detail vs. noise.

FYI, uv-dev builds builds's wheel in 571ms.

---

_Comment by @NMertsch on 2024-02-28 22:54_

Is this a duplicate of #920?

---

_Comment by @henryiii on 2024-02-28 23:39_

Kind of, though that refers to “pip build”, which isn’t a thing. And I’m specifically interested in mimicking pypa/build as closely as reasonable with “uv build”, just like “uv pip” and “uv venv”. 

---

_Comment by @zanieb on 2024-02-29 04:12_

I think this is a duplicate of #920 — it's the same idea of exposing our PEP 517 build frontend, the name of the command is up for consideration still.

---

_Referenced in [astral-sh/uv#920](../../astral-sh/uv/issues/920.md) on 2024-02-29 08:16_

---

_Comment by @WillDuke on 2024-03-10 15:28_

Echoing an earlier comment, the output from `pypa/build` is so good that I often install it just to debug failures in commands like `pip install -e .`. `build` will often provide an actionable error message (or other output) when the original command failed due to something inscrutable. It's so useful that I consider it a major feature of the tool.

---

_Referenced in [hynek/build-and-inspect-python-package#83](../../hynek/build-and-inspect-python-package/issues/83.md) on 2024-03-23 13:47_

---

_Referenced in [astral-sh/uv#6278](../../astral-sh/uv/issues/6278.md) on 2024-08-22 00:21_

---

_Referenced in [FinanceData/FinanceDataReader#225](../../FinanceData/FinanceDataReader/issues/225.md) on 2024-08-23 08:46_

---

_Referenced in [astral-sh/uv#5507](../../astral-sh/uv/issues/5507.md) on 2024-08-25 21:27_

---

_Comment by @chadrik on 2024-08-27 22:45_

What I'd like to see from `uv build` is recursively building all packages in nested workspaces.  I see a lot of potential in workspaces for bulk operations, but right now they're quite limited. 

---

_Referenced in [astral-sh/uv#6739](../../astral-sh/uv/issues/6739.md) on 2024-08-28 12:34_

---

_Renamed from "Expose build frontend (`pypa/build` replacement)" to "Expose `uv build` build frontend (`pypa/build` replacement)" by @zanieb on 2024-08-28 12:35_

---

_Comment by @zanieb on 2024-08-28 12:35_

Modifying the title to help with search as we're getting other requests.

---

_Comment by @alex on 2024-08-30 15:29_

One thing that'd be incredibly useful, which pypa/build does not currently support, is being able to turn an sdist into a wheel. Were `uv` to support that, we'd pretty much immediately switch to it I think.

---

_Comment by @astrojuanlu on 2024-08-30 16:25_

TIL https://github.com/pypa/build/issues/311

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-31 16:47_

---

_Comment by @charliermarsh on 2024-08-31 17:30_

I have this mostly working.

---

_Comment by @charliermarsh on 2024-08-31 17:43_

It's good motivation to add support for streaming the build output though. Feels essential!

---

_Referenced in [astral-sh/uv#6895](../../astral-sh/uv/pulls/6895.md) on 2024-08-31 17:53_

---

_Comment by @charliermarsh on 2024-08-31 23:56_

Ok, I have the build output working in https://github.com/astral-sh/uv/pull/6903, but unsure whether to show it by default or only with `--verbose`.

---

_Comment by @alex on 2024-08-31 23:59_

My intuition is that I basically always would want it when running uv build.

On Sat, Aug 31, 2024 at 7:56 PM Charlie Marsh ***@***.***>
wrote:

> Ok, I have the build output working in #6903
> <https://github.com/astral-sh/uv/pull/6903>, but unsure whether to show
> it by default or only with --verbose.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/1510#issuecomment-2323075334>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBEN3SRA2NZFPNZOELLZUJJ2RAVCNFSM6AAAAABDMJW74SVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGMRTGA3TKMZTGQ>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
>


-- 
All that is necessary for evil to succeed is for good people to do nothing.


---

_Comment by @charliermarsh on 2024-09-01 16:59_

Have that part working here: https://github.com/astral-sh/uv/pull/6912

---

_Closed by @charliermarsh on 2024-09-04 15:23_

---

_Closed by @charliermarsh on 2024-09-04 15:23_

---
