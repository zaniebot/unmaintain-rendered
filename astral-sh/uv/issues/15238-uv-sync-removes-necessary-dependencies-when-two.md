```yaml
number: 15238
title: "`uv sync` removes necessary dependencies when two packages include the same module"
type: issue
state: open
author: jaseemabid
labels:
  - bug
assignees: []
created_at: 2025-08-12T12:54:27Z
updated_at: 2025-08-12T16:55:10Z
url: https://github.com/astral-sh/uv/issues/15238
synced_at: 2026-01-12T16:02:06Z
```

# `uv sync` removes necessary dependencies when two packages include the same module

---

_@jaseemabid_

### Bug Description

`uv sync --frozen --no-dev` removes runtime dependencies (`types-boto3-ecr`)
even though they are required in the lock file.

### Reproduction

The full project is here in a gist, but this is what I did to get there:

https://gist.github.com/jaseemabid/e2acd88b8ccb59e5d60b591247a4655c

This is a fairly tiny project with 2 runtime and 1 dev dependency.

```bash

❯ uv init
❯ uv add "boto3>=1.40"
❯ uv add "types-boto3-ecr>=1.40"

❯ uv add --dev "types-boto3[full]==1.40"
❯ uv sync
```

The full lockfile is included in the gist.

### Expected vs Actual

I expected this to work fine, since the package is explicitly requested:

```
❯ uv run --no-dev python -c 'import types_boto3_ecr'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import types_boto3_ecr
ModuleNotFoundError: No module named 'types_boto3_ecr'
```

This only works without the `--no-dev` flag, but I can't tell why.

```
❯ uv run python -c 'import types_boto3_ecr'
```

**Expected:** `types-boto3-ecr` should be available (it's a runtime dependency)
**Actual:** `ModuleNotFoundError` - package is missing despite being in runtime
dependencies

This is pretty concerning that a package I have explicitly mentioned in
project.toml which UV also added to uv.lock isn't in the final virtual env.

I had runtime crashes because of this issue and it's kinda bad.

### Environment

-   uv 0.8.6 (Homebrew 2025-08-07)
-   Python 3.13.6
-   Reproduced on macOS and Docker Linux

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.8.6 (Homebrew 2025-08-07)

### Python version

Python 3.13.6

---

_Label `bug` added by @jaseemabid on 2025-08-12 12:54_

---

_Comment by @charliermarsh on 2025-08-12 14:27_

I can't reproduce this:
```
❯ uv init foo
Initialized project `foo` at `/Users/crmarsh/workspace/uv/foo`

❯ uv add "boto3>=1.40"
Using CPython 3.14.0rc1
Creating virtual environment at: .venv
Resolved 8 packages in 169ms
Prepared 2 packages in 356ms
Installed 7 packages in 18ms
 + boto3==1.40.7
 + botocore==1.40.7
 + jmespath==1.0.1
 + python-dateutil==2.9.0.post0
 + s3transfer==0.13.1
 + six==1.17.0
 + urllib3==2.5.0

❯ uv add "types-boto3-ecr>=1.40"
Resolved 9 packages in 201ms
Prepared 1 package in 105ms
Installed 1 package in 2ms
 + types-boto3-ecr==1.40.0

❯ uv add --dev "types-boto3[full]==1.40"
Resolved 14 packages in 118ms
Prepared 5 packages in 810ms
Installed 5 packages in 86ms
 + botocore-stubs==1.38.46
 + types-awscrt==0.27.5
 + types-boto3==1.40.0
 + types-boto3-full==1.40.7
 + types-s3transfer==0.13.0

❯ uv venv
Using CPython 3.14.0rc1
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

❯ uv run --no-dev python -c 'import types_boto3_ecr'
Installed 8 packages in 25ms
```

Did you somehow corrupt your cache? Does `uv cache clean` help?

---

_Label `needs-mre` added by @charliermarsh on 2025-08-12 14:27_

---

_Comment by @charliermarsh on 2025-08-12 14:29_

Do you have example logs from `uv sync --frozen --no-dev` removing that package? There's no output of `uv sync` above.

---

_Comment by @jaseemabid on 2025-08-12 15:31_

@charliermarsh The example here doesn't show the package removed in logs but it's clearly breaking the next import. 

Here is a complete session:

1. Sync the packages. 

```
❯ uv sync
Resolved 14 packages in 1ms
Installed 5 packages in 117ms
 + botocore-stubs==1.38.46
 + types-awscrt==0.27.5
 + types-boto3==1.40.0
 + types-boto3-full==1.40.7
 + types-s3transfer==0.13.0
```

2. Package import works.

```
❯ .venv/bin/python -c 'import types_boto3_ecr'
```

3. Sync again with `--no-dev`. 

```
❯ uv sync --no-dev
Resolved 14 packages in 1ms
Uninstalled 5 packages in 400ms
 - botocore-stubs==1.38.46
 - types-awscrt==0.27.5
 - types-boto3==1.40.0
 - types-boto3-full==1.40.7
 - types-s3transfer==0.13.0
```

4. The logs don't mention removing `types-boto3-full` explicitly, but it's clearly gone now. 

```
❯ .venv/bin/python -c 'import types_boto3_ecr'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import types_boto3_ecr
ModuleNotFoundError: No module named 'types_boto3_ecr'
```

---

_Comment by @charliermarsh on 2025-08-12 15:53_

Thanks. The problem is that `types-boto3-full` overlaps with `types-boto3-ecr`. It's a superset of that package, and includes the same contents. So when `types-boto3-full` gets uninstalled, it removes the `types_boto3_ecr` module (since that's listed in the `RECORD` file), but it doesn't appear as uninstalled to any installers, because `types_boto3_ecr-1.40.0.dist-info` still exists.

