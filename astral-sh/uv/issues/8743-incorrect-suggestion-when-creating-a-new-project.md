```yaml
number: 8743
title: Incorrect suggestion when creating a new project with --backend instead of --build-backend
type: issue
state: closed
author: issokuos
labels:
  - cli
  - external
assignees: []
created_at: 2024-11-01T06:34:03Z
updated_at: 2025-01-29T22:15:06Z
url: https://github.com/astral-sh/uv/issues/8743
synced_at: 2026-01-12T15:59:33Z
```

# Incorrect suggestion when creating a new project with --backend instead of --build-backend

---

_@issokuos_

`uv` suggests the wrong argument when creating a new project with a wrong `--backend` argument

```bash
uv init  --lib --backend maturin no-name
```

It returns the following error message:

```bash
error: unexpected argument '--backend' found

  tip: a similar argument exists: '--package'

Usage: uv init <PATH|--name <NAME>|--virtual|--package|--no-package|--app|--lib|--script|--vcs <VCS>|--build-backend <BUILD_BACKEND>|--no-readme|--author-from <AUTHOR_FROM>|--no-pin-python|--no-workspace|--python <PYTHON>>

For more information, try '--help'.
```

I was expecting the suggestion to  be _a similar argument exists: `--build-backend`_ 


Version:

```bash
uv --version
uv 0.4.25 (97eb6ab4a 2024-10-21)
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Label `cli` added by @charliermarsh on 2024-11-01 13:32_

---

_Comment by @charliermarsh on 2024-11-01 13:32_

That's an odd one though I don't think we control these suggestions.

---

_Label `upstream` added by @zanieb on 2024-11-01 13:41_

---

_Comment by @zanieb on 2024-11-01 13:41_

Yeah these are implemented in Clap, not sure what we can do here.

---

_Comment by @zanieb on 2024-11-01 13:43_

I guess we can declare a hidden `--backend` argument with [`UnknownArgumentValueParser`](https://docs.rs/clap/latest/clap/builder/struct.UnknownArgumentValueParser.html) and suggest the proper one. There are some minor caveats, as discussed in https://github.com/clap-rs/clap/discussions/5781 (though that mostly focused on subcommands).

---

_Comment by @issokuos on 2025-01-24 21:03_

I can try to implement this if that is ok

---

_Comment by @zanieb on 2025-01-24 21:15_

@styvane sure!

---

_Closed by @zanieb on 2025-01-29 22:15_

---

_Closed by @zanieb on 2025-01-29 22:15_

---
