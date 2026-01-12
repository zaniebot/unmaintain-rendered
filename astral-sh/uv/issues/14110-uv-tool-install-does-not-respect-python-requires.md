```yaml
number: 14110
title: uv tool install does not respect python-requires
type: issue
state: open
author: gsemet
labels:
  - question
assignees: []
created_at: 2025-06-17T16:43:35Z
updated_at: 2025-11-07T17:20:30Z
url: https://github.com/astral-sh/uv/issues/14110
synced_at: 2026-01-12T16:01:43Z
```

# uv tool install does not respect python-requires

---

_@gsemet_

### Summary

Hello

i have a package that requires python <3.13. In my package `pyproject.toml` i have:
```toml
requires-python = ">=3.9,<3.13"
```

when installing this tool using `uv tool install`, i see the interpreter used is 3.13, which `python` and `python3` alias points to 3.12 by default

```text
$ uv python list
cpython-3.14.0b2-linux-x86_64-gnu                 <download available>
cpython-3.14.0b2+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.13.5-linux-x86_64-gnu                   <download available>
cpython-3.13.5+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.13.3-linux-x86_64-gnu                   /home/myuser/.local/share/uv/python/cpython-3.13.3-linux-x86_64-gnu/bin/python3.13
cpython-3.12.11-linux-x86_64-gnu                  <download available>
cpython-3.12.3-linux-x86_64-gnu                   /usr/bin/python3.12
cpython-3.12.3-linux-x86_64-gnu                   /usr/bin/python3 -> python3.12
cpython-3.11.13-linux-x86_64-gnu                  <download available>
cpython-3.11.12-linux-x86_64-gnu                  /home/myuser/.local/share/uv/python/cpython-3.11.12-linux-x86_64-gnu/bin/python3.11
cpython-3.10.18-linux-x86_64-gnu                  <download available>
cpython-3.10.17-linux-x86_64-gnu                  /home/myuser/.local/share/uv/python/cpython-3.10.17-linux-x86_64-gnu/bin/python3.10
cpython-3.10.16-linux-x86_64-gnu                  /home/myuser/.local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu/bin/python3.10
cpython-3.10.14-linux-x86_64-gnu                  /home/myuser/.local/share/uv/python/cpython-3.10.14-linux-x86_64-gnu/bin/python3.10
cpython-3.9.23-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
pypy-3.11.11-linux-x86_64-gnu                     <download available>
pypy-3.10.16-linux-x86_64-gnu                     <download available>
pypy-3.9.19-linux-x86_64-gnu                      <download available>
pypy-3.8.16-linux-x86_64-gnu                      <download available>
graalpy-3.11.0-linux-x86_64-gnu                   <download available>
graalpy-3.10.0-linux-x86_64-gnu                   <download available>
graalpy-3.8.5-linux-x86_64-gnu                    <download available>
````

i see my app is installed in `/home/myuser/.local/share/uv/tools/agpkg/lib/python3.13/site-packages/mytoolname`.

When i force installation to python 3.12, it works:

```bash
uv tool install --python 3.12 mytoolname
```

Is it possible `uv tool install` does not respect the python-requires from a given package?

### Platform

Windows WSL2

### Version

0.7.13

### Python version

python 3.12

---

_Label `bug` added by @gsemet on 2025-06-17 16:43_

---

_Comment by @zanieb on 2025-06-17 16:45_

The `uv tool` interface doesn't ever read from a `pyproject.toml`, since tool installations are _global_ and your project is _local_ state.

---

_Label `bug` removed by @zanieb on 2025-06-17 16:45_

---

_Label `question` added by @zanieb on 2025-06-17 16:45_

---

_Comment by @gsemet on 2025-06-17 16:45_

yes, but the wheel generated from my package should have this restriction defined, and `uv tool install` reads this wheel.

In the wheel's METADATA I see:

```text
...
Classifier: Topic :: Software Development :: Libraries :: Python Modules
Requires-Python: <3.13,>=3.9
Requires-Dist: colorama
Requires-Dist: dohq-artifactory>=1
Requires-Dist: dpath
Requires-Dist: extsemver>=2.10.4
Requires-Dist: jsonpath
````

