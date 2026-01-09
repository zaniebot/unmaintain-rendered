---
number: 3410
title: "Add a `include` parameter to allow expanding scope of included files"
type: issue
state: closed
author: neingeist
labels:
  - configuration
  - cli
assignees: []
created_at: 2023-03-08T21:52:09Z
updated_at: 2023-08-24T00:10:30Z
url: https://github.com/astral-sh/ruff/issues/3410
synced_at: 2026-01-07T13:12:14-06:00
---

# Add a `include` parameter to allow expanding scope of included files

---

_Issue opened by @neingeist on 2023-03-08 21:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff 0.0.254 seems to ignore Python scripts that do not end in `.py`. Is there an option to include them in the configuration?


---

_Comment by @charliermarsh on 2023-03-08 22:07_

We typically require that those files are passed-in explicitly -- e.g., if you do `ruff /path/to/file`, we'll _always_ check `file` regardless of the extension; but if you do `ruff path/to`, we only check `.py` and `.pyi` files.

I think this is consistent with Black's behavior (running `black foo` won't reformat files in `foo` that _don't_ end in `.py`).


---

_Label `question` added by @charliermarsh on 2023-03-08 22:07_

---

_Comment by @neingeist on 2023-03-08 22:35_

I see! I absolutely agree that the default behavior is good. However, Black can be configured to include these extra files:

```
[tool.black]
include = '.*/(bad-movies|test|.*\.py)$'
```
Something like that I would wish for :) (Black will check `bad-movies` and `test`, too, with this configuration.)

---

_Comment by @charliermarsh on 2023-03-08 22:42_

Oh interesting, so that's the file list that's used if you execute `black` (instead of `black /path/to/folder`)?

---

_Comment by @neingeist on 2023-03-08 22:42_

Ruff is a great tool, by the way! Need to remind myself to also give positive feedback, not only report bugs/wish for features 😇

---

_Comment by @charliermarsh on 2023-03-08 22:42_

Or does `include` also apply if you do `black /path/to/folder`?

---

_Comment by @charliermarsh on 2023-03-08 22:42_

Oh heh thank you!

---

_Comment by @charliermarsh on 2023-03-08 22:45_

I can also just look at the Black docs to answer those questions. I agree it could make sense to support something like that.

---

_Comment by @neingeist on 2023-03-08 22:46_

`black` without an option doesn't do anything, so it's like `ruff`. This `include` is used when I run `black .`  (or `black /path/to/src/`).  It seems to search the path and match against the regex in `include`.

---

_Comment by @charliermarsh on 2023-03-08 22:48_

Ahhh I see, ok. Lemme think on that. (So our `include` is roughly `*.pyi?` right now.)

---

_Comment by @neingeist on 2023-03-08 22:54_

Black's default seems to be `include = '\.pyi?$'`. While I checked Black's behavior I noticed that it still ignores files in `.gitignore`.

---

_Renamed from "Option to include files?" to "Add a `include` parameter to allow expanding scope of included files" by @charliermarsh on 2023-03-08 22:54_

---

_Label `question` removed by @charliermarsh on 2023-03-08 22:54_

---

_Label `configuration` added by @charliermarsh on 2023-03-08 22:54_

---

_Label `cli` added by @charliermarsh on 2023-03-08 22:54_

---

_Comment by @charliermarsh on 2023-03-08 22:55_

(We also ignore files in `.gitignore`.)

---

