```yaml
number: 6233
title: TCH001 false positive in Ruff 0.0.281
type: issue
state: closed
author: flying-sheep
labels: []
assignees: []
created_at: 2023-08-01T08:48:10Z
updated_at: 2023-08-01T15:05:08Z
url: https://github.com/astral-sh/ruff/issues/6233
synced_at: 2026-01-12T15:54:45Z
```

# TCH001 false positive in Ruff 0.0.281

---

_@flying-sheep_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
When updating to Ruff 0.0.281, the following started falsely throwing TCH001:

[playground 1](https://play.ruff.rs/#N4KABGBEDOCmA2sDGAXSAuMBtSBBAMvpALoA04UKAhgE4DmsKAtAG6w3QCWA9gHYZQADgE8AzAEZxkEAF8AJAshKUsaCgB0S6SABmNbgFswAfWM6ArinM1YpsJwODuNFGCq9e3aih69oIXX0jFGFBTl46e0dnVwAVAE0ABQBRYwBhAAlktIBpAEkAOQBxAL1DMHVjaHNBJxcoutdq2pjYABNjGmMDKhQaTgAPYyR4Kmg4fxBOHTAElPSs3MKi9AoIMqNoJE4RBpiwaEFaOACQUyp4eDsAXmw1ym4kaBpIcggIGBrG9s7u3v6hiMxhNXiBiKc2rAZihHs8ABTcABGACtMIdjrB1Icen1BgBKMBMAB8ByOHExTy6OIBq3eUGUqg0WnuNisNF4YCRyPUMOG8LxICAA) | [playground 2](https://play.ruff.rs/#N4KABGBEDOCmA2sDGAXSAuMBtSBBAMvpALoA04UKAhgE4DmsKAtAG6w3QCWA9gHYZQADgE8AzAEZxkEAF8AJAshKUsaCgB0S6SABmNbgFswAfWM6ArinM1YpsJwODuNFGCq9e3aih69oIXX0jQSoUAAt4TgAje0dnVwAFULDAwzAUYUFOXjpYpxcwABUATQSAUWMAYQAJMsqAaQBJADkAcQDOHSLSipq6prb0Cgg9NPdeABNQqjz4sFwPABFp4bBR4Pcp6FmC5eoAMRoqA1gA0yp4eDsAXmxIUzYOX1NIUigADxIA2oAlMrBbklwgAKUw6TiIUwASnUIRsvBQARQNGEQwgIyCYDgVkEKG43Hg0GM0CQRgc+VcDBQxkeXD4AXRJhp7DpvBuYCpzKefGBahowN+ZVhtFgCOF8JQUKhIFg7yQsFxYGBjTiLjKNH0NDe+HxAGtzIJ1ZqoWj0cjUatGeswOouaydq4HiznsZVrL5YqVRSjc43NtYKbGRADNBcrdIJVxl57LwdOwwLTfOpNJb0UdOHAwF74j7+SG6FC1pjTgEJrAuu9gVQJpgFrw9lRC0wAHxgBuHY4B1ZaFRqTRKVY2Kw0XhuCbqPFmI4nYHSoA)

```py
"""test."""

from __future__ import annotations

from typing import TYPE_CHECKING

from ._support import supported_r_matrix_classes

if TYPE_CHECKING:
    from scipy import sparse


__all__ = [
    "tocsr",
    "supported_r_matrix_classes",
]


def tocsr(obj: sparse.spmatrix) -> sparse.csr_matrix:
    """test."""
    return obj.to_csr()
```

```
    from ._support import supported_r_matrix_classes
                          ^^^^^^^^^^^^^^^^^^^^^^^^^^
TCH001: Move application import `._support.supported_r_matrix_classes` into a type-checking block
```

---

_Comment by @mkniewallner on 2023-08-01 10:54_

Probably the same underlying issue as https://github.com/astral-sh/ruff/issues/6207, which should be fixed by https://github.com/astral-sh/ruff/pull/6208.

---

_Comment by @flying-sheep on 2023-08-01 11:10_

indeed, duplicate of #6207

---

_Closed by @flying-sheep on 2023-08-01 11:10_

---

_Comment by @charliermarsh on 2023-08-01 15:04_

This should be fixed in v0.0.282, which is out now.

---

_Comment by @charliermarsh on 2023-08-01 15:05_

I'm also going to add Poetry to our ecosystem CI checks, to prevent future regressions.

---
