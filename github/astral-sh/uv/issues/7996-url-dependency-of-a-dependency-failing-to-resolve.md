---
number: 7996
title: URL dependency of a dependency failing to resolve
type: issue
state: closed
author: ringohoffman
labels:
  - question
assignees: []
created_at: 2024-10-08T07:57:30Z
updated_at: 2024-10-09T07:28:38Z
url: https://github.com/astral-sh/uv/issues/7996
synced_at: 2026-01-07T13:12:17-06:00
---

# URL dependency of a dependency failing to resolve

---

_Issue opened by @ringohoffman on 2024-10-08 07:57_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```console
$ uv --version
uv 0.4.19
```

Related:

* https://github.com/astral-sh/uv/issues/1932#issuecomment-2155660516
* https://github.com/astral-sh/uv/issues/4152

I have a package published to AWS Code Artifact that has a `pyproject.toml` that looks something like this:

```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
]

# https://packaging.python.org/en/latest/specifications/declaring-project-metadata/
[project]
name = "my_package"
dynamic = ["version"]
dependencies = [
    "transformers[torch] @ git+https://github.com/huggingface/transformers@1f33023cfa998c237372baff36b166c014f33405#egg=transformers",
]
```

When I try to install it, I get the same error as in the 2 linked issues:

```
error: Package `transformers` attempted to resolve via URL: git+https://github.com/huggingface/transformers@1f33023cfa998c237372baff36b166c014f33405#egg=transformers. URL dependencies must be expressed as direct requirements or constraints. Consider adding `transformers @ git+https://github.com/huggingface/transformers@1f33023cfa998c237372baff36b166c014f33405#egg=transformers` to your dependencies or constraints file.
```

`pip` is able to install it though.

I would try to publish a package like this to PyPI as a MRE, but I think PyPI specifically forbids publishing packages with direct links.

So, similar to the recursive dependency in #4152, is there another case not being considered here? As in when the link is coming from the package you are trying to install.

---

_Renamed from "URL dependency" to "URL dependency of a dependency failing to resolve" by @ringohoffman on 2024-10-08 07:59_

---

_Comment by @charliermarsh on 2024-10-08 22:32_

Is this a dupe of https://github.com/astral-sh/uv/issues/5070?

---

_Label `question` added by @charliermarsh on 2024-10-08 22:32_

---

_Comment by @ringohoffman on 2024-10-09 05:02_

> Is this a dupe of #5070?

Yes seems like it.

---

_Closed by @charliermarsh on 2024-10-09 07:28_

---

_Comment by @charliermarsh on 2024-10-09 07:28_

(Merging.)

---
