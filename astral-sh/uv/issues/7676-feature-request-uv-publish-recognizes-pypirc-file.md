---
number: 7676
title: Feature Request - uv publish recognizes .pypirc file
type: issue
state: open
author: geozeke
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2024-09-24T21:44:49Z
updated_at: 2025-10-29T13:15:46Z
url: https://github.com/astral-sh/uv/issues/7676
synced_at: 2026-01-10T01:24:17Z
---

# Feature Request - uv publish recognizes .pypirc file

---

_Issue opened by @geozeke on 2024-09-24 21:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Thanks soooooooo much for the work on this amazing tool! I'm in the process of migrating the toolchain for all my Python projects to uv & ruff!

The recently-released `publish` command is a really welcome addition. When I run it, it's listed as "experimental", so I'm hoping there's still an opportunity for input on the feature. Definitely not an urgent feature need, but It would be great if `publish` recognized the `.pypirc` toml file, as specified in the [Python Packaging User Guide](https://packaging.python.org/en/latest/specifications/pypirc/). Namely:

```toml
[distutils]
index-servers =
    pypi
    testpypi
    private-repository

[pypi]
username = __token__
password = <PyPI token>

[testpypi]
username = __token__
password = <TestPyPI token>

[private-repository]
repository = <private-repository URL>
username = <private-repository username>
password = <private-repository password>
```

This would be like `twine`, but maybe add a new `-r, --repository` option that takes the shorthand name from the index servers listed in `.pypirc`. In that case, with a properly-configured `.pypirc` file like the one above in the user's home directory, publishing would be handled something like this:

To publish to `pypi` (the default), enter:

```shell
uv publish
```

To publish to `test.pypi`, enter:

```shell
uv publish -r testpypi
```

Private repos/others look like this:

```shell
uv publish -r private-repository
```



---

_Comment by @zanieb on 2024-09-24 22:03_

I guess we probably should since it's a standardized file though it seems a bit behind-the-times as there are several warnings suggesting alternative methods for authentication. It's also weird that it has a `[distutils]` section. We're generally very hesitant to read from other tool's configuration i.e. https://github.com/astral-sh/uv/issues/1404

---

_Comment by @geozeke on 2024-09-24 22:07_

Fair points. Definitely an only-if-it-makes-sense feature request 😊

---

_Comment by @zanieb on 2024-09-24 22:09_

I'm actually not sure this is a standard it might just be recommended by the PyPA? I'll follow up on that.

Added to the guide in https://github.com/pypa/packaging.python.org/pull/734 and it looks like twine just used distutils to parse it originally per https://github.com/pypa/twine/pull/15

There's an open pull request from 2017 that updates the guide to discourage use of pypirc? https://github.com/pypa/packaging.python.org/issues/297

---

_Comment by @geozeke on 2024-09-24 22:31_

Thanks for the link. Interesting conversation in the PR from 2017. Some talk of keyring support being a little "flaky" on linux [at that time], so maybe `.pypirc` use was kept around. Much has changed since then, so maybe its use is considered legacy at this point.


---

_Label `compatibility` added by @zanieb on 2024-09-24 22:54_

---

_Label `needs-decision` added by @zanieb on 2024-09-24 22:54_

---

_Comment by @raayu83 on 2024-09-25 07:09_

I think support for `.pypirc` would still be useful.
There are people who work in environments where `.pypirc` is set up automatically in the CI/CD environment and changing that to support new authentication methods would take some time and need to be prioritized first.
Therefore I believe supporting `.pypirc` would help increase adoption of uv.

---

_Comment by @bulletmark on 2024-09-25 10:42_

I created a wrapper for this: https://github.com/bulletmark/uv-publish.

---

_Referenced in [langflow-ai/langflow#3919](../../langflow-ai/langflow/pulls/3919.md) on 2024-09-26 23:51_

---

_Comment by @cthoyt on 2024-09-27 07:15_

Big agree on this! My workflow uses `twine upload --skip-existing dist/*` locally and relies on the `.pypirc` file. I'm very excited to replace this with `uv publish` if it doesn't require me having to reengineer my local environment.

The other benefit of the config file is it lets you keep track of multiple index’s credentials 

(yes, I know that everyone is moving over to using trusted publishing, but I'm not ready for that yet)

---

_Referenced in [astral-sh/uv#7963](../../astral-sh/uv/issues/7963.md) on 2024-10-07 09:12_

---

_Referenced in [astral-sh/uv#8352](../../astral-sh/uv/issues/8352.md) on 2024-10-19 07:58_

---

_Renamed from "Feature Request - vu publish recognizes .pypirc file" to "Feature Request - uv publish recognizes .pypirc file" by @konstin on 2024-11-03 17:44_

---

_Referenced in [astral-sh/uv#8806](../../astral-sh/uv/pulls/8806.md) on 2024-11-04 20:21_

---

_Comment by @spapanik on 2025-01-15 14:54_

My 2p: The .pypirc is extremely annoying that uses the distutils name, and the format is outdated, as [pypi] doesn't even accept a username anymore. But copying and pasting the token is not great, and also having tokens in env vars is not generally recommended (for example the Postgres documentation has this about the PGPASSWORD variable: PGPASSWORD behaves the same as the [password](https://www.postgresql.org/docs/17/libpq-connect.html#LIBPQ-CONNECT-PASSWORD) connection parameter. Use of this environment variable is not recommended for security reasons, as some operating systems allow non-root users to see process environment variables via ps; instead consider using a password file)

---

_Comment by @eriktaubeneck on 2025-03-25 19:16_

The hesitation to rely on the `.pypirc` is reasonable. In the meantime, would it be possible to allow these to be included in the `uv.toml` config file?

---

_Comment by @jhadida on 2025-04-09 03:42_

An equivalent of `.pypirc` would be very useful, I agree with @cthoyt.

It doesn't have to follow the exact same format, but IMO it should not be a `uv.toml` setting unless it could keep track of multiple indexes. Even then it would still feel clumsy because credentials for these different indexes would need to be stored somewhere else. 

The change of authentication for publishing over time is not an issue I think. The `username` and `password` properties will become obsolete over time, and they can simply be replaced by new properties like `token` or whatever else is needed. 

---

_Comment by @hemna on 2025-08-13 01:35_

Just ran into this problem trying to publish for the first time with uv. 

---

_Comment by @frobnitzem on 2025-10-29 13:15_

I worked around this with a custom make rule:

```
publish: build ## Publish to pypi
    @echo "🚀 Publishing project"
    $(eval user := $(shell sed -ne 's/username *= *//p' $(HOME)/.pypirc))
    $(eval pass := $(shell sed -ne 's/password *= *//p' $(HOME)/.pypirc))
    uv run --isolated --no-project --with dist/*.whl tests/smoke_test.py
    uv run --isolated --no-project --with dist/*.tar.gz tests/smoke_test.py
    uv publish -u $(user) -p $(pass)

.PHONY: build
build: clean-build ## Build wheel file
    @echo "🚀 Creating wheel file"
    uv build

.PHONY: clean-build
clean-build: ## Clean build artifacts
    @echo "🚀 Removing build artifacts"
    uv run python -c "import shutil; import os; shutil.rmtree('dist') if os.path.exists('dist') else None"
```

---
