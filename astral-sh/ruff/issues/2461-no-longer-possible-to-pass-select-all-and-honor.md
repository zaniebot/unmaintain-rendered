---
number: 2461
title: "No longer possible to pass --select=ALL and honor pyproject.toml's tool.ruff.ignore"
type: issue
state: closed
author: ppentchev
labels: []
assignees: []
created_at: 2023-02-02T01:20:28Z
updated_at: 2023-02-02T13:45:08Z
url: https://github.com/astral-sh/ruff/issues/2461
synced_at: 2026-01-10T01:22:40Z
---

# No longer possible to pass --select=ALL and honor pyproject.toml's tool.ruff.ignore

---

_Issue opened by @ppentchev on 2023-02-02 01:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->
Hi,

First of all, thanks A LOT for writing and developing ruff!

Now, I'm not sure that this is an issue per se, since I know it can be worked around by e.g. trying to keep up with new rules, but, as noted in other issues, this is somewhat difficult, mostly because of the impressive rate of ruff development (and thanks for that!).

So, in short: up until ruff-0.0.237, I had these two Tox environments defined:

```
[defs]
pyfiles =
  src/sameconf
  unit_tests

[testenv:ruff]
skip_install = True
tags =
  check
deps =
  ruff >= 0.0.237, < 1
commands =
  ruff -- {[defs]pyfiles}

[testenv:ruff-all]
skip_install = True
tags =
  check
deps =
  ruff == 0.0.237
commands =
  ruff --select=ALL -- {[defs]pyfiles}
```
...and I use a semi-automated Tox test runner (very soon to be released publicly) to run Tox tests in groups: first all the ruff environments in parallel so that they can catch trivial mistakes very quickly (and once again, thanks for that!), then all the other static checkers (flake8, mypy, pylint, pydocstyle, a couple of others that I learned about from ruff) in parallel, so that they can catch mistakes before I run a potentially very time-consuming unit tests suite, and then, as the last step, unit and functional test environments, again in parallel. But I digress.

(side note: note that the above tox.ini syntax only works with Tox 3.x; I have not yet figured out a way to define a multiline list in Tox 4.x and then use it in a command without the newlines breaking the command into several lines for the shell with unfortunate results)

So the thing is, I had figured out a way to keep abreast of ruff development: run the default checks for the newest version I got from PyPI each time the environment was recreated, and every now and then, when I actually noticed that a new version was installed, bump the version in the ruff-all environment, run with all the checks (NB: except for the ones listed in tool.ruff.ignore with comments explaining why each one is ignored), and then, if it should fail because of new checks added, figure out whether I should ignore them for this particular project.

...and you can probably see where this is going: with ruff-0.0.238, `--select=ALL` on the command line overrides the tool.ruff.ignore list in pyproject.toml, so now the ruff-all environment gives me all kinds of (sometimes conflicting) advice about things I'd already specifically, consciously told it to ignore for this particular project. And yes, I can get around that by duplicating the ignore list on the ruff-all environment command line (adding `--ignore=ANN101,COM812,D203,D213,...`), but that feels somehow suboptimal.

So, sure, I understand that the preferred mode of using ruff is to list all the checks that I want and try to keep up with ruff's development if I want to make use of the new check areas, too. Please feel free to close this issue as "works as intended, your way only worked by accident, please use the supported one" if you feel that it is not worth it to spend your time and effort figuring out a way to put up with my whims :) Still, I thought I'd ask, just in case there is a simple solution that I'm missing (and I'd be the first to admit that there probably is one, I miss things very often :)

Thanks for reading this far, thanks in advance for your time, and keep up the great work!

G'luck,
Peter


---

_Comment by @ppentchev on 2023-02-02 01:29_

So I just thought of a possible way to fix this: have a separate .ruff-all.toml or ruff-all/.ruff.toml or something config file that specifies select="ALL" and then ignore=[...]. At first blush, this seems like a workable workaround, although I still have the minor issue of having to duplicate the non-select-ignore settings, e.g. line-length, target-version, anything else that any other checks may need. Still, it seems that it might work... so if you decide to say "so go with it, go away, let me concentrate on the real development issues", that's fine, and thanks :)

