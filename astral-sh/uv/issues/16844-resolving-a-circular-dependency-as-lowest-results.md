---
number: 16844
title: "resolving a circular dependency as `lowest` results in an older version of the project being fetched"
type: issue
state: closed
author: neutrinoceros
labels:
  - question
assignees: []
created_at: 2025-11-25T14:13:42Z
updated_at: 2025-11-26T16:15:14Z
url: https://github.com/astral-sh/uv/issues/16844
synced_at: 2026-01-10T01:26:10Z
---

# resolving a circular dependency as `lowest` results in an older version of the project being fetched

---

_Issue opened by @neutrinoceros on 2025-11-25 14:13_

### Summary

When experimenting with `--resolution=lowest` for astropy's complete dependency graph, I hit a diabolical edge case, where `astropy` has an optional dependency on `asdf-astropy>=0.3`, which *itself* requires `astropy>=5.0.4` (astropy's current version number is in the `8.0.0dev+...`). Apparently, uv fails to recognize that `astropy` is the package being resolved for, and it attempts to build v5.0.4 from source. This would already be a waste of time even if it worked, but it doesn't, making it harder to ignore.

[example logs](https://github.com/astropy/astropy/actions/runs/19665128196/job/56320090314?pr=18955)
relevant excerpt :
```
Building astropy==5.0.4
  × Failed to download and build `astropy==5.0.4`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `setuptools`, `setuptools-scm>=6.2`,
  │   `wheel`, `cython==0.29.22`, `markupsafe==2.0.1`, `jinja2==3.0.3`,
  │   `oldest-supported-numpy`, `extension-helpers`
  ╰─▶ Because markupsafe==2.0.1 has no usable wheels and you require
      markupsafe==2.0.1, we can conclude that your requirements are
      unsatisfiable.

      hint: Wheels are required for `markupsafe` because building from
      source is disabled for `markupsafe` (i.e., with `--no-build-package
      markupsafe`)
  help: `astropy` (v5.0.4) was included because `asdf-astropy` (v0.3.0)
        depends on `astropy`
```

To add insult to injury, astropy's version number is dynamic...

I suppose at this point, the resolver cannot really be expected to do better than *taking a bet* and just assume that the version of astropy being built will satisfy the requirements. I think this would be fine as long as the assumption is checked after we're done building astropy.

I do note that there's a known workaround we can use by making astropy requiring a more recent version of asdf-astropy, but I think that's ultimately a hack, and only works because we end up downloading older versions of astropy that have matching wheels, so I'd be interested to see if there's a chance to resolve the root issue in uv instead.

Unfortunately I couldn't reproduce this locally on macOS (amr64), so I don't have the ability to reduce my reproducer yet.
Possibly related, but I don't think that's quite a duplicate: https://github.com/astral-sh/uv/issues/16372

### Platform

Seen on ubuntu ubuntu-24.04

### Version

uv==0.9.11

### Python version

3.11

---

_Label `bug` added by @neutrinoceros on 2025-11-25 14:13_

---

_Referenced in [astropy/astropy#18992](../../astropy/astropy/issues/18992.md) on 2025-11-25 14:16_

---

_Comment by @konstin on 2025-11-26 16:10_

The CI workflow you linked uses `uv pip install`, without `astropy` in the list of dependencies. It seems correct that `uv pip` doesn't pick up the current pyproject.toml if `.` isn't requested.

---

_Renamed from "BUG: resolving a circular dependency as `lowest` results in an older version of the project being fetched" to "resolving a circular dependency as `lowest` results in an older version of the project being fetched" by @konstin on 2025-11-26 16:11_

---

_Label `bug` removed by @konstin on 2025-11-26 16:11_

---

_Label `question` added by @konstin on 2025-11-26 16:11_

---

_Comment by @neutrinoceros on 2025-11-26 16:15_

oh, excellent point, I missed that. Well, that's a problem for tox and tox-uv then. Thank you !

---

_Closed by @neutrinoceros on 2025-11-26 16:15_

---