This `Requires-Python: <3.13,>=3.9` should tell `uv tool install` to not use python 3.13.

---

_Comment by @zanieb on 2025-06-17 16:48_

I see, I misunderstood what you were saying.

We ignore upper bounds on `requires-python` during resolution, presumably we do so there as well (see https://github.com/astral-sh/uv/issues/8374#issuecomment-2424246009). I can verify that's what's happening.

---

_Comment by @gsemet on 2025-06-17 16:49_

interesting. In my use case, one module (dohq-artifactory) does not work properly in python 3.13, so i need a way to prevent use of this version of python. I have validated my application against python 3.10, 3.11 and 3.12.

---

_Comment by @zanieb on 2025-06-17 16:53_

Yeah I'm not sure we have a way to do that right now. It seems reasonable to add something for this particular use-case, but it'll be hard.

---

_Comment by @zanieb on 2025-06-17 16:53_

Why does it fail on 3.13? (just wondering)

---

_Comment by @gsemet on 2025-06-17 16:58_

this lib kind of mimick pathlib behavior, but against artifactory, and pathlib has been reworked in 3.12, 3.13. the lib author fixed for 3.12, but i have some globing syntax that does not work in 3.13.

my problem is that all my users that using `uv tool install mytool` can have a breakage if uv find a python 3.13 on their system, despit my careful annotation in the wheel to not use 3.13

if i remove python 3.13 from the system it will install with 3.12 that i still have in my system. also, if i fix the python interpreter with `uv tool install --python 3.12 agpkg` it works, but it is more error prone for users.

i do not really see a reason why uv or any other tool would decide to not respect an explicit METADATA annotation, done by the author of a wheel. Maybe this is not standard, but the syntax seem supported.
I deliver an application, that i validate against some dependency/interpreter version, so i need to be sure the installation tool will install the same version that i have validated.

i have the same kind of issue with dependencies (for my application, i think i should pin all dependencies in the wheel, leaving no resolution capabilities to the installer), but i did not cross this rubicon yet.

but at least the upper bound of python-requires, if defined, should be enforced, at least for uv tool install

---

_Comment by @deeagle001 on 2025-09-22 08:56_

Are there any progress in this topic? I have exactly the same issue as the reporter, with the same package (dohq-artifactory).
As a solution, I would rather appreciate a flag like "--strict-requires-version" than force the user to read the same info manually, figure out his own suitable python version and provide it to the "--python" flag.

---

_Comment by @powercoconola on 2025-10-15 20:10_

Looking to get this type of support as well. For us it is keeping us from incrementally updating tools as we test them, it seems the only way to do it is with the UV_PYTHON environment variable to pin the version to a specific version. This acts an "upper-bound" for tool installs. But now if I validate one of my tools with 3.13 or 3.14, I can't just update that one tool since UV_PYTHON ensures ALL tools are installed that way.

---

_Comment by @vikhik on 2025-11-07 15:31_

We are also looking for this support - we have an internal build tool which uses some libraries that require `pywin32` - and that currently has no support for `cp314` ABI, so anyone who is setting up a new machine runs into this issue. I would like to avoid them needing to specify `--python 3.13` in the `uv tool install <tool>` call, because we already specify the python version in our .toml, so it's extraneous data.

---

_Comment by @vikhik on 2025-11-07 17:20_

Just to play around and figure out what would be necessary to make this work - and I managed to make it work locally.

Note: I am not a rust programmer, and I heavily used AI to assist. **THIS IS JUST A PROOF OF CONCEPT**
However, I tested it for my local package, and it works. So at least it can be used as a conversation starter for the actual uv maintainers.

https://github.com/vikhik/uv/compare/main...vikhik:uv:feat/respect-requires-python-for-tools

---