_Referenced in [astral-sh/ruff#2192](../../astral-sh/ruff/issues/2192.md) on 2023-03-08 22:55_

---

_Comment by @neingeist on 2023-03-09 00:42_

Ah, I just noticed your example was a glob pattern, mine was a regex (because Black uses a regex here). Probably something to think about a bit.

---

_Comment by @ahal on 2023-03-10 21:12_

This feature would be appreciated! We have some custom file extensions (which are a subset of Python) in a large monorepo. In order to pass them into `ruff` explicitly, we have a Python wrapper to find them all.. But finding the files takes much longer than it takes `ruff` to lint the entire monorepo :p.

---

_Comment by @charliermarsh on 2023-03-10 21:16_

😅 😅 😅  Will try to get to this soon.



---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-11 21:11_

---

_Comment by @neingeist on 2023-03-15 17:22_

> (We also ignore files in `.gitignore`.)

Yeah, I just wanted to mention it, because Black's behavior seems to be:

- search the path
- process files matching `include` regex (with a sensible default)
- BUT not ignored files (e.g. `.gitignore`)

---

_Comment by @steve-mavens on 2023-03-25 13:59_

In view of https://github.com/charliermarsh/ruff/issues/2192, would it be useful for this feature to distinguish between, "I want you to check all files matching this pattern" and "I'm throwing the kitchen sink at you here, and I only want you to check all files matching this pattern which also have a #! python"?

Then (assuming a regex-y syntax) someone might say:
```
include = .*\.pyi?
include-scripts = .*
```

Which means, "check all .py files, all .pyi files, and all files that contain shebangs"

Or:
```
include = *.pyi?
include-scripts = ^[^\.]*$
```

which means, "check all .py files, all .pyi files, and all files with no file extension that contain shebangs".


---

_Referenced in [astral-sh/ruff#3783](../../astral-sh/ruff/issues/3783.md) on 2023-03-28 20:28_

---

_Referenced in [astral-sh/ruff#3914](../../astral-sh/ruff/pulls/3914.md) on 2023-04-08 04:14_

---

_Closed by @charliermarsh on 2023-04-12 03:39_

---

_Comment by @charliermarsh on 2023-04-12 03:45_

The first version of this will go out in the next release -- e.g., you can do:

```toml
[tool.ruff]
extend-include = ["*.pyw"]
```

...to include `.pyw` files alongside the default `.py` and `.pyi` files.

---

_Comment by @tylerlaprade on 2023-08-23 22:47_

Oh, this is incredible!

By the way, my usecase is that I only want to run Ruff on manual Django migrations, not auto-generated ones. We have a convention that they are always named `####_manual_foo`, so I _tried_ to match with `extend-exclude = ["src/migrations/[0-9][0-9][0-9][0-9]_[!m][!a][!n][!u][!a][!l]*", "typings"]`, but today we had a confusing hit on `0212_contract_budget....py`. Then we realized that the 3rd letter of both is `n`, so this pattern is incorrect. What I really want is to exclude everything in `migrations/`, but then to include anything starting with `manual`, which I'm hoping `extend-include` will allow me to do.

EDIT: Hmm, it doesn't seem to work the way I expected

EDIT2: I made it work with just `extend-exclude`. ``"src/migrations/[0-9][0-9][0-9][0-9]_[!{manual}]*"`` does exactly what I want!

---

_Comment by @charliermarsh on 2023-08-23 23:09_

Nice! I think it probably didn't work as you'd expected because we do exclusions first, then inclusions, similar to Black (https://github.com/astral-sh/ruff/pull/3914#issuecomment-1504528725).

---

_Comment by @tylerlaprade on 2023-08-23 23:14_

Update: It... sometimes works? I can't actually figure out what the logic is. `0118_contract_budget...` is correctly excluded, but a bunch of random `####_alter_...` are included, and I'm not sure why.

EDIT: Okay, I figured it out. `contract` starts with `c`, which is not in `"manual"`. `alter` starts with `a`, which is. Based on the [globset docs](https://docs.rs/globset/latest/globset/#syntax), I assumed I could use curlies to define a full string as a pattern instead of just a single character, but apparently it's still matching individual characters from inside the curlies.

![image](https://github.com/astral-sh/ruff/assets/5475199/efe4d517-3248-41e3-85ba-4929e91772fc)


---

_Comment by @tylerlaprade on 2023-08-23 23:17_

>exclusions first, then inclusions

Shouldn't that actually make it behave the way I want? I want to first exclude the entire directory, then selectively include a few files from that directory.

---

_Comment by @charliermarsh on 2023-08-24 00:10_

@tylerlaprade - The way to think of it is: first, we iterate over all directories and files, removing any that match `exclude`; then, we pass the remaining files through `include`, and lint anything that matches. So if something is removed by an exclude, it can't be "re-added" by an `include`.

---
