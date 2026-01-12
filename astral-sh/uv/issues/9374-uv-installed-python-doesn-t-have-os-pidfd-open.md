```yaml
number: 9374
title: "uv installed python doesn't have os.pidfd_open"
type: issue
state: open
author: jennifgcrl
labels:
  - external
  - uv python
assignees: []
created_at: 2024-11-23T01:06:11Z
updated_at: 2024-11-26T00:06:11Z
url: https://github.com/astral-sh/uv/issues/9374
synced_at: 2026-01-12T15:59:48Z
```

# uv installed python doesn't have os.pidfd_open

---

_@jennifgcrl_

os.pidfd_open has been supported since python 3.9 and kernel 5.2, but despite this recent python versions pulled by uv don't support os.pidfd_open. It would be nice if either upstream provided builds with `os.pidfd_open` enabled, or if uv provided more sources for python builds, or if uv had the option to build python from source.

Related: https://github.com/indygreg/python-build-standalone/issues/193

---

_Comment by @samypr100 on 2024-11-23 02:02_

Semi related https://github.com/astral-sh/uv/pull/8517

---

_Label `upstream` added by @samypr100 on 2024-11-23 02:08_

---

_Label `uv python` added by @samypr100 on 2024-11-23 02:08_

---

_Comment by @TurtleOrangina on 2024-11-23 11:18_

Adding the option to build python from source would be nice for developers looking for maximum performance from their python version as well. Using https://github.com/indygreg/python-build-standalone is much quicker to download and run for a one-off python execution, so ideal for the default uv behavior, but for somebody who wants to use the best python possible for his system and does not mind waiting a bit longer for it to build, the option for a source compiled python (like pyenv does) should be an improvement.

---

_Comment by @zanieb on 2024-11-26 00:06_

We don't have plans to build Python from source on the user's machine, there are other tools better suited for that. We're already investing a great deal into the standalone builds, which _are_ pretty heavily optimized. I'd prefer to invest my time into weak symbols upstream as suggested by Greg in https://github.com/indygreg/python-build-standalone/issues/193#issuecomment-1751971155

---
