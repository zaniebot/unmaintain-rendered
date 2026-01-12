```yaml
number: 7028
title: Application of config settings when installing package depends on content of uv cache
type: issue
state: closed
author: smackesey
labels:
  - bug
  - cache
assignees: []
created_at: 2024-09-04T15:14:26Z
updated_at: 2024-09-10T01:49:18Z
url: https://github.com/astral-sh/uv/issues/7028
synced_at: 2026-01-12T15:59:09Z
```

# Application of config settings when installing package depends on content of uv cache

---

_@smackesey_

We use `uv` to build the python environments that we run pyright against for our large codebase [Dagster](https://github.com/dagster-io/dagster). Pyright environments do not handle Python import hooks correctly. It is therefore important that all editables installs in the environment use a "legacy" style, where the pth file contains a static path instead of an import hook.

Modern style:

```
### __editable__.foo-0.1.0.pth

import __editable___foo_0_1_0_finder; __editable___foo_0_1_0_finder.install()
```

Legacy style:

```
### __editable__.foo-0.1.0.pth

/path/to/foo
```

The way you force a legacy style when `setuptools` is used as build backend (which is what our packages use) is to pass `editable_mode=compat` as a config option. So this should work, and has worked in the past:

```
uv pip install --config-setting editable_mode=compat -e my_package
```

However, it seems like the cache is interfering in this command working properly. If the package has already been installed as editable without `editable_mode=compat` in one environment, then attempting to install it into a new environment with `editable_mode=compat` does not work.

### uv version

```
uv 0.4.3 (Homebrew 2024-09-02)
```

### Repro

Create a toy package `foo`:

```
mkdir foo
mkdir foo/foo
touch foo/__init__.py

cat >foo/pyproject.toml <<EOF
[project]
name = "foo"
version = "0.1.0"
EOF
```

Create a venv and install foo as an editable:

```
uv venv .venv-1
uv pip install --python=.venv-1/bin/python -e foo
cat .venv-1/lib/python3.12/site-packages/__editable__.foo-0.1.0.pth

# => import __editable___foo_0_1_0_finder; __editable___foo_0_1_0_finder.install()
```

Notice the content of the above pth is an import hook. That's expected.

But now create another venv and install foo as an editable using `--config-setting editable_mode=compat`. This should cause the content of the `pth` file to be a straight path instead of an import hook:

```
uv venv .venv-2
uv pip install --python=.venv-2/bin/python --config-setting editable_mode=compat -e foo
cat .venv-2/lib/python3.12/site-packages/__editable__.foo-0.1.0.pth

# => import __editable___foo_0_1_0_finder; __editable___foo_0_1_0_finder.install()
```

It looks like `editable_mode=compat` was ignored because we still have an import hook. Now repeat the above but with `--no-cache`:

```
uv venv .venv-3
uv pip install --python=.venv-3/bin/python --config-setting editable_mode=compat --no-cache -e foo
cat .venv-3/lib/python3.12/site-packages/__editable__.foo-0.1.0.pth

# => /path/to/foo
```

Now it works. So it looks like `--config-setting editable_mode=compat` is being passed through dependent on whether we are using the uv cache.

---

_Comment by @charliermarsh on 2024-09-04 23:49_

Yeah, I think these definitely aren't included in the cache key.

---

_Label `bug` added by @charliermarsh on 2024-09-04 23:49_

---

_Label `cache` added by @charliermarsh on 2024-09-04 23:49_

---

_Comment by @charliermarsh on 2024-09-05 20:00_

I think using `--reinstall` would also work here (or `--reinstall-package foo`).

---

_Comment by @smackesey on 2024-09-06 14:11_

>I think using --reinstall would also work here (or --reinstall-package foo).

Thanks, this is working and has enabled a workaround



---

_Comment by @charliermarsh on 2024-09-06 15:02_

👍 I’m still looking into fixing it as part of some other caching improvements.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-06 21:16_

---

_Closed by @charliermarsh on 2024-09-10 01:49_

---

_Closed by @charliermarsh on 2024-09-10 01:49_

---
