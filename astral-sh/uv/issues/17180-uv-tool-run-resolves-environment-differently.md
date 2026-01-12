```yaml
number: 17180
title: "`uv tool run` resolves environment differently depending on type of `--with` argument"
type: issue
state: open
author: dlovell
labels:
  - bug
assignees: []
created_at: 2025-12-18T18:18:58Z
updated_at: 2025-12-19T14:29:43Z
url: https://github.com/astral-sh/uv/issues/17180
synced_at: 2026-01-12T16:02:45Z
```

# `uv tool run` resolves environment differently depending on type of `--with` argument

---

_@dlovell_

### Summary

`--with git+https://github.com/...` will use `pyproject.toml`'s `[uv.tool.sources]`
while
`--with $name.tgz` will not and only pull from pypi

to reproduce
```
url=https://github.com/xorq-labs/xorq-template-sklearn
commit=d432336c7a115b18b068b759d79360f2d2a3c81e
tgz=$commit.tar.gz
wget $url/archive/$tgz 2>/dev/null && echo success || echo failure
success
uv tool run --verbose --isolated --with $tgz python -c 'from xorq.caching import ParquetCache; print(ParquetCache)' >tgz.out 2>tgz.err && echo success || echo failure
failure
uv tool run --verbose --isolated --with "git+$url#$commit" python -c 'from xorq.caching import ParquetCache; print(ParquetCache)' >url.out 2>url.err && echo success || echo failure
success
```

some particularly relevant lines from stderr
```
grep -A7 'Attempting GitHub fast path.*xorq$' url.err
DEBUG Attempting GitHub fast path for: xorq @ git+https://github.com/xorq-labs/xorq
DEBUG Querying GitHub for commit at: https://api.github.com/repos/xorq-labs/xorq/commits/HEAD
DEBUG Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/xorq-labs/xorq/a40490520a6ad824d770f58559914a1cee253046/pyproject.toml
DEBUG Skipping GitHub fast path; `pyproject.toml` has sources: https://raw.githubusercontent.com/xorq-labs/xorq/a40490520a6ad824d770f58559914a1cee253046/pyproject.toml
DEBUG Fetching source distribution from Git: https://github.com/xorq-labs/xorq
DEBUG Acquired lock for `https://github.com/xorq-labs/xorq`
DEBUG Using existing Git source `https://github.com/xorq-labs/xorq`
DEBUG Released lock at `/home/dan/.cache/uv/git-v0/locks/5667d9fb21e3ee57`
```
and
```
grep 'Found fresh.*xorq' tgz.err | grep -v datafusion
DEBUG Found fresh response for: https://pypi.org/simple/xorq/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/9a/3462e33887935096419f40a6985b630ed865a65b473c3498fb360e5a90c8/xorq-0.3.4-py3-none-any.whl.metadata
```

### Platform

Linux 6.9.3-76060903-generic x86_64 GNU/Linux

### Version

uv 0.9.5

### Python version

Python 3.13.5

---

_Label `bug` added by @dlovell on 2025-12-18 18:18_

---

_Renamed from "`uv tool run` resolves environment depending on type of `--with` argument" to "`uv tool run` resolves environment differently depending on type of `--with` argument" by @dlovell on 2025-12-18 19:14_

---

_Comment by @nooscraft on 2025-12-19 14:17_

@zanieb should tar.gz files also check for and respect tool.uv.sources when present, just like git URLs do?

---

_Comment by @zanieb on 2025-12-19 14:29_

I didn't think we supported `.tar.gz` archives like this, we'll need to sort that out first :)

If we're treating it as a source tree, we should respect sources. If we're treating it as a source distribution, we shouldn't. In this case, it's not a spec-compliant source distribution but we may be treating it as one?

---
