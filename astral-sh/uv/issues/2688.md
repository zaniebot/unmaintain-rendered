```yaml
number: 2688
title: is uv pip sync with --find-links broken?
type: issue
state: closed
author: gwdekker
labels:
  - bug
assignees: []
created_at: 2024-03-27T09:49:49Z
updated_at: 2024-03-27T16:32:38Z
url: https://github.com/astral-sh/uv/issues/2688
synced_at: 2026-01-10T05:40:32Z
```

# is uv pip sync with --find-links broken?

---

_Issue opened by @gwdekker on 2024-03-27 09:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv version: 0.1.24, running on MacOS.

In my use case, I want to install dependencies, some of them living as .whl files in a local directory. When running `uv pip sync ... -find-links LOCAL_PATH the whls were not found. I made a small example that reproduces the case for me, with disabled network lookup to simplify the case

Case 1: uv pip install seems to work correctly
` uv -v pip install MY_PACKAGE --find-links /Users/OTHER_STUFF/libraries/.wheels --no-index` 
result: based on the logs, it clearly finds the package (and only starts failing later, as this package has external dependencies which it is now blocked to fetch due to no-index flag). Snippet from the log:

```
DEBUG Found 4 packages in `--find-links` entry: /Users/OTHER_STUFF/libraries/.wheels
DEBUG Solving with target Python version 3.11.4
DEBUG Adding direct dependency: MY_PACKAGE*
DEBUG Searching for a compatible version of MY_PACKAGE (*)
DEBUG Selecting: MY_PACKAGE==0.0.1 (MY_PACKAGE-0.0.1-py3-none-any.whl)
DEBUG Adding transitive dependency: pytest*
...
```

Case 2: uv pip sync seems to fail
`uv -v pip sync requirements-dev2.txt --find-links /Users/OTHER_STUFF/libraries/.wheels --no-index`

content of the requirements file: 
`MY_PACKAGE==0.0.1`

Snippet from the log:
```
INFO Found a virtualenv through VIRTUAL_ENV at: /Users/SOME_PATH
DEBUG Cached interpreter info for Python 3.11.4, skipping probing: .venv/bin/python
DEBUG Using Python 3.11.4 environment at .venv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Found 4 packages in `--find-links` entry: /Users/OTHER_STUFF/libraries/.wheels
DEBUG Identified uncached requirement: MY_PACKAGE ==0.0.1
DEBUG Unnecessary package: ruff==0.1.13
error: MY_PACKAGE isn't available locally, but making network requests to registries was banned.
```

Note: there actually is a whitespace in `DEBUG Identified uncached requirement: MY_PACKAGE ==0.0.1`. Could this be a pointer to what is wrong?

I also tried `pip-sync` for case 2 instead of `uv pip sync`, which also seems to work.

---

_Label `needs-mre` added by @charliermarsh on 2024-03-27 14:26_

---

_Comment by @charliermarsh on 2024-03-27 14:27_

I feel like the space probably _is_ indicative of what's going wrong, but I don't see how that could be happening. Can you upload the exact requirements file?

---

_Comment by @gwdekker on 2024-03-27 15:08_

[mre.zip](https://github.com/astral-sh/uv/files/14775265/mre.zip)

I made you a small MRE that replicates the issue on my side, see the README.md for instructions.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-27 15:09_

---

_Comment by @charliermarsh on 2024-03-27 15:10_

Thank you!

---

_Comment by @charliermarsh on 2024-03-27 15:13_

Okay, running on `main`, I do see `error: lib1 isn't available locally, but making network requests to registries was banned`, but I no longer see the space in `lib1==0.0.1`, so just gonna ignore that for now.

---

_Comment by @charliermarsh on 2024-03-27 15:16_

By the way, removing `--no-index` does succeed (but it should still succeed without it).

---

_Label `needs-mre` removed by @charliermarsh on 2024-03-27 15:20_

---

_Label `bug` added by @charliermarsh on 2024-03-27 15:20_

---

_Closed by @charliermarsh on 2024-03-27 16:15_

---

_Comment by @charliermarsh on 2024-03-27 16:29_

Thanks for the great MRE! Fixed in the next release.

---

_Comment by @gwdekker on 2024-03-27 16:31_

Thanks a lot! Just a quick check: does it work without —no-index because it finds the local library, or is there a lib1 on PyPI mirroring the local library and is the problem therefore masked?

---

_Comment by @charliermarsh on 2024-03-27 16:32_

It finds both (like pip), but it correctly selects the local library since you pinned the version (in my testing).

---
