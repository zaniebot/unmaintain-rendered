```yaml
number: 3248
title: "Request: Allow --output-file, --annotation-style, etc. multiple times, only use right-most"
type: issue
state: open
author: AndydeCleyre
labels:
  - cli
assignees: []
created_at: 2024-04-24T16:02:50Z
updated_at: 2025-11-13T05:04:25Z
url: https://github.com/astral-sh/uv/issues/3248
synced_at: 2026-01-10T03:23:52Z
```

# Request: Allow --output-file, --annotation-style, etc. multiple times, only use right-most

---

_Issue opened by @AndydeCleyre on 2024-04-24 16:02_

`requirements.in`:

```
requests
```

```console
$ uv pip compile requirements.in -o reqs1.txt -o reqs2.txt
error: the argument '--output-file <OUTPUT_FILE>' cannot be used multiple times
$ uv --version
uv 0.1.37
$ uname
Linux
```

I'm requesting that instead, the above command write output to (only) `reqs2.txt`, the latest instance of `-o`/`--output-file` on the command line. This is useful when using shell aliases or functions to wrap `uv`, when you specify a "default" output file in the alias or function, while allowing any invocation to easily override that default by specifying the option again, on the command line.

That's how pip-tools behaves, and I make use of that behavior.

---

_Comment by @zanieb on 2024-04-24 17:08_

I think I'd find this behavior really confusing. What if we provided a `UV_COMPILE_OUTPUT_FILE` environment variable instead that you could override with the command line?

---

_Comment by @zanieb on 2024-04-24 17:09_

This would also be resolved by #3049 / #1511  I presume?

---

_Comment by @AndydeCleyre on 2024-04-24 17:41_

A file-based configuration or global configuration would not work for my case, which is functions that have their own defaults but are separate from any other ways the user wishes to use pip or uv.

With `UV_COMPILE_OUTPUT_FILE`, let me think: if `uv` is the backend being used I could add my own parsing of all the options, find and remove all output flags, use the last one to set `UV_COMPILE_OUTPUT_FILE`, and run my commands. It *might* be workable for my main case, but adds a lot of complication for the `uv` code path. Certainly not suitable for aliases.

FWIW, this is a very common convention (later options "win"), and is used not just by pip-tools but also pip itself. For example, the following command only uses the `cache2` folder:

```console
$ pip install --cache-dir="$PWD/cache1" requests --cache-dir="$PWD/cache2"
```

---

_Comment by @zanieb on 2024-04-24 18:05_

> Certainly not suitable for aliases.

I think you'd just write a simple bash function instead of an alias that sets a default value for the variable before calling into `uv`?

> FWIW, this is a very common convention (later options "win"), and is used not just by pip-tools but also pip itself. For example, the following command only uses the cache2 folder:

I think with something like `--cache-dir` it's much clearer that a single cache directory can be used whereas with `--output-file` I would be uncertain if multiple output files will be written.


---

_Comment by @AndydeCleyre on 2024-04-24 18:17_

FWIW here's maybe a more analogous option from pip:

```console
$ pip install --dry-run --quiet --report=out1.json requests --report=out2.json
```

As expected, only `out2.json` is written.

---

_Comment by @AndydeCleyre on 2024-04-24 18:42_

Similarly, both busybox `wget` and GNU `wget` use the latest-provided options for output file.

---

_Comment by @AndydeCleyre on 2024-04-24 18:57_

I'll note that I am having similar troubles with e.g. `--annotation-style`, which I expected to work like it does in pip-tools (latest wins). I won't open a separate issue for that at this time, as it gets to the same larger question.

---

_Comment by @hmc-cs-mdrissi on 2024-04-25 00:42_

I suspect part of this comes from argparse's design. Many python libraries use argparse/similar library. argparse's default behavior is last one wins with no awareness of meaning flag.

edit: Personally I like current duplicates are hard error over last one wins. My guess is it's commonness in python ecosystem is less that clis made choice individually, but argument handling libraries default choices made it common.

---

_Comment by @AndydeCleyre on 2024-04-25 02:49_

