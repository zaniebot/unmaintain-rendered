```yaml
number: 1838
title: "Do not include `unsafe` dependencies in the compiled file unless some flag specified"
type: issue
state: closed
author: drjackild
labels: []
assignees: []
created_at: 2024-02-21T22:17:32Z
updated_at: 2024-02-21T22:42:10Z
url: https://github.com/astral-sh/uv/issues/1838
synced_at: 2026-01-12T15:58:32Z
```

# Do not include `unsafe` dependencies in the compiled file unless some flag specified

---

_@drjackild_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hey guys, it's me again! 😅 `uv` includes some packages in the compiled file, which are considered `unsafe` (for example, [PEP 518](https://peps.python.org/pep-0518/#rationale) specifically asking to not pin `setuptools` unless you know what you're doing). By default, `pip-tools` do not pin `setuptools`, `pip` and `distibute` packages (https://github.com/jazzband/pip-tools/blob/9d0a91a382748a68f86d58a9e086b46a9b7c74f7/piptools/utils.py#L11).

You could tweak this behaviour by those options:

```
--allow-unsafe / --no-allow-unsafe
                                  Pin packages considered unsafe: distribute,
                                  pip, setuptools.

                                  WARNING: Future versions of pip-tools will
                                  enable this behavior by default. Use --no-
                                  allow-unsafe to keep the old behavior. It is
                                  recommended to pass the --allow-unsafe now
                                  to adapt to the upcoming change.
 --unsafe-package TEXT           Specify a package to consider unsafe; may be
                                  used more than once. Replaces default unsafe
                                  packages: distribute, pip, setuptools
```

Probably would be good to implement something in `uv`, wdyt?

---

_Comment by @hauntsaninja on 2024-02-21 22:30_

Duplicate of https://github.com/astral-sh/uv/issues/1353 and https://github.com/astral-sh/uv/issues/1415

Not sure how PEP 518 is relevant here, that would apply to build environments. If you have a compiled lock file for your build environment I think you'd need `--no-build-isolation` or similar to get it to actually work.

---

_Comment by @drjackild on 2024-02-21 22:42_

Ah, didn't search before creating an issue, sorry! Closing as duplicate

---

_Closed by @drjackild on 2024-02-21 22:42_

---
