```yaml
number: 364
title: Support resolving for an alternate Python distribution
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/alternate-metadata
created_at: 2023-11-08T04:27:33Z
updated_at: 2023-11-08T23:19:17Z
url: https://github.com/astral-sh/uv/pull/364
synced_at: 2026-01-10T15:50:28Z
```

# Support resolving for an alternate Python distribution

---

_Pull request opened by @charliermarsh on 2023-11-08 04:27_

## Summary

Low-priority but fun thing to end the day. You can now pass `--target-version py37`, and we'll generate a resolution for Python 3.7.

See: https://github.com/astral-sh/puffin/issues/183.


---

_Review requested from @zanieb by @charliermarsh on 2023-11-08 04:27_

---

_Review requested from @konstin by @charliermarsh on 2023-11-08 04:27_

---

_Label `enhancement` added by @charliermarsh on 2023-11-08 04:27_

---

_Review comment by @konstin on `crates/puffin-cli/src/python_version.rs`:6 on 2023-11-08 09:49_

I'd add a from string with `3.7`, `3.8`, etc., virtualenv usees this (`virtualenv -p 3.8`)

---

_Review comment by @konstin on `crates/puffin-cli/src/python_version.rs`:41 on 2023-11-08 09:55_

That isn't correct for pypy:
```
$ cat pep508_script.py 
import sys

def format_full_version(info):
    version = '{0.major}.{0.minor}.{0.micro}'.format(info)
    kind = info.releaselevel
    if kind != 'final':
        version += kind[0] + str(info.serial)
    return version

if hasattr(sys, 'implementation'):
    implementation_version = format_full_version(sys.implementation.version)
else:
    implementation_version = "0"

print(implementation_version)
$  /home/konsti/.pyenv/versions/pypy3.10-7.3.13/bin/pypy3 pep508_script.py
7.3.13
```

I'm not sure what the correct implementation is - Evaluate every comparison with `implementation_version` to true, because it could be true? It's not a widely used marker: https://github.com/search?q=path%3A%2Frequirements.*%5C.txt%2F+implementation_version&type=code&query=path%3Arequirements.txt+implementation_version A cheaper solution would be leaving the field empty or setting the value to `[elided]`.

---

_@konstin reviewed on 2023-11-08 10:03_

An interesting thing that we can do is to check version intersections: Use `>=3.10` as input instead of `3.10` and check whether the `python_version` version intersects with out current one (`==3.12`, `<3.11` and `!=3.11` intersect, `<3.8` does not not)

---

_@konstin reviewed on 2023-11-08 10:04_

---

_Review comment by @konstin on `crates/puffin-cli/src/python_version.rs`:41 on 2023-11-08 10:04_

This is the one thing i think we should fix before merging

---

_@charliermarsh reviewed on 2023-11-08 15:01_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/python_version.rs`:41 on 2023-11-08 15:01_

Oh hmm, I'd say we should consider just not touching it here, just as we aren't touching `implementation_name`.

---

_@charliermarsh reviewed on 2023-11-08 15:19_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/python_version.rs`:41 on 2023-11-08 15:19_

@konstin - How's this -- we set the version if you're using CPython, and preserve it if you're using anything else.

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:584 on 2023-11-08 15:32_

Do we need to use `--target-python`, `--python-version`, or `--target-python-version` for clarity? `--target-version` seems a little vague in the context of a resolver. I think I have a preference for just `--python-version` since that's what you're overriding unless we are supporting different target python builds in which `--target-python` makes sense.

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:585 on 2023-11-08 15:32_

Why this format? I'd expect "3.12"

---

_@zanieb reviewed on 2023-11-08 15:32_

---

_@zanieb reviewed on 2023-11-08 15:34_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:585 on 2023-11-08 15:34_

Is there a plan to support like `pypy310`?

---

_@charliermarsh reviewed on 2023-11-08 15:36_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:584 on 2023-11-08 15:36_

Hmm, well the current naming and version scheme is identical to what we use in Ruff and what's used in Black. Either way we're going to deviate from some tools so I just opted to be consistent with Ruff.

---

_@zanieb reviewed on 2023-11-08 15:38_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:584 on 2023-11-08 15:38_

I think in Black and Ruff there aren't any other target versions to be concerned about but for a package manager it feels too vague.

---

_@konstin reviewed on 2023-11-08 16:23_

---

_Review comment by @konstin on `crates/puffin-cli/src/python_version.rs`:41 on 2023-11-08 16:23_

Sounds fine

---

_@konstin reviewed on 2023-11-08 16:28_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:584 on 2023-11-08 16:28_

virtualenv has
```
-p py, --python py            interpreter based on what to create environment (path/identifier) - by default use the interpreter where the tool is installed - first found wins (default: [])
```
, rye uses `rye pin 3.10` and `python +3.8 my-script.py`.

Wheel tags use `py3`, `py39`, `cp10`, `pypy37` (https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#python-tag), which i think is where that syntax comes from.

---

_@charliermarsh reviewed on 2023-11-08 19:45_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:584 on 2023-11-08 19:45_

I changed it to `--python-version`.

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:584 on 2023-11-08 19:46_

I haven't changed the input format yet, I may defer it for now.

---

_@charliermarsh reviewed on 2023-11-08 19:46_

---

_Review requested from @konstin by @charliermarsh on 2023-11-08 19:46_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-08 19:46_

---

_@zanieb reviewed on 2023-11-08 22:30_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:584 on 2023-11-08 22:30_

Thanks! Input format seems fine.

---

_@zanieb approved on 2023-11-08 22:30_

---

_Merged by @charliermarsh on 2023-11-08 23:19_

---

_Closed by @charliermarsh on 2023-11-08 23:19_

---

_Branch deleted on 2023-11-08 23:19_

---
