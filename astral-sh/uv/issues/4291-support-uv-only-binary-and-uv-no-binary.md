---
number: 4291
title: "Support `UV_ONLY_BINARY` and `UV_NO_BINARY` environment variables"
type: issue
state: open
author: njzjz
labels:
  - help wanted
  - configuration
assignees: []
created_at: 2024-06-12T22:09:38Z
updated_at: 2025-03-05T00:20:19Z
url: https://github.com/astral-sh/uv/issues/4291
synced_at: 2026-01-10T01:23:36Z
---

# Support `UV_ONLY_BINARY` and `UV_NO_BINARY` environment variables

---

_Issue opened by @njzjz on 2024-06-12 22:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This is useful with cibuildwheel or some other complicated tools in the CI environment.

pip has similiar `PIP_NO_BINARY` and `PIP_ONLY_BINARY`: https://pip.pypa.io/en/latest/cli/pip_install/#cmdoption-no-binary

---

_Referenced in [astral-sh/uv#1794](../../astral-sh/uv/issues/1794.md) on 2024-06-12 22:10_

---

_Comment by @zanieb on 2024-06-13 00:50_

I think we've avoided this because we're not interested in supporting the pip syntax outside of the pip interface. Maybe we could namespace them e.g. `UV_PIP_NO_BINARY` in the meantime?

---

_Label `configuration` added by @zanieb on 2024-06-13 00:50_

---

_Comment by @henryiii on 2024-08-13 03:07_

Some of them are available, though, like `UV_CONSTRAINT`, `UV_INDEX_URL`, `UV_EXTRA_INDEX_URL`, and `UV_BREAK_SYSTEM_PACKAGES`. I don't really care what it's called, but it would be nice to have, as you can't pass flags through inside cibuildwheel to every command.

---

_Referenced in [astral-sh/uv#6428](../../astral-sh/uv/issues/6428.md) on 2024-08-23 01:00_

---

_Comment by @sanmai-NL on 2024-09-24 09:22_

@zanieb Please consider that other package managers also support this, since it's a genuinely useful feature for containerized builds, and not just a Pip feature.

- [PDM](https://pdm-project.org/latest/usage/lockfile/#set-acceptable-format-for-locking-or-installing)
- [Poetry](https://python-poetry.org/docs/configuration/#installerno-binary) (no env var, but functionally somewhat equivalent persistent config).

---

_Comment by @zanieb on 2024-09-24 13:02_

@sanmai-NL we do support this via persistent configuration, e.g.,

- https://docs.astral.sh/uv/reference/settings/#no-binary
- https://docs.astral.sh/uv/reference/settings/#no-binary-package
- https://docs.astral.sh/uv/reference/settings/#no-build
- https://docs.astral.sh/uv/reference/settings/#no-build-package

Since our top-level settings differ from pip (we don't use `:all:`, we have a dedicated flag instead), I'm not sure how best to introduce environment variables for this.

I guess:

```
UV_NO_BINARY=1
UV_NO_BINARY_PACKAGE="foo bar"
```

And `uv pip` users would need to use the new syntax too?

---

_Comment by @lengau on 2025-02-06 15:03_

Yeah, having environment variables available for this would be very useful for us. We have two different build environments we use, and neither of them make it particularly easy to add the `--no-binary` argument in the cases where we need it, but both make it really easy to add environment variables. I like @zanieb 's most recent suggestion for how to do it.

---

_Comment by @zanieb on 2025-02-06 16:59_

I'm happy to review a contribution adding the variables described.

---

_Label `help wanted` added by @zanieb on 2025-02-06 16:59_

---

_Referenced in [astral-sh/uv#11399](../../astral-sh/uv/pulls/11399.md) on 2025-02-10 19:55_

---

_Comment by @lengau on 2025-02-10 20:08_

I've added a version that handles build commands at https://github.com/astral-sh/uv/pull/11399.

Unfortunately, when doing so I noticed that the build commands and `uv pip` work somewhat differently, so I wanted to run my potential implementation up the flagpole before doing it.

The problem is that `uv pip`, like `pip`, has order-dependent instances of `--no-binary` and similar, and it does accept the special values of `:all:` and `:none`. My proposal therefore is to use a similar behaviour to how `pip` itself handles the environment variables, but to also handle the way `uv pip` handles them.

1. If `UV_NO_BINARY` is true, `uv pip` commands treat that as equivalent to `uv pip <cmd> --no-binary=:all: [...]`.
2. If `UV_NO_BINARY_PACKAGE` is set, `uv pip` commands treat that as equivalent to `uv pip <cmd> --no-binary=<package> [repeated for each unpacked package name] [...]`

This is a special case only needed for pip commands because of how they work.



---

_Comment by @zanieb on 2025-02-10 21:06_

Yeah we'd reuse the top-level uv semantics in `uv pip`, that sounds correct.

---

_Referenced in [astral-sh/uv#11963](../../astral-sh/uv/issues/11963.md) on 2025-03-05 00:16_

---

_Comment by @zanieb on 2025-03-05 00:19_

Done in #11399 

---

_Closed by @zanieb on 2025-03-05 00:19_

---

_Comment by @zanieb on 2025-03-05 00:20_

Oops, not done — not handled in `uv pip` yet.

---

_Reopened by @zanieb on 2025-03-05 00:20_

---
