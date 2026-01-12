```yaml
number: 13614
title: "`uv lock --check` fails even though `uv lock` gives no updates"
type: issue
state: closed
author: mikeweltevrede
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-05-23T07:21:35Z
updated_at: 2025-05-26T18:17:20Z
url: https://github.com/astral-sh/uv/issues/13614
synced_at: 2026-01-12T16:01:33Z
```

# `uv lock --check` fails even though `uv lock` gives no updates

---

_@mikeweltevrede_

### Summary

I hope you can help me with this :)

Running `uv lock --check` seems to fail even though when I run `uv lock` there are no updates:

```
(my-repo) my-device my-repo % uv self version
uv 0.7.3 (3c413f74b 2025-05-07)
(my-repo) my-device my-repo % uv lock --check
Resolved 345 packages in 5.88s
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
(my-repo) my-device my-repo % uv lock
Resolved 345 packages in 9.68s
(my-repo) my-device my-repo % uv lock --check
Resolved 345 packages in 5.05s
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

The pyproject.toml looks something like this (not shared entirely for confidentiality):

```
[project]
requires-python = "<4.0,>=3.9"
dependencies = [
    "typing-extensions>=4.6.0; python_version < '3.13'",
]

[dependency-groups]
local = [
    "typing-extensions==4.9.0",
]
dev = [
    "numba==0.55.1; python_version == '3.9'",
    "numba==0.56.4; python_version == '3.10'",
    "numba==0.57.1; python_version == '3.11'",
    "numba; python_version > '3.11'",
    "numpy==1.21.5; python_version == '3.9'",
    "numpy==1.23.5; python_version == '3.10' or python_version == '3.11'",
    "numpy; python_version > '3.11'",
]
lint = [
    "pre-commit==4.2.0",
    "pre-commit-hooks==5.0.0",
    "ruff==0.11.7",
]
```

This is the `uv.toml` file:

```
prerelease = "allow"
python-downloads = "never"
python-preference = "only-system"
link-mode = "symlink"

[[index]]
name = "my-corporate-proxy"
url = "https://my-corporate-proxy.com"
default = true
verify_ssl = true

[pip]
strict = true
verify-hashes = true
no-build-isolation-package = ["numba"]
```

The venv is created with the following make command (locally, we use `uv venv --python 3.10` and on the CI the Python version is selected on the build agent so `uv venv` is used):

```
# `export CC` reason: https://stackoverflow.com/questions/79137098/macos-python-3-9-no-module-named-distutils-msvccompiler
.PHONY: install-project
install-project:
	@echo "🚀 Creating virtual environment using uv"
	@uv self version
	@uv venv
	@echo "🚀 Installing numba without build isolation"
	@make install-root
	@export CC=/usr/bin/clang && export CXX=/usr/bin/clang++
	@uv pip install setuptools numpy && uv pip install numba
	@echo "🚀 Installing project (with all dependencies)"
	@uv sync --all-groups --all-extras
	@uv run pre-commit install
```

What I already checked:
- Deleting the lock file and recreating it
- The Github issues. No similar ones come up.
- Look at the docs, e.g. [uv lock](https://docs.astral.sh/uv/reference/cli/#uv-lock), [Locking and syncing](https://docs.astral.sh/uv/concepts/projects/sync/#locking-and-syncing), [Troubleshooting build failures](https://docs.astral.sh/uv/reference/troubleshooting/build-failures/#troubleshooting-build-failures), and [Locking environments](https://docs.astral.sh/uv/pip/compile/#locking-environments)

### Platform

Darwin 24.5.0 arm64 (macOS 15.5 Sequoia)

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

### Python version

Python 3.9.22, 3.10.17, 3.11.12 (all on Azure DevOps CI), Python 3.10.5 (Locally)

---

_Label `bug` added by @mikeweltevrede on 2025-05-23 07:21_

---

_Renamed from "Lock file" to "`uv lock --check` fails even though `uv lock` gives no updates" by @mikeweltevrede on 2025-05-23 08:06_

---

_Comment by @mikeweltevrede on 2025-05-23 08:13_

PS: One thing that would already be great, if possible, is if the error message would mention why it needs to be updated. For example, is it a specific library, Python version, etcetera.

---

_Comment by @charliermarsh on 2025-05-23 11:20_

Unfortunately I can't reproduce this as-is. Are you able to put together a minimal example that we can reproduce? Something that we can `git clone` and a command we can run? You can use `https://public:heron@pypi-proxy.fly.dev/basic-auth/simple/` instead of your own index if you need an alternative.

---

_Label `needs-mre` added by @charliermarsh on 2025-05-23 11:21_

---

_Comment by @konstin on 2025-05-23 11:53_

Can you share verbose logs? Those should contain the reason for the lockfile invalidation.

---

_Comment by @mikeweltevrede on 2025-05-23 12:09_

> Can you share verbose logs? Those should contain the reason for the lockfile invalidation.

@konstin Thanks, I did not know of that flag. This is the bottom of the verbose logs. There is not much in the rest of the logs. It does not tell me anything, is there anything that draws your attention?

![Image](https://github.com/user-attachments/assets/5a0fd9b5-4993-4360-8065-d2fa7d8578a9)

PS: Running `RUST_LOG=error uv lock --check --verbose` to decrease the immense amount of `DEBUG` logs yields no output (no ERROR logs). Using `RUST_LOG=info` yields some results that don't tell me anything either (`"INFO add_decision: Id::<PubGrubPackage>(340) @ 1.17.1 without checking dependencies"` for multiple versions)

---

_Comment by @mikeweltevrede on 2025-05-23 12:10_

> Unfortunately I can't reproduce this as-is. Are you able to put together a minimal example that we can reproduce? Something that we can `git clone` and a command we can run? You can use `https://public:heron@pypi-proxy.fly.dev/basic-auth/simple/` instead of your own index if you need an alternative.

Thanks @charliermarsh I will look into whether I can make an MRE

---

_Comment by @konstin on 2025-05-23 12:42_

Can you share the verbose logs for `uv lock --check`, either `uv lock --check -v` or `uv lock --check -vv`, potentially as a GitHub gist?

---

_Comment by @mikeweltevrede on 2025-05-23 12:52_

@konstin here is a gist for the `-v` log: https://gist.github.com/mikeweltevrede/4da89b4f35393fe415689cc092f26c64. The `-vv` log was too big and timed out the gist upload. Thanks

---

_Comment by @konstin on 2025-05-23 12:57_

Thanks!

We're missing a normalization somewhere, requested is `marker: false`, we're seeing `marker: python_full_version < '0'` (which is another way to write false)

---

_Assigned to @Gankra by @konstin on 2025-05-23 12:59_

---

_Comment by @mikeweltevrede on 2025-05-23 14:54_

Thanks @konstin. Good to know that I will be out of office until June 5 so my delays might not be that quick. I appreciate your timely responses and hope we can find a solution soon. Please let me know in case I need to check or test something.

---

_Comment by @Gankra on 2025-05-23 14:55_

With those logs I have a minimal reproducer, so we should be able to fix it in the next release

---

_Closed by @Gankra on 2025-05-26 18:17_

---
