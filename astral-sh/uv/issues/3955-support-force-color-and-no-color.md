---
number: 3955
title: "Support `FORCE_COLOR` and `NO_COLOR`"
type: issue
state: closed
author: hugovk
labels:
  - enhancement
  - help wanted
  - configuration
assignees: []
created_at: 2024-06-01T09:42:17Z
updated_at: 2024-06-04T21:00:44Z
url: https://github.com/astral-sh/uv/issues/3955
synced_at: 2026-01-10T01:23:32Z
---

# Support `FORCE_COLOR` and `NO_COLOR`

---

_Issue opened by @hugovk on 2024-06-01 09:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


https://force-color.org and https://no-color.org are "community standards" supported by many tools, such as ruff, pip, pytest, rich, nox, tox.

It would be nice to set the `FORCE_COLOR: 1` env var in GitHub Actions so I can have coloured output in the CI. It makes logs more readable, especially when there's a failure. 

uv does have a `--color` CLI option, but more and more tools are using uv behind the scenes (such as https://github.com/hynek/build-and-inspect-python-package) so I don't have direct access to the uv invocation.

---

_Comment by @charliermarsh on 2024-06-01 12:24_

Yeah good call -- I think we might respect one of these... `NO_COLOR`, at least? Via https://docs.rs/anstream/latest/anstream/struct.AutoStream.html. I'll look into it.

---

_Comment by @charliermarsh on 2024-06-01 12:26_

Yeah we do support `NO_COLOR`, but not `FORCE_COLOR=1`.

---

_Label `enhancement` added by @charliermarsh on 2024-06-01 12:26_

---

_Label `help wanted` added by @charliermarsh on 2024-06-01 12:26_

---

_Label `configuration` added by @charliermarsh on 2024-06-01 12:26_

---

_Comment by @samypr100 on 2024-06-01 23:45_

It does seem like anstream already supports `CLICOLOR_FORCE` which seems to do the same (in premise) as `FORCE_COLOR`, so maybe a doc update?

---

_Comment by @charliermarsh on 2024-06-01 23:47_

We might need to validate the precedence here too with the command-line arguments.

---

_Comment by @hugovk on 2024-06-02 09:42_

I already have `FORCE_COLOR` on the CI for other tools. I'd prefer if uv supported `FORCE_COLOR`, so I don't need to add an extra env var just for uv.

I forgot to mention it earlier, CPython 3.13 also supports `FORCE_COLOR`:

* https://docs.python.org/3.13/using/cmdline.html#controlling-color

---

_Referenced in [rust-cli/anstyle#192](../../rust-cli/anstyle/issues/192.md) on 2024-06-02 16:49_

---

_Comment by @charliermarsh on 2024-06-03 01:50_

Yeah we should support it -- we also support it in Ruff for this reason.

---

_Comment by @charliermarsh on 2024-06-03 01:58_

Thanks for looking into it in anstream @samypr100, you rock.

---

_Referenced in [astral-sh/uv#3979](../../astral-sh/uv/pulls/3979.md) on 2024-06-03 02:49_

---

_Comment by @epage on 2024-06-03 18:45_

> Should we add CLICOLOR_FORCE as well so that it's respected over the color CLI argument?

FYI `anstream` respects `NO_COLOR` and `CLICOLOR` / `CLICOLOR_FORCE`.  Until recently, `FORCE_COLOR` has been very ad-hoc with people copying patterns of it and extending it in bespoke ways without any coordination.  Looks like someone is now trying to coordinate that effort though I think I want to be cautious and see how well their variant takes off vs any other.  I'm also concerned that both `CLICOLOR` and `FORCE_COLOR` exist without specifying their interaction.

*see also my comments on rust-cli/anstyle#192*

---

_Comment by @charliermarsh on 2024-06-03 18:49_

Thanks @epage. For some reason `FORCE_COLOR` has become very popular in the Python ecosystem -- you can see all the listed projects here: https://github.com/astral-sh/ruff/issues/5499#issue-1787106134

---

_Comment by @epage on 2024-06-03 19:18_

That raises even more questions for me as none of them implement https://force-color.org/ (though most are likely "close enough").

- Non-empty `FORCE_COLOR`
  - https://force-color.org/
- Checks for presence of `FORCE_COLOR`
  - https://github.com/Textualize/rich/blob/master/rich/console.py#L952-L956
  - https://github.com/wntrblm/nox/blob/main/nox/_options.py#L605
  - https://github.com/pytest-dev/pytest/blob/97ed533f63d5780a05702a711555cb6744247a37/src/_pytest/_io/terminalwriter.py#L26-L37
  - https://github.com/pypa/build/blob/main/src/build/__main__.py#L46-L53
- treats it as a bool-ish
  - https://github.com/tox-dev/tox/blob/main/src/tox/config/cli/parser.py#L352-L361
  - *(rust)* https://github.com/zkat/supports-color/blob/d5c906d1fcd91cd1bf92d22f3d0cca452de8ca9f/src/lib.rs#L38-L43 (I believe this was modeled off of a JS package)
    - Note that higher numbers express certain capabilities of the terminal
- treats it as an integer-bool
  - https://github.com/python/mypy/blob/master/mypy/util.py#L552-L557

So what approach is "right"?  https://force-color.org/ is just a website that some person put up within the last 9 months.  That doesn't make it definitive.

---

_Referenced in [donatj/force-color.org#30](../../donatj/force-color.org/issues/30.md) on 2024-06-04 02:17_

---

_Comment by @epage on 2024-06-04 02:18_

I've raised the discrepancies in donatj/force-color.org#30

---

_Closed by @charliermarsh on 2024-06-04 21:00_

---

_Referenced in [astral-sh/uv#7352](../../astral-sh/uv/issues/7352.md) on 2024-09-13 08:21_

---

_Referenced in [crate-ci/typos#1141](../../crate-ci/typos/issues/1141.md) on 2024-11-04 12:51_

---

_Referenced in [Textualize/rich#2924](../../Textualize/rich/issues/2924.md) on 2025-03-22 20:47_

---
