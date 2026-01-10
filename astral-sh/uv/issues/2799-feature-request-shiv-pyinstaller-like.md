```yaml
number: 2799
title: "feature request : shiv/pyinstaller like functionality?"
type: issue
state: open
author: sahmad-d2x
labels:
  - wish
assignees: []
created_at: 2024-04-03T07:23:45Z
updated_at: 2024-04-07T00:29:45Z
url: https://github.com/astral-sh/uv/issues/2799
synced_at: 2026-01-10T05:40:32Z
```

# feature request : shiv/pyinstaller like functionality?

---

_Issue opened by @sahmad-d2x on 2024-04-03 07:23_

Hi,

One of things I have often found pip lacking has been `shiv` or `pyinstaller` like functionality. It would be amazing (and `cargo/rust`, `go-like` if uv could build out binaries/zipped virtualenvs  like `shiv` or `pyinstaller` do. I realize that this might be out of scope for pip (it installs packages and does not build them?) but I think since this particular task is also packaging-centric, pip or uv should provide the feature/ability.

https://shiv.readthedocs.io/en/latest/
https://pyinstaller.org/en/stable/

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Renamed from "shiv-like functionality?" to "{shiv-,pyinstaller-} like functionality?" by @sahmad-d2x on 2024-04-03 07:27_

---

_Renamed from "{shiv-,pyinstaller-} like functionality?" to "shiv/pyinstaller like functionality?" by @sahmad-d2x on 2024-04-03 07:27_

---

_Renamed from "shiv/pyinstaller like functionality?" to "feature request : shiv/pyinstaller like functionality?" by @sahmad-d2x on 2024-04-03 07:28_

---

_Comment by @samypr100 on 2024-04-05 02:53_

From purely a build perspective, if uv had an exposed [pip wheel](https://pip.pypa.io/en/stable/cli/pip_wheel/) equivalent, it might be close enough to satisfy this request and stick to standards. On the other hand, `pyinstaller` also bundles python into the mix, so it serves the different purpose by of freezing python and it's dependencies into a platform specific single binary. Definitely a wish to have something like that experience. I think once the low level tooling is flushed out, something like that can become closer to being achievable.

---

_Label `wish` added by @charliermarsh on 2024-04-07 00:29_

---
