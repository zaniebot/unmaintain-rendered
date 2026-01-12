```yaml
number: 3694
title: Make calling ruff programmatically slightly faster
type: issue
state: open
author: samuelcolvin
labels:
  - core
  - needs-decision
assignees: []
created_at: 2023-03-23T20:22:24Z
updated_at: 2024-02-23T21:44:47Z
url: https://github.com/astral-sh/ruff/issues/3694
synced_at: 2026-01-12T15:54:44Z
```

# Make calling ruff programmatically slightly faster

---

_@samuelcolvin_

(this is low priority, but maybe interesting)

I using ruff in [pytest-examples](https://github.com/pydantic/pytest-examples) to lint and fix code examples.

Currently I have to write a `ruff.toml` config file, then write the python file, then call ruff by `subprocess`.

I guess if ruff had someway to set all config via cli arguments, then receive the python code via stdin, I could avoid needing to write to files.


---

_Comment by @charliermarsh on 2023-03-23 21:28_

> ...then receive the python code via stdin.

If helpful in the interim: Ruff _can_ take code via stdin:

```
ruff on main [$!?] is 📦 v0.0.259 via 🐍 v3.11.0 (.venv) via 🦀 v1.67.1
❯ cat foo.py
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: foo.py
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ def f():
   2   │     x = 1
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

ruff on main [$!?] is 📦 v0.0.259 via 🐍 v3.11.0 (.venv) via 🦀 v1.67.1
❯ cat foo.py | ruff - --stdin-filename=foo.py
foo.py:2:5: F841 [*] Local variable `x` is assigned to but never used
Found 1 error.
[*] 1 potentially fixable with the --fix option.

ruff on main [$!?] is 📦 v0.0.259 via 🐍 v3.11.0 (.venv) via 🦀 v1.67.1
❯ cat foo.py | ruff - --stdin-filename=foo.py --fix
def f():
    pass
```

---

_Label `core` added by @charliermarsh on 2023-03-23 21:29_

---

_Label `wishlist` added by @charliermarsh on 2023-03-23 21:29_

---

_Label `wishlist` removed by @charliermarsh on 2023-03-23 21:29_

---

_Comment by @charliermarsh on 2023-03-23 21:29_

(But there's no way to provide configuration programmatically. You could write to a tempfile and pass it in as `--config /path/to/file.toml` if easier, but nothing better than that sadly.)

---

_Comment by @samuelcolvin on 2023-03-23 21:50_

Thanks for the pointer on `--stdin-filename`.

> (But there's no way to provide configuration programmatically. You could write to a tempfile and pass it in as `--config /path/to/file.toml` if easier, but nothing better than that sadly.)

yup, that's what I'm doing now.

BTW, it's slightly surprising that `line-length` can be set via the command line.

---

_Comment by @charliermarsh on 2023-03-23 21:54_

Haha it's hidden! We deprecated it but it's still possible :D

---

_Comment by @samuelcolvin on 2023-03-23 22:40_

ha, thanks for the pointer about stdin, that's working well (seems with stdin, you have to manually define the config file).

It would amazing if config could be set via an (albeit) very ugly command line flag, perhaps base64 encoded to avoid newline weirdnesses.

My solution still involves writing the config on every test which is slowing things down.

---

_Comment by @DanCardin on 2023-03-24 14:09_

Idk if this sounds crazy or not, but if ruff were to support file-level configuration comments akin to the noqa ones (but also for selection) i.e. `# ruff: ignore: N, E5` / `# ruff: select: N, E5`, then for this particular usecase you could(?) pipe some comment configuration ahead of the code you're piping in.

With the extra benefit that the new behavior would be cross functional for things outside this usecase.

---

_Comment by @MichaReiser on 2023-03-24 14:17_

I would refrain from adding new features to ruff only to make programmatically calling ruff via the CLI slightly faster. The proper solution in my view is to add a python API to lint a file given its content as a string and the settings 

---

_Comment by @DanCardin on 2023-03-24 14:21_

I would think file-level configuration comments (which enable both selecting and ignoring) would be a generally useful feature. given that you can already `noqa` specific violation types.

...and it just so happens that it would enable you to avoid writing files (even if it's somewhat of a hack for you). Although i absolutely agree, an actual programmatic API would make more sense trying to address this specific problem.

---

_Comment by @MichaReiser on 2023-03-24 15:09_

> I would think file-level configuration comments (which enable both selecting and ignoring) would be a generally useful feature. given that you can already noqa specific violation types.

I'm sorry. My intention wasn't to say that this feature isn't useful or that I consider it a hack. The only thing I wanted to convey is that I don't recommend any new features with no practical use other than speeding up ruff's programmatic CLI invocation.  

---

_Comment by @samuelcolvin on 2023-03-24 20:48_

> I would refrain from adding new features to ruff only to make programmatically calling ruff via the CLI slightly faster. The proper solution in my view is to add a python API to lint a file given its content as a string and the settings

That's exactly what I'm trying to do, but since Ruff doesn't build with pyo3 whatsoever, adding a formal python interface is a massive change.

Hence allowing full functionality via subprocess without touching the filesystem might be the best solution IMHO.

Also @charliermarsh, I've run into a problem using stdin - when using `--fix` I get back the reformatted content which is great, but there's no way to get error messages in this case.

Surely the formatted python is going to `stdout`, error messages should be piped to `stderr`?

---

_Comment by @charliermarsh on 2023-03-24 21:06_

Hmm yeah, perhaps they should be included as `stderr` -- it's just a bit strange because there's no other context in which we write diagnostics to `stderr` (then again, there's no other context in which we write "fixed" content to `stdout`).

If you're looking to parse the diagnostics, you could also consider using `--format=json`?


---

_Comment by @samuelcolvin on 2023-03-24 22:33_

Currently I'm just using a regex to fix the file name and line number, when printing.

Would be great if stderr could get the diagnostics in this case.

---

_Comment by @samuelcolvin on 2023-03-26 15:29_

> I've run into a problem using stdin - when using `--fix` I get back the reformatted content which is great, but there's no way to get error messages in this case.

For anyone else running into this problem, my fix is to run ruff again without `--fix` and use the error from that run, see https://github.com/pydantic/pytest-examples/pull/5.

---

_Comment by @MichaReiser on 2023-07-10 08:30_

See #5102 for another use case where an option to pass the configuration (they propose to pass the config as JSON string) would enable more advanced workflows.

---

_Label `needs-decision` added by @MichaReiser on 2023-07-10 08:30_

---

_Comment by @MichaReiser on 2023-07-17 21:02_

It probably doesn't satisfy all your needs and we don't make any stability guarantees but an alternative is to run ruff's wasm version. I don't know how fast https://github.com/wasmerio/wasmer-python is... could as well be slower than spawning a new process 

---

_Comment by @Finkregh on 2023-09-12 09:55_

This would also help when providing pre-commit hooks to others. I'd like to allow others to use prepared hooks to get a consistant linting/formatting without having to add additional config files to their repository.
Currently this excludes setting e.g. `pydocstyle` as pre-commit only allows setting arguments, not copying files from other repositories.

This also applies to CI jobs, but there you could just copy a config file from somewhere else, of course.

(Thanks for this great tool!)

---

_Comment by @samuelcolvin on 2023-12-21 13:44_

@charliermarsh any progress on this?

This problem get's worse now that we have `ruff format` which has to be run separately from `ruff check --fix` - instead of invoking one process for each snippet, we now have to invoke two to use just ruff.

1. It would be awesome if there was a way to run code through ruff without having to create a new process each time - for me this doesn't need to be a python library, just a way to stream multiple files to ruff, then later stream another etc.
2. (and I assume this is already on your roadmap) it would be nice if there was a way to get all the stuff done by `check --fix` in the same process as `format`.

:pray: thank you.

---

_Comment by @zanieb on 2023-12-21 14:35_

Hey Samuel!

1. We're interested in porting `ruff-lsp` to Rust which would give you a persistent process you could call (https://github.com/astral-sh/ruff-lsp/issues/300), I wonder if that would work for you.
2. We are planning a single command; see #8232

---

_Comment by @charliermarsh on 2023-12-21 14:42_

I don’t know that the LSP rewrite will help here — would need to look deeper… Since you’ll still have subprocess overhead when calling from Python.

---

_Comment by @samuelcolvin on 2023-12-21 14:51_

I think ruff-lsp might work, creating one subprocess is absolutely fine if we can use it for all tests.

The problem in our case is creating one process (two now we're using `ruff format` not black for formatting) for every code example in markdown and docstrings while running tersts.

Timing examples:
* running `pdm run pytest tests/test_docs.py` runs 478 tests in 7.93s (16ms/test) - that's basically running, linting and formatting for ever code snippet in the code base using `pytest-examples`, I think much of that time is spent spinning up the (currently) one ruff process per case
* for comparison, running `pdm run pytest tests/test_types.py` runs 754 tersts in 1.42s (1.9ms/test)

Those timings roughly match my intuition that launching a rust process takes ~10ms.

---

_Comment by @zanieb on 2023-12-21 15:03_

We do support formatting of docstring code snippets now :) perhaps what you really want is linter support for code snippets without extracting them yourself.

---

_Comment by @samuelcolvin on 2023-12-21 15:08_

ye, quite possibly.

A few questions:
* can this work in markdown files too?
* do you provide a way for magic markdown classes to disable linting, like [this](https://github.com/pydantic/pydantic/blob/19fa86cbfcc7b64c5d59c04c38fa80b497f275a2/docs/concepts/alias.md?plain=1#L27)
* (I guess this is possible by disabling some linting rule), we use `#>` to identify print output which is automatically inserted into snippets

Sorry to derail this discussions, just interested in whether we could get rid of the linting inside pytest-examples, it would make it much simpler.

cc @davidhewitt

---

_Comment by @MichaReiser on 2024-02-23 21:43_

The latest release provides a way to [set arbitrary configuration options via the CLI](https://github.com/astral-sh/ruff/pull/9599), and you can pass the file content on via stdin. Could you let me know if this is enough for your use case?

Also, @snowsignal is working on our LSP rewrite in rust. Stay tuned.

---
