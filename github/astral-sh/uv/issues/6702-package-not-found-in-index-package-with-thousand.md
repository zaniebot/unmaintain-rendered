---
number: 6702
title: Package not found in index ~Package with thousand of versions cannot be found~
type: issue
state: closed
author: mina-asham
labels:
  - question
assignees: []
created_at: 2024-08-27T17:00:22Z
updated_at: 2024-08-27T19:04:37Z
url: https://github.com/astral-sh/uv/issues/6702
synced_at: 2026-01-07T13:12:17-06:00
---

# Package not found in index ~Package with thousand of versions cannot be found~

---

_Issue opened by @mina-asham on 2024-08-27 17:00_

Hi, we have an internal registry, a package we use is setup for automated releases, and thus we have over 11K versions released!
If I try to add that package (`xyz>=1.2.3` for obfuscation), I get this error:
```
> uv pip install "xyz>=1.2.3"
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of xyz and you require xyz>=1.2.3, we can conclude that your requirements are
      unsatisfiable.
```

I am trying to switch away from pip/pip-tools for the obvious benefits but hitting this issue, for reference I used to get this issue with `pip` and `pip cache purge` used to fix the issue for that package.

This package for us have >11K releases, specifically:
```
> pip index versions xyz | sed "s/,/\n/g" | wc -l
  11039
```

Using latest `uv` version: `0.3.4`

I realise that I am not sharing a ton of details to get you to solve this, but I am more than happy to run debug commands, obfuscate them and share results here.

---

_Comment by @charliermarsh on 2024-08-27 17:01_

Do you have the index configured somewhere, like in `pip.conf`? If so, you'll need to add a `uv.toml` with something like:

```toml
index-url = "..."
```

---

_Label `question` added by @charliermarsh on 2024-08-27 17:01_

---

_Comment by @mina-asham on 2024-08-27 17:03_

Yes, the index is stored in `~/.config/uv/uv.toml`:
```
> cat ~/.config/uv/uv.toml
index-url = "OBFUSCATED"
extra-index-url = ["https://pypi.python.org/simple"]
```

I can also confirm that ALL our other private packages work fine, but that's the only one releasing a TON of packages and reaching thousands.

---

_Comment by @charliermarsh on 2024-08-27 17:04_

Ohh interesting, I see... Can you run with `--verbose` and see if there are any warnings or similar? I know you can't share the full logs, but the more of `--verbose` you can share, the more we can help.

---

_Comment by @mina-asham on 2024-08-27 17:10_

The verbose output:
```
> uv pip install --verbose "xyz>=1.2.3"
DEBUG uv 0.3.4 (Homebrew 2024-08-26)
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.9.7-macos-aarch64-none` at `/Users/minaasham/miniconda3/bin/python3` (conda prefix)
DEBUG Using Python 3.9.7 environment at miniconda3/bin/python3
DEBUG Acquired lock for `miniconda3`
DEBUG At least one requirement is not satisfied: xyz>=1.2.3
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.9.7
DEBUG Adding direct dependency: xyz>=1.2.3
DEBUG Found stale response for: https://pypi.python.org/simple/xyz/
DEBUG Sending revalidation request for: https://pypi.python.org/simple/xyz/
DEBUG Found not-modified response for: https://pypi.python.org/simple/xyz/
DEBUG Searching for a compatible version of xyz (>=1.2.3)
DEBUG No compatible version found for: xyz
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of xyz and you require xyz>=1.2.3, we can conclude that your requirements are
      unsatisfiable.
```

---

_Comment by @charliermarsh on 2024-08-27 17:11_

Ok, so I'd suggest switch PyPI and your index -- like:

```toml
index-url = "https://pypi.python.org/simple"
extra-index-url = ["OBFUSCATED"]
```

We search in the extra index _first_, and if a package exists on that index, we stop looking, to prevent dependency confusion attacks (this behavior is configurable -- you can opt-out).

Does the package _also_ exist on PyPI? I don't know if `xyz` was literal but it does: https://pypi.org/project/xyz/


---

_Comment by @mina-asham on 2024-08-27 17:55_

`xyz` is just obfuscation, but yes the real package has a pypi version, so the name is shadowed, I would have thought that the index-url is looked up first liked pip does?

---

_Comment by @charliermarsh on 2024-08-27 17:56_

We use the extra index first, since it's very common for users to use PyTorch as the extra index, for example. There's more on it here: https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes.

(In the future, we're going to introduce a better API for this entirely.)

---

_Comment by @mina-asham on 2024-08-27 17:57_

I was able to make it work with `--index-strategy=unsafe-first-match` and `--index-strategy=unsafe-best-match`

---

_Comment by @charliermarsh on 2024-08-27 17:57_

Yeah, either of those would work. `--index-strategy=unsafe-best-match` is the closest to pip.

---

_Comment by @mina-asham on 2024-08-27 18:00_

Thank you! I think we can resolve this for now, there is no actual issue, I thought it was due to the massive number of versions but that was a red herring.

---

_Closed by @mina-asham on 2024-08-27 18:00_

---

_Comment by @charliermarsh on 2024-08-27 18:16_

Sorry for the confusion around index ordering. I think we would've gotten issues like this regardless of the default we chose, just because we opted for a different model (we stop as soon as we find a package on the first matching registry, by default).

---

_Renamed from "Package with thousand of versions cannot be found" to "~Package with thousand of versions cannot be found~" by @mina-asham on 2024-08-27 19:04_

---

_Renamed from "~Package with thousand of versions cannot be found~" to "Package not found in index ~Package with thousand of versions cannot be found~" by @mina-asham on 2024-08-27 19:04_

---