Yup, argparse does it. So does Click. So does [go-flags](https://github.com/jessevdk/go-flags). And [bazel](https://bazel.build/docs/user-manual). Looks like [rustc](https://github.com/rust-lang/compiler-team/issues/731) will be doing it (if I'm interpreting that correctly).

---

_Comment by @charliermarsh on 2024-04-25 02:54_

Is this the same? https://github.com/clap-rs/clap/issues/4261

---

_Comment by @AndydeCleyre on 2024-04-25 03:10_

Yeah, looks like exactly that.

---

_Comment by @charliermarsh on 2024-04-25 03:13_

I think you can set this globally with something like https://docs.rs/clap/latest/clap/struct.Command.html#method.args_override_self.

---

_Label `cli` added by @charliermarsh on 2024-04-26 00:23_

---

_Renamed from "Request: Allow --output-file multiple times, only use right-most" to "Request: Allow --output-file, --annotation-style, etc. multiple times, only use right-most" by @AndydeCleyre on 2024-10-25 15:24_

---

_Comment by @AndydeCleyre on 2024-10-29 20:29_

I probably should have posted these questions here instead of #3259:

Would either of the following make this change more palatable to those against it?

- Warn on stderr clarifying which option is being used, when single-use options occur more than once
- Only enable this when a new flag to do so is explicitly provided

---

_Comment by @AndydeCleyre on 2024-11-14 15:39_

If this change is more likely to happen limited to a certain subset of flags, I'll list here the ones I most wish to override later (*most* likely to break usage of my project when subbing uv for pip-tools, though not at all exhaustive):

`uv pip compile`:

- `--no-header`
- `--annotation-style`
- `-o`

---

_Comment by @AndydeCleyre on 2024-11-24 22:04_

@zanieb 

Is there any form of this that has been suggested, or that you can come up with, which you're willing to allow?

---

_Comment by @zanieb on 2024-11-25 23:46_

I don't think adding this to a subset of options would be great — consistency across the CLI seems important.

I do find your ecosystem examples convincing, but I still worry about the possibility of confusion. I'm just not seeing enough interest to merit changing this right now. (We have many issues with upvotes but nobody else has asked for this change yet) 

---

_Comment by @AndydeCleyre on 2024-12-10 18:20_

I expect it's just a matter of time, as my project was relatively quick to adopt and adapt to uv, so I hold out hope that this will change eventually. In the meantime I'll add one more widely used example: `git`.

```console
$ alias gd="git diff --submodule=log"
$ gd --submodule=diff  # this works, as expected
```

---

_Comment by @AndydeCleyre on 2025-03-03 03:26_

> I don't think adding this to a subset of options would be great — consistency across the CLI seems important.

I [see now](https://github.com/astral-sh/uv/issues/11205#issuecomment-2637986124) this is the behavior for some options, but not the ones I'm hoping for here.

---

_Comment by @AndydeCleyre on 2025-04-22 02:08_

Now that pylock.toml files can be generated by specifying the output file, I have an additional longing to use my wrapper functions but sometimes override the output file argument.

What's wrong with making the behavior pip- and pip-tools- compatible, while adding a big fat warming?

---

_Comment by @zanieb on 2025-04-22 02:37_

We're in the same situation as before, where there are not really other people asking for this and it would be a fundamental change to the behavior of the CLI.

---

_Comment by @AndydeCleyre on 2025-11-12 19:01_

> I don't think adding this to a subset of options would be great — consistency across the CLI seems important.

In addition to the exceptions in the current implementation [noted above](https://github.com/astral-sh/uv/issues/11205#issuecomment-2637986124), I notice that this is currently implemented for `--upgrade` and `--no-upgrade`:

```console
$ uv --version
uv 0.9.8 (85c5d3228 2025-11-07)
$ uv venv .venv
$ . ./.venv/bin/activate
$ uv pip install 'wrapt<2.0.0'
Resolved 1 package in 99ms
Prepared 1 package in 54ms
Installed 1 package in 7ms
 + wrapt==1.17.3
$ uv pip install --upgrade --no-upgrade wrapt
Audited 1 package in 0.85ms
$ uv pip list
Package Version
------- -------
wrapt   1.17.3
$ uv pip install --no-upgrade --upgrade wrapt
Resolved 1 package in 119ms
Prepared 1 package in 79ms
Uninstalled 1 package in 0.48ms
Installed 1 package in 8ms
 - wrapt==1.17.3
 + wrapt==2.0.1
```

It's already a guessing game which options this will work for, and there's not an existing consistent behavior that enabling this compatibility with the `pip` and `pip-tools` interfaces uv approximately clones would disrupt.

---

_Comment by @zanieb on 2025-11-12 19:17_

Doing this for inverse boolean flags is different than doing it for the same option repeated. We actually _need_ to do it that way to allow things like `UV_UPGRADE=1 uv ... --no-upgrade` to work, that's just a detail of Clap's design.

---

_Comment by @AndydeCleyre on 2025-11-13 05:04_

OK, `--upgrade --no-upgrade` isn't *technically* a repeated option, though I think of it as equivalent to a hypothetical `--upgrade=yes --upgrade=no`.

But there is some allowance of technically repeated flags here:

```console
$ uv pip install --no-upgrade --upgrade --no-upgrade wrapt  # works, does not upgrade (expected)
$ uv pip install --no-upgrade --no-upgrade wrapt  # fails, disallowed
$ uv pip install --upgrade --no-upgrade --upgrade wrapt  # works, upgrades (expected)
$ uv pip install --upgrade --upgrade wrapt  # fails, disallowed
$ uv pip install --upgrade --no-upgrade --upgrade --no-upgrade --upgrade --no-upgrade --upgrade --no-upgrade wrapt  # works, does not upgrade (expected)
$ uv pip install --upgrade --no-upgrade --upgrade --no-upgrade --upgrade --no-upgrade --upgrade --no-upgrade --upgrade wrapt  # works, upgrades (expected)
```

---
