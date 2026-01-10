---
number: 11163
title: Panic when linting (VS Code extension)
type: issue
state: closed
author: xremming
labels:
  - bug
  - parser
assignees: []
created_at: 2024-04-26T12:01:06Z
updated_at: 2024-05-03T00:34:07Z
url: https://github.com/astral-sh/ruff/issues/11163
synced_at: 2026-01-10T01:22:50Z
---

# Panic when linting (VS Code extension)

---

_Issue opened by @xremming on 2024-04-26 12:01_

Ruff crashed when running using the VS Code plugin. Unfortunately the full error message was not printed. I also do not know what file was open or what it was trying to parse when the error happened. The error has also not happened again.

If you can provide me with instructions on how I might be able to either find logs on what happened or how to catch the error better I can try to bug hunt this more.

> Ruff: Lint failed ( error: Ruff crashed. If you could open an issue at: https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D ...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative! thread 'main' panicked at /home/runner/work/ruff/ruff/crates/ruff_text_size/src/range.rs:48:9: assertion failed: start.raw <= end.raw note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace )

Python version: `Python 3.11.8`
OS: Windows 11 + WSL2 Arch

<details>
<summary>pyproject.toml</summary>

```
[tool.poetry]
name = "REDACTED"
version = "1.0.0"
license = "UNLICENSED"
description = "REDACTED"
authors = ["REDACTED"]
readme = "README.md"
package-mode = false

[tool.poetry.dependencies]
python = "^3.11"
pydantic = "^2.7.0"
boto3 = "^1.34.88"
rich = "^13.7.1"

[tool.poetry.group.dev.dependencies]
ruff = "^0.4.0"
mypy = "^1.9.0"

[tool.ruff]
line-length = 88
src = ["./tools/"]

[tool.ruff.format]
docstring-code-format = true

[tool.ruff.lint]
select = ["ALL"]
ignore = [
  "E501",
  "TID252",
  "COM812",
  "ISC001",
  "S101",
  "ANN101",
  "FA",
  "CPY",
  "D",
]

[tool.mypy]
strict = true
plugins = "pydantic.mypy"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

</details>

<details>
<summary>Relevant section from poetry.lock.</summary>

```
[[package]]
name = "ruff"
version = "0.4.0"
description = "An extremely fast Python linter and code formatter, written in Rust."
optional = false
python-versions = ">=3.7"
files = [
    {file = "ruff-0.4.0-py3-none-macosx_10_12_x86_64.macosx_11_0_arm64.macosx_10_12_universal2.whl", hash = "sha256:70b8c620cf2212744eabd6d69c4f839f2be0d8880d27beaeb0adb6aa0b316aa8"},
    {file = "ruff-0.4.0-py3-none-macosx_10_12_x86_64.whl", hash = "sha256:cfa3e3ff53be05a8c5570c1585ea1e089f6b399ca99fcb78598d4a8234f248db"},
    {file = "ruff-0.4.0-py3-none-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:5616cca501d1d16b932b7e607d7e1fd1b8c8c51d6ee484b7940fc1adc5bea541"},
    {file = "ruff-0.4.0-py3-none-manylinux_2_17_armv7l.manylinux2014_armv7l.whl", hash = "sha256:46eff08dd480b5d9b540846159fe134d70e3c45a3c913c600047cbf7f0e4e308"},
    {file = "ruff-0.4.0-py3-none-manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:2d546f511431fff2b17adcf7110f3b2c2c0c8d33b0e10e5fd27fd340bc617efc"},
    {file = "ruff-0.4.0-py3-none-manylinux_2_17_ppc64.manylinux2014_ppc64.whl", hash = "sha256:c7b6b6b38e216036284c5779b6aa14acbf5664e3b5872533219cf93daf40ddfb"},
    {file = "ruff-0.4.0-py3-none-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:5e1cf8b064bb2a6b4922af7274fe2dffcb552d96ba716b2fbe5e2c970ed7de18"},
    {file = "ruff-0.4.0-py3-none-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:9911c9046b94253e1fa844c0192bb764b86866a881502dee324686474d498c17"},
    {file = "ruff-0.4.0-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:4ca7a971c8f1a0b6f5ff4a819c0d1c2619536530bbd5a289af725d8b2ef1013d"},
    {file = "ruff-0.4.0-py3-none-musllinux_1_2_aarch64.whl", hash = "sha256:752e0f77f421141dd470a0b1bed4fd8f763aebabe32c80ed3580f740ef4ba807"},
    {file = "ruff-0.4.0-py3-none-musllinux_1_2_armv7l.whl", hash = "sha256:84f2a5dd8f33964d826c5377e094f7ce11e55e432cd42d3bf64efe4384224a03"},
    {file = "ruff-0.4.0-py3-none-musllinux_1_2_i686.whl", hash = "sha256:0b20e7db4a672495320a8a18149b7febf4e4f97509a4657367144569ce0915fd"},
    {file = "ruff-0.4.0-py3-none-musllinux_1_2_x86_64.whl", hash = "sha256:0b0eddd339e24dc4f7719b1cde4967f6b6bc0ad948cc183711ba8910f14aeafe"},
    {file = "ruff-0.4.0-py3-none-win32.whl", hash = "sha256:e70befd488271a2c28c80bd427f73d8855dd222fc549fa1e9967d287c5cfe781"},
    {file = "ruff-0.4.0-py3-none-win_amd64.whl", hash = "sha256:8584b9361900997ccf8d7aaa4dc4ab43e258a853ca7189d98ac929dc9ee50875"},
    {file = "ruff-0.4.0-py3-none-win_arm64.whl", hash = "sha256:fea4ec813c965e40af29ee627a1579ee1d827d77e81d54b85bdd7b42d1540cdd"},
    {file = "ruff-0.4.0.tar.gz", hash = "sha256:7457308d9ebf00d6a1c9a26aa755e477787a636c90b823f91cd7d4bea9e89260"},
]
```

</details>

---

_Comment by @MichaReiser on 2024-04-26 12:11_

Thanks for the very detailed write up. I think that might be related to a parser bug in 0.4. Can you try upgrading to 0.4.1 or newer.

---

_Comment by @xremming on 2024-04-26 12:20_

Sure! I upgraded to latest locally (seems to be 0.4.2) and will report if I see this error again. We get updates through Dependabot PRs weekly on Mondays.

---

_Comment by @charliermarsh on 2024-05-03 00:34_

I'm going to optimistically close based on fixes we shipped to the parser, but if you see it again just let me know and we'll re-open.

---

_Closed by @charliermarsh on 2024-05-03 00:34_

---

_Label `bug` added by @charliermarsh on 2024-05-03 00:34_

---

_Label `parser` added by @charliermarsh on 2024-05-03 00:34_

---