I don't really know what we can do here -- it's kind of a natural fallout of how they've designed these packages, and isn't really specific to uv. You can see the exact same behavior with pip:

```
❯ python -m venv .venv
❯ .venv/bin/pip install types-boto3-ecr
❯ .venv/bin/pip install types-boto3-full
❯ .venv/bin/pip uninstall types-boto3-full
❯ .venv/bin/python -c 'import types_boto3_ecr'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import types_boto3_ecr
ModuleNotFoundError: No module named 'types_boto3_ecr'
```

Even reinstalling won't help, because the package appears to be installed:
```
❯ .venv/bin/pip install types-boto3-ecr
Requirement already satisfied: types-boto3-ecr in ./.venv/lib/python3.14/site-packages (1.40.0)
❯ .venv/bin/python -c 'import types_boto3_ecr'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import types_boto3_ecr
ModuleNotFoundError: No module named 'types_boto3_ecr'
```


---

_Label `needs-mre` removed by @charliermarsh on 2025-08-12 15:54_

---

_Comment by @charliermarsh on 2025-08-12 16:01_

It would be nice if we could either (1) detect incomplete installs or (2) avoid removing those files when they're referenced in another `RECORD`.

---

_Comment by @jaseemabid on 2025-08-12 16:28_

@charliermarsh Thanks, that makes sense. It took me a while to wrap my head around `RECORD` and how things can be deps but not shown in `uv tree` output. I'll go read up more on the details of it. 

---

_Comment by @charliermarsh on 2025-08-12 16:29_

Yeah, I think `types-boto3-ecr` isn't formally a dependency of `types-boto3-full` -- `types-boto3-full` just includes verbatim the `types_boto3_ecr` module, so it doesn't get tracked as a dependency, it just gets "inlined", so-to-speak.

---

_Comment by @jaseemabid on 2025-08-12 16:30_

@charliermarsh Also what workaround would you suggest so that uv would make sure that uv won't remove the package from runtime deps? 

---

_Comment by @charliermarsh on 2025-08-12 16:38_

I think the most robust thing, which is obviously not ideal, would be remove the `types-boto3-full` dependency and replace it with the enumerated packages that you need (like `types-boto3-ecr`, `types-boto3-billing`, etc.). Then uv would be able to track them correctly. Otherwise, I _think_ you'd have to do something like `uv sync --reinstall-package types-boto3-ecr` to force uv to reinstall it and ignore the existing `.dist-info`.


---

_Comment by @jaseemabid on 2025-08-12 16:53_

Gotcha. 

I understand this is a pretty odd edge case and I'll figure out a solution locally. 

In the long run it would be great if UV can infer these deps even if packages do silly crimes under the hood. 

---

_Comment by @charliermarsh on 2025-08-12 16:54_

I agree. Let's leave this open. (I may tweak the title.)

---

_Renamed from "`uv sync --frozen --no-dev` incorrectly removes runtime dependencies" to "`uv sync` removes necessary dependencies when two packages include the same module" by @charliermarsh on 2025-08-12 16:55_

---
