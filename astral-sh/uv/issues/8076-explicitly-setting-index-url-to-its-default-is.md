```yaml
number: 8076
title: "Explicitly setting `index-url` to its default is not equivalent to having it undefined when syncing lockfiles with unreachable URLs"
type: issue
state: closed
author: mgab
labels:
  - question
assignees: []
created_at: 2024-10-10T08:56:34Z
updated_at: 2024-10-29T14:05:08Z
url: https://github.com/astral-sh/uv/issues/8076
synced_at: 2026-01-12T15:59:19Z
```

# Explicitly setting `index-url` to its default is not equivalent to having it undefined when syncing lockfiles with unreachable URLs

---

_@mgab_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Problem description

When trying to sync from a lockfile that contains unreachable URLs, if there's any index url explicitly defined anywhere it correctly tries to get the packages from that index. Explicitly setting the default value `index-url = "https://pypi.org/simple"` works, too. However, if no index url is explicitly configured anywhere the sync command fails with an error reporting that the URLs in the lockfile are not reachable.

I would assume that explicitly setting the default value should be equivalent to not setting any value. If this is not the desired behavior I guess it should be mentioned in the documentation (e.g. [here](https://docs.astral.sh/uv/reference/settings/#index-url)).

## Steps to reproduce

You need to have no index URL configured anywhere (envvars, `~/.config/uv/uv.toml`...)

Then, on an empty folder
```bash
uv init
uv add art
rm -r .venv
uv clean
sed -e 's/pypi.org/unresponding_url.org/' -e 's/files.pythonhosted.org/unresponding_url.org/g' -i '' uv.lock
uv sync --verbose
```
This command fails.

<details>

<summary>`uv sync --verbose` output</summary>

```
DEBUG uv 0.4.19 (Homebrew 2024-10-07)
DEBUG Found project root: `/Users/m.gabalda/Code/uv_test/test_short`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/m.gabalda/Code/uv_test/test_short/.python-version`
DEBUG Searching for Python 3.12 in managed installations or system path
DEBUG Searching for managed installations at `/Users/m.gabalda/.local/share/uv/python`
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/m.gabalda/.pyenv/shims/python` (search path)
Using CPython 3.12.5 interpreter at: /Users/m.gabalda/.pyenv/versions/3.12.5/bin/python
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-short @ file:///Users/m.gabalda/Code/uv_test/test_short
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 2 packages in 0.61ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: art==6.3
DEBUG No cache entry for: https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl
DEBUG Transient request failure for https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl, retrying: error sending request for url (https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl, retrying: error sending request for url (https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl, retrying: error sending request for url (https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl, retrying: error sending request for url (https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: art==6.3
  Caused by: Could not connect, are you offline?
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://unresponding_url.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
```

</details>


On the other hand, if before one defines explicitly the default value for `index-url` in `pyproject.toml` (or in `~/.config/uv/uv.toml`) it succeeds in searching for the dependencies in pypi:

```
echo -e '\n[tool.uv]\nindex-url = "https://pypi.org/simple"' >> pyproject.toml
uv sync --verbose
```

<details>

<summary>`uv sync --verbose` output</summary>

```
DEBUG uv 0.4.19 (Homebrew 2024-10-07)
DEBUG Found project root: `/Users/m.gabalda/Code/uv_test/test_short`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/m.gabalda/Code/uv_test/test_short/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-short @ file:///Users/m.gabalda/Code/uv_test/test_short
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to missing remote index: `art` `6.3` from `https://unresponding_url.org/simple`
DEBUG Starting clean resolution
DEBUG Found static `pyproject.toml` for: test-short @ file:///Users/m.gabalda/Code/uv_test/test_short
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: test-short*
DEBUG Searching for a compatible version of test-short @ file:///Users/m.gabalda/Code/uv_test/test_short (*)
DEBUG Adding transitive dependency for test-short==0.1.0: art>=6.3
DEBUG No cache entry for: https://pypi.org/simple/art/
WARN Skipping file for art: art-1.0-py3.4.egg
DEBUG Searching for a compatible version of art (>=6.3)
DEBUG Selecting: art==6.3 [preference] (art-6.3-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl.metadata
DEBUG Tried 2 versions: art 1, test-short 1
DEBUG Split universal resolution took 19.142s
Resolved 2 packages in 19.14s
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: art==6.3
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b0/2e/2ad0cca2345e5bff23b979602917f39e844951dafe3b541935cc2850ee36/art-6.3-py3-none-any.whl
Prepared 1 package in 212ms
DEBUG Extracting file name=PackageName("art")
DEBUG Cloning /Users/m.gabalda/.cache/uv/archive-v0/NOcfpTT4LlhTH0K29ewYy/art to /Users/m.gabalda/Code/uv_test/test_short/.venv/lib/python3.12/site-packages/art
DEBUG Cloning /Users/m.gabalda/.cache/uv/archive-v0/NOcfpTT4LlhTH0K29ewYy/art-6.3.dist-info to /Users/m.gabalda/Code/uv_test/test_short/.venv/lib/python3.12/site-packages/art-6.3.dist-info
DEBUG Extracted 2 files name=PackageName("art")
DEBUG Writing entrypoints name=PackageName("art")
DEBUG No data name=PackageName("art")
DEBUG Writing extra metadata name=PackageName("art")
DEBUG Writing record name=PackageName("art")
Installed 1 package in 5ms
 + art==6.3
```

</details>

## Versions

Platform: macOS 14.6.1
uv version: uv 0.4.19 (Homebrew 2024-10-07)
 

---

_Comment by @charliermarsh on 2024-10-10 09:06_

Is there a reproduction here that doesn't involve manually modifying the `uv.lock`? (You're definitely on your own if you're modifying the `uv.lock` out-of-band; any guarantees go out the window.)

---

_Label `needs-mre` added by @charliermarsh on 2024-10-10 09:06_

---

_Comment by @mgab on 2024-10-10 12:11_

My use case is sharing a project, that doesn't have any explicit `index-url` or `extra-index-url` configured, between two setups:
1. one that has some `index-url` configured at the user level (e.g. a proxy)
2. one that relies on the default `index-url` configuration without setting it explicitly (and for which the proxy the other setup uses is not reachable).

AFAIU, if all the exact same packages are found in both index urls that setup should work. However, when (1) tries to sync using a lockfile generated by (2) it fails because it cannot reach the URLs listed in the lockfile instead of trying to get the files from the default index.

So, in short, the problem seems to be about dealing with unreachable URLs in the lockfile while not having an explicitly defined `index-url` value. Manually modifying the URLs in the lockfile to a non-responding hostname was a way to provide a minimal way to reproduce the issue, but I'm open to other ideas.



---

_Comment by @mgab on 2024-10-11 07:50_

You can reproduce the issue using this [uv.lock.txt](https://github.com/user-attachments/files/17339212/uv.lock.txt) and [pyproject.toml.txt](https://github.com/user-attachments/files/17339211/pyproject.toml.txt) files (removing the `.txt` suffix that githubs requires to upload them)

The situation is essentially equivalent (URLs in the lockfile point at a domain that does not respond), but this time generated using a local mirror of pypi.

<details>

<summary>Steps to generate those files</summary>

----

On one terminal, start a toy local pypi mirror. It can be populated with `python-pypi-mirror` and served with the standard library module `http.server`, just following the [usage example](https://github.com/montag451/pypi-mirror) in `python-pypi-mirror`. I'll use `ast` as a dependency example:

```bash
uvx --from python-pypi-mirror pypi-mirror download -d downloads ast
uvx --from python-pypi-mirror pypi-mirror create -d downloads -m simple
python3 -m http.server
```

Then on another terminal, create a new project and add `ast` as a project dependency using the local mirror as index url 

```
uv init
UV_EXTRA_INDEX_URL=http://127.0.0.1:8000/simple uv add art --no-cache
```

And then kill the http server on the first terminal.

----

</details>

As in the other example, the issue is the same: trying to sync using that lockfile that has non-working URLs behaves differently depending on whether there's a index URL specifically defined or you rely on the default value.

Assuming there are no extra-index-url or index-url configured anywhere for the user, now this will fail

```
uv sync --reinstall
```

while this, apparently equivalent according to the docs, will succeed

```
UV_INDEX_URL=https://pypi.org/simple uv sync --reinstall
```

---

_Comment by @charliermarsh on 2024-10-11 09:36_

Sorry yes -- this is intentional. If you don't provide _any_ indexes explicitly, then we assume that we can accept the indexes that are already present in the lockfile. If you provide _at least_ one index, then we reject the lockfile if it references any indexes that weren't provided by the user. By setting `UV_INDEX_URL` explicitly, that is semantically different than accepting the empty defaults.


---

_Label `needs-mre` removed by @charliermarsh on 2024-10-11 09:37_

---

_Label `question` added by @charliermarsh on 2024-10-11 09:37_

---

_Comment by @mgab on 2024-10-11 11:20_

Oh, I see. Even if I see the desired behavior makes sense, it seems a behavior a bit confusing for a default value. In general I'd expect that defaults imply that not providing any value is equivalent to providing the default one.

Do you think it'd make sense to clarify it in the [documentation](https://docs.astral.sh/uv/reference/settings/#index-url)? I couldn't find any reference to it.

In fact, probably then `https://pypi.org/simple` is not really a default value for the `index-url` setting, but rather a fallback package index used whenever there are no other relevant URL explicitly defined (either as index urls or in the lockfile)

---

_Comment by @mgab on 2024-10-29 14:05_

Release [0.4.23](https://github.com/astral-sh/uv/releases/tag/0.4.23) revamped the structure of the indexes and clarified the documentation! I'm closing the issue as I think it's all good now, thanks! 🙏

---

_Closed by @mgab on 2024-10-29 14:05_

---
