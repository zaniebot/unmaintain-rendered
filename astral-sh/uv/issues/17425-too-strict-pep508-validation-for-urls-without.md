```yaml
number: 17425
title: Too strict PEP508 validation for URLs without file extension
type: issue
state: open
author: ppalucha
labels:
  - bug
assignees: []
created_at: 2026-01-12T20:30:56Z
updated_at: 2026-01-12T22:38:32Z
url: https://github.com/astral-sh/uv/issues/17425
synced_at: 2026-01-12T23:24:19Z
```

# Too strict PEP508 validation for URLs without file extension

---

_@ppalucha_

### Summary

Hi, package pyro-api contains following URL in test dependencies (extra_requires):

```
pyro-ppl@https://api.github.com/repos/pyro-ppl/pyro/tarball/dev
```

See https://github.com/pyro-ppl/pyro-api/blob/0.1.2/setup.py

I know the comment in setup.py says to remove these lines before uploading to PyPI, however I don't think this comment is correct. The above URL is fine, can be downloaded (creates a tar.gz file) and satisfies the PEP508 grammar (I checked it using code like at https://github.com/astral-sh/uv/issues/8326#issuecomment-2423123210_)

Note: I don't want to discuss if it makes sense to provide a non-pinned, no-hash, mutable URL as a requirement.

After building a wheel pyro-abi 0.1.2 directly from the source code (without commenting out the test extras), I can install the resulting wheel using 'pip'. 

However, installation with uv fails:

```
× Failed to read `pyro-api @ file:///tmp/pyro_api-0.1.2-py3-none-any.whl`
├─▶ Couldn't parse metadata of pyro_api-0.1.2-py3-none-any.whl from pyro-api @ file:///work/lbuild-out/pyro_api-0.1.2-py3-none-any.whl
 ╰─▶ Expected direct URL (`https://api.github.com/repos/pyro-ppl/pyro/tarball/dev`) to end in a supported file extension: `.whl`, `.tar.gz`, `.zip`, `.tar.bz2`,
 `.tar.lz`, `.tar.lzma`, `.tar.xz`, `.tar.zst`, `.tar`, `.tbz`, `.tgz`, `.tlz`, or `.txz`
 pyro-ppl@ https://api.github.com/repos/pyro-ppl/pyro/tarball/dev ; extra == "test"
```

I believe the requirement of the file extension is too strict and not justified by PEP508 (or PEP 440).
My expectation is that `pyro-ppl@https://api.github.com/repos/pyro-ppl/pyro/tarball/dev` should be accepted as a proper requirement URL.

### Platform

Linux 6.10.14-linuxkit aarch64 GNU/Linux

### Version

uv 0.9.22 (82a6a66b8 2026-01-06)

### Python version

3.13.8

---

_Label `bug` added by @ppalucha on 2026-01-12 20:30_

---

_Comment by @ppalucha on 2026-01-12 22:38_

I proposed a PR that fixes it, assuming we agree this is the right direction: https://github.com/astral-sh/uv/pull/17426

---