---

_Comment by @charliermarsh on 2023-02-02 01:40_

Thanks for writing this up. I appreciate it! I have one thought on a solution that might work, but let me try it first...


---

_Comment by @charliermarsh on 2023-02-02 01:46_

So, this works, but I can think of a few ways to improve it.

Create `./ruff.toml` with your current settings, like:

```toml
select = ["E", "F"]
ignore = ["F401"]
line-length = 10
```

Then create `./ruff-all/ruff.toml` (unfortunately it _has_ to be called `ruff.toml`, but I'm going to fix this), like this:

```toml
# Inherit all settings from the existing `ruff.toml`.
extend = "../ruff.toml"

# Select everything...
select = ["ALL"]

# Repeat the ignores (unfortunately)...
ignore = ["F401"]
```

Then run `ruff --config=./ruff-all/ruff.toml /path/to/file.py`, and `line-length` and other settings will be preserved.

Unfortunately you do need to specify the `ignore` again in the extended configuration. I thought that would be unnecessary, I'll look into it.


---

_Comment by @charliermarsh on 2023-02-02 01:49_

I'll also \cc @not-my-profile who helped drive this recent change, in case they have input on the best way to do this.


---

_Referenced in [astral-sh/ruff#2467](../../astral-sh/ruff/pulls/2467.md) on 2023-02-02 04:33_

---

_Comment by @not-my-profile on 2023-02-02 04:47_

Yes this is all working as intended. The previous config handling was very counter-intuitive since you often couldn't override `.toml` settings via the CLI ... or even later `.toml` config files couldn't override the settings of earlier `.toml` files.

>     # Inherit all settings from the existing `ruff.toml`.
>     extend = "../ruff.toml"

`extend` does not simply inherit all settings ... it also didn't do that before #2312 ... the `select` and `ignore` settings have been and still are treated specially.

Before the most specific setting won no matter where it was defined ... since #2312 a ruleset is computed after every config file and `ignore` settings always only affect previously selected rules.

While we could introduce an `extended-by` option to specify the next config layer (since `extend` can only specify the previous config layer), I think introducing such an option only for the ignore situation seems like a bad idea ... especially since it would significantly complicate our config file resolution.

I have implemented a simple solution in #2467 by special casing `select = []` causing it to carry over ignores to the next config layer. That should of course be documented ... while that's a bit of magical behavior I think it's still very much preferable to the previous config handling.

So with this you could have a `base.toml` specifying:

```toml
select = []
ignore = ["ANN101", "COM812", "D203", "D213"]
```

And then either use it with e.g. `--config base.toml --select ALL` or  `--config base.toml --select E,F` or alternatively have `all.toml` and `some.toml` both specifying different `select`s and `extend = "base.toml"`.

Note the difference to:

```toml
ignore = ["ANN101", "COM812", "D203", "D213"]
```

which is different since it substracts the ignored rules from the default `select` set (`["E", "F"]`).


---

_Comment by @ppentchev on 2023-02-02 07:14_

Thanks to you both! @charliermarsh's `extend = "path"` worked like a charm for the present. Now my pyproject.toml file has a simple section:
```
# See .config/ruff-all/pyproject.toml for the strict version
# https://github.com/charliermarsh/ruff/issues/2461
[tool.ruff]
target-version = "py38"
line-length = 100
```
...and .config/ruff-all/pyproject.toml has this:
```
[tool.ruff]
extend = "../../pyproject.toml"
select = ["ALL"]
ignore = [
  # We know what self is, we hope...
  "ANN101",

  # We leave most of the formatting to the `black` tool.
  "COM812",
...
```

I will test #2467 in a couple of hours; it looks like it may simplify things structurally even further.

Thanks again to both of you for the lightning-fast responses!

G'luck,
Peter


---

_Closed by @charliermarsh on 2023-02-02 13:45_

---
