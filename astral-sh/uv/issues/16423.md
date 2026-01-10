```yaml
number: 16423
title: Make importlib.metadata.packages_distributions work with uv build backend
type: issue
state: open
author: bunny-therapist
labels:
  - external
  - build-backend
assignees: []
created_at: 2025-10-23T15:56:06Z
updated_at: 2025-10-24T11:12:32Z
url: https://github.com/astral-sh/uv/issues/16423
synced_at: 2026-01-10T03:23:54Z
```

# Make importlib.metadata.packages_distributions work with uv build backend

---

_Issue opened by @bunny-therapist on 2025-10-23 15:56_

### Summary

- uv 0.9.5
- Ubuntu 24.04.3 LTS

Minimum reproducible example at https://github.com/bunny-therapist/uv-top-level-issue

- In that repo, simply run `uv run demonstrate.py`. It will raise an AssertionError.
- If you modify pyproject.toml so the build backend is instead setuptools, `uv run demonstrate.py` now works.

The function `importlib.metadata.packages_distributions` (or alternatively, the one in the backport `importlib_metadata` that I used instead to have more up-to-date code and proper versioning of the failing code) produces a mapping of distribution package names to lists of python packages/modules. 

The logic that it uses to find packages seems to not work for uv. I request that it does, since my understanding is that the function in question is the recommended way to find the distribution package a certain module belongs to.

I originally thought this a bug, but https://docs.python.org/3/library/importlib.metadata.html#package-distributions says _"Some editable installs, do not supply top-level names, and thus this function is not reliable with such installs."_ so I think this is more appropriately labeled a feature request.


---

_Label `enhancement` added by @bunny-therapist on 2025-10-23 15:56_

---

_Comment by @bunny-therapist on 2025-10-24 08:16_

I have verified that `importlib.metadata.packages_distributions` works for a built package. It does not work for an _editable_ install, nor with `uv sync --no-editable`.

---

_Label `enhancement` removed by @konstin on 2025-10-24 08:31_

---

_Label `external` added by @konstin on 2025-10-24 08:31_

---

_Comment by @konstin on 2025-10-24 08:32_

`importlib.metadata.packages_distributions` reads a file called `top_level.txt`. This file is only written by setuptools, afaik as a historical artifact from eggs, the format used before wheels. It is not part of any PEP, https://packaging.python.org/ or otherwise widely used. Other build backends don't seem to implement this non-standard file either, I've checked hatchling, pdm and flit. The documentation is misleading in that the function isn't expected to work with editable installs at all, except for setuptools. I've raised this with the `importlib[._]metadata` maintainers to update the documentation: https://github.com/python/importlib_metadata/issues/526

https://github.com/python/cpython/blob/161b3064efdafd2008378a88a8009897df1b58d2/Lib/importlib/metadata/__init__.py#L1122-L1123

https://github.com/python/importlib_metadata/blob/95ecf2aa573a68d4624506f801ca70992a337f41/importlib_metadata/__init__.py#L1133-L1134

---

_Label `build-backend` added by @konstin on 2025-10-24 08:33_

---

_Comment by @bunny-therapist on 2025-10-24 09:15_

Yeah, that was also my impression. I see `importlib[._]metadata` also has some `_top_level_inferred` logic but that also fails. It also looks like that logic was updated over the years to deal with different build-backends, and that it was never reliable due to the lack of a proper PEP. I was not aware of PEP 794, really good news! Thank you.

Would you be able to say somewhat confidently that `importlib[._]metadata.packages_distributions` in its current form can be expected to reliably work for all _non-editable_ installs of packages built with the uv build-backend?

---

_Comment by @konstin on 2025-10-24 09:26_

AFAIK `_top_level_inferred` reads `RECORD`, and non-editable installs generally have a correct `RECORD`.

---

_Comment by @bunny-therapist on 2025-10-24 09:32_

Ok great. Thank you. I see PEP 794 is accepted - I assume it requires implementation in uv? Is that planned? (I searched issues/PRs/branches but found nothing.)

---

_Comment by @konstin on 2025-10-24 11:12_

Yep, let's track this here: https://github.com/astral-sh/uv/issues/16435

---
