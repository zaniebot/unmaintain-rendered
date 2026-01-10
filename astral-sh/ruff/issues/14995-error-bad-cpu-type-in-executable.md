---
number: 14995
title: Error - Bad CPU type in executable
type: issue
state: closed
author: sr-murthy
labels:
  - needs-info
assignees: []
created_at: 2024-12-15T23:20:07Z
updated_at: 2024-12-16T07:56:33Z
url: https://github.com/astral-sh/ruff/issues/14995
synced_at: 2026-01-10T01:22:55Z
---

# Error - Bad CPU type in executable

---

_Issue opened by @sr-murthy on 2024-12-15 23:20_

I'm running a Ruff linting step (`ruff check`) in a CI test job matrix on GitHub Actions which fails currently on all MacOS 13 x86_64 runners, for the Python versions 3.11-3.13,  with the following error:
```shell
/bin/bash: /Users/runner/work/<project>/<project>/.venv/bin/ruff: Bad CPU type in executable
```
The linting step is run inside a virtualenv located in the working directory, which contains the Ruff executable (`.venv/bin/ruff`), and as a test I printed out the file information on Ruff, before running the check, which shows the following:
```shell
.venv/bin/ruff: Mach-O 64-bit executable arm64
```
So it seems the Ruff version that was installed in each of these test jobs on these MacOS 13 x64_64 runners is for Arm64. Do you have different versions for different platforms? The installation is via [PDM](https://github.com/pdm-project/pdm), but I don't think PDM is the cause.

Note that the jobs succeed for Mac OS 14-15 x86_64 runners regardless of Python version, and I see the same Ruff build info. as above, i.e. `Mach-O 64-bit executable arm64`.

Can you possibly explain or suggest a solution?

---

_Label `needs-info` added by @MichaReiser on 2024-12-16 07:32_

---

_Comment by @MichaReiser on 2024-12-16 07:38_

Hi @sr-murthy 

Hmm interesting. We do build and publish different binaries for different platforms and architecture and that includes a [darwin-x64](https://pypi.org/project/ruff/#ruff-0.8.3-py3-none-macosx_10_12_x86_64.whl) target. 

Does PDM have a debug mode where it shows the wheel build output? If so, would you mind sharing it. 

---

_Comment by @sr-murthy on 2024-12-16 07:54_

@MichaReiser It works now.

I cleared the project pip cache on GH, and it has now detected and installed the correct Ruff wheel on the x86_64 runners. Here is some log output for the relevant steps prior to the lint:
```shell
...
unearth.preparer: Downloading <Link https://files.pythonhosted.org/packages/ef/c5/0aabdc9314b4b6f051168ac45227e2aa8e1c6d82718a547455e40c9c9faa/ruff-0.8.3-py3-none-macosx_10_12_x86_64.whl (from https://pypi.org/simple/ruff/)> (10.3 MB)
  ✔ Install ruff 0.8.3 successful
...
.venv/bin/ruff: Mach-O 64-bit executable x86_64
...
```

---

_Closed by @sr-murthy on 2024-12-16 07:55_

---
