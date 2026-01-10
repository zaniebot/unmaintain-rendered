```yaml
number: 266
title: "`possibly-unresolved-reference` with conditional assignment and guarded usage"
type: issue
state: closed
author: my1e5
labels: []
assignees: []
created_at: 2025-05-08T10:04:14Z
updated_at: 2025-05-08T10:09:01Z
url: https://github.com/astral-sh/ty/issues/266
synced_at: 2026-01-10T02:34:09Z
```

# `possibly-unresolved-reference` with conditional assignment and guarded usage

---

_Issue opened by @my1e5 on 2025-05-08 10:04_

### Summary

I’m seeing a `possibly-unresolved-reference` warning for a common pattern I use with `tqdm` progress bars. The warning may be completely valid, but I wanted to mention it since mypy doesn’t flag it.

https://play.ty.dev/ec8d3619-1214-4d3c-aba4-f7578d1502a7
```py
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "tqdm",
# ]
# ///

import time

from tqdm import tqdm


def run(n: int, *, show_progress: bool = True) -> None:
    """Run a loop with an optional progress bar."""

    def callback() -> None:
        time.sleep(0.1)
        if show_progress:
            pbar.update()

    if show_progress:
        pbar = tqdm(total=n)

    for _ in range(n):
        callback()

    if show_progress:
        pbar.close()


if __name__ == "__main__":
    run(10, show_progress=True)
```
### ty
```
$ uvx ty check foo.py
warning: lint:possibly-unresolved-reference: Name `pbar` used when possibly not defined
  --> foo.py:19:13
   |
17 |         time.sleep(0.1)
18 |         if show_progress:
19 |             pbar.update()
   |             ^^^^
20 |
21 |     if show_progress:
   |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `pbar` used when possibly not defined
  --> foo.py:28:9
   |
27 |     if show_progress:
28 |         pbar.close()
   |         ^^^^
   |
info: `lint:possibly-unresolved-reference` is enabled by default

Found 2 diagnostics
```
### mypy
```
$ uvx --with types-tqdm mypy --strict foo.py
Installed 6 packages in 1.42s
Success: no issues found in 1 source file
```

### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Comment by @AlexWaygood on 2025-05-08 10:07_

Mypy does actually emit an error on the exact same line if you pass `--enable-error-code=possibly-undefined` to mypy: https://mypy-play.net/?mypy=latest&python=3.12&enable-error-code=possibly-undefined&gist=2c55a51cc7c25926197894f7d9a21d14 (mypy's version of this check is not enabled by default).

And pyright, like ty, displays the error by default on the same line: https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=MQAg9BIM4MYE4EsAOAXAUKOBTAjgVwWygFokBPFACwHsA7EAXhACIA%2BBgZgDoBGD5jCAAmWJFlojaMBFiiMQAbUEgVLFDiEBbZgBpBAXUEQwaNAk1JqcFCBTmspgGZxqm2xrfnL191tNoRRxA4PFoACloALhAEWhQdEAAqBKgaAHcAfSQXAHMiKGiAI2pqABt5ABUQrABKEGJWEAA5OixItFUWZmYAJVCQAEMQUpKkEDSEKkH6alQEOgHy7Oo82TlCgbguboEO1UCQGEXSjZgAazC6hubW9s7Ou00sLihSrFEwgAZeGr37mKCqWomWWqygBT%2B-1USA2WzwSCEAxQWEupk6CEB6SyuXydyhMM28nUWjCKGoKEWDFov0hjisIAyMXocAGtByKOpeP%2BR1KJwG51RkIx0CxoNxkM6BK2MBGUBRNLMQQyGVoAyeysYTGYys0A1iyuYXOCoTCPE%2BKVFOLWDCqeFqaCAA

---

_Closed by @my1e5 on 2025-05-08 10:09_

---
