```yaml
number: 17065
title: "bug: `uv` is missing optional conditional dependencies(Almost got fired)"
type: issue
state: open
author: Aviksaikat
labels:
  - bug
assignees: []
created_at: 2025-12-10T11:00:46Z
updated_at: 2025-12-13T19:41:37Z
url: https://github.com/astral-sh/uv/issues/17065
synced_at: 2026-01-10T03:11:35Z
```

# bug: `uv` is missing optional conditional dependencies(Almost got fired)

---

_Issue opened by @Aviksaikat on 2025-12-10 11:00_

### Summary

## Summary

`uv` is not able to pick up conditional dependencies.

```sh
uv pip install -e "deps/web3-ethereum-defi[web3v7]"
```

 Doing the above ignores the dependencies defined like this


```toml
[...]

web3 = [
    {version = "^7.12.0", markers = "extra != 'web3v6'"},
    {version = "==6.14.0", markers = "extra == 'web3v6'"}
]

eth-tester = [
    {version = "*", markers = "extra != 'web3v6'"},
    {version = "*", markers = "extra == 'web3v6'"}
]

[tool.poetry.extras]

web3v7 = ["eth-tester"]
web3v6 = ["eth-tester"]
```

### Results

`web3` is not getting installed, but when we are using `pip` it's working just fine

### Platform

macOs arm

### Version

uv 0.9.17 (2b5d65e61 2025-12-09)

### Python version

Python 3.13.9

---

_Label `bug` added by @Aviksaikat on 2025-12-10 11:00_

---

_Comment by @zanieb on 2025-12-10 12:41_

We don't support reading Poetry-specific fields and neither does pip? so we won't read

```
[tool.poetry.extras]
web3v7 = ["eth-tester"]
web3v6 = ["eth-tester"]
```

Can you share a complete minimal reproducible example?

---

_Comment by @Aviksaikat on 2025-12-10 17:31_

1. Setup virtual env
```sh
uv venv .venv

# Activate the virtual environment
source .venv/bin/activate
```

2. Clone the repo

```sh
git clone https://github.com/tradingstrategy-ai/web3-ethereum-defi
cd web3-ethereum-defi
```

3. Install the dependencies 

```sh
uv pip install -e "web3-ethereum-defi[web3v7]"
```

4. Verify the installation

```sh
# web3py is not installed
uv pip list | grep web3
web3-ethereum-defi        0.35         /Users/avik/Work/tradingstrategy/freqtrade-gmx-demo-main/deps/web3-ethereum-defi
web3-google-hsm           0.1.0
```

### But doing the same thing using `pip` installs `web3py`

```sh
pip list | grep web3
web3                      7.14.0
web3-ethereum-defi        0.35            /Users/avik/Work/tradingstrategy/freqtrade-gmx-demo-main/deps/web3-ethereum-defi
web3-google-hsm           0.1.0
```

For some reason, `uv` is not able to read the conditional dependencies. As `poetry` follows the PEP standards for it should be compatible with `pip`.   


---

_Comment by @zanieb on 2025-12-10 18:03_

Looking at `web3-ethereum-defi`, it looks like it doesn't define a dependency on `web3` under that extra in the wheel `METADATA`? https://inspector.pypi.io/project/web3-ethereum-defi/0.35/packages/21/98/c394244e90cc4c1139678d0366ac61a16821d1f3b4f33600995963cafb4a/web3_ethereum_defi-0.35-py3-none-any.whl/web3_ethereum_defi-0.35.dist-info/METADATA#line.73

Perhaps I'm missing something?

---

_Comment by @zanieb on 2025-12-10 18:25_

I'm guessing `web3` is being included by a transitive dependency that uv excludes? It looks like `safe-eth-py` would do so.

---

_Comment by @Aviksaikat on 2025-12-10 18:55_

> Looking at `web3-ethereum-defi`, it looks like it doesn't define a dependency on `web3` under that extra in the wheel `METADATA`? https://inspector.pypi.io/project/web3-ethereum-defi/0.35/packages/21/98/c394244e90cc4c1139678d0366ac61a16821d1f3b4f33600995963cafb4a/web3_ethereum_defi-0.35-py3-none-any.whl/web3_ethereum_defi-0.35.dist-info/METADATA#line.73
> 
> Perhaps I'm missing something?

This is very odd. other `extra != "web3v6"` conditioned dependencies are evaluating just fine. hmm.....

---

_Comment by @zanieb on 2025-12-13 19:41_

I think we explicitly do not include the case where a dependency is "removed" when an extra is activated. While the markers you have are specification compliant, the pattern breaks assumptions of our resolution algorithm.

---
