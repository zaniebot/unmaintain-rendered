---
number: 1323
title: "support finding packages installed into Debian `dist-packages` directories"
type: issue
state: open
author: Hyask
labels:
  - imports
assignees: []
created_at: 2025-10-08T08:29:59Z
updated_at: 2026-01-08T23:28:41Z
url: https://github.com/astral-sh/ty/issues/1323
synced_at: 2026-01-10T01:48:23Z
---

# support finding packages installed into Debian `dist-packages` directories

---

_Issue opened by @Hyask on 2025-10-08 08:29_

### Summary

Reproducer is quite simple:
```
root@ubuntu-noble:~/test# cat - >test.py <<EOD
import apt

print(apt.apt_pkg.version_compare("1.2.3", "1.2.4"))
print(apt.apt_pkg.version_compare("1.2.4", "1.2.3"))
EOD
root@ubuntu-noble:~/test# python3 test.py
-1
1
root@ubuntu-noble:~/test# ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 fileserror[unresolved-import]: Cannot resolve imported module `apt`
 --> test.py:1:8
  |
1 | import apt
  |        ^^^
2 |
3 | print(apt.apt_pkg.version_compare("1.2.3", "1.2.4"))
  |
info: Searched in the following paths during module resolution:
info:   1. /root/test (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
root@ubuntu-noble:~/test#
```

Some packages (like `apt`) are usually best installed from the system, when writing system tools, so having a `pyproject.toml` file isn't really a viable option.
Just for the sake of it, I've tried adding a simple one with no dependencies specified, but as expected, it doesn't change the behavior.

### Version

ty 0.0.1-alpha.21 (01fca5525 2025-10-07)

---

_Label `imports` added by @sharkdp on 2025-10-08 09:44_

---

_Label `question` added by @sharkdp on 2025-10-08 09:44_

---

_Comment by @carljm on 2025-10-08 16:40_

Thanks for the report!

Assuming that you installed `ty` (using e.g. `uv install ty` or `pip install ty`) into the same system Python interpreter which has access to the `apt` package, then I think this is a duplicate of https://github.com/astral-sh/ty/issues/989.

If that is not the case, then I don't think that ty should make any assumption here; you should explicitly use the `--python` option (either via CLI or in a `ty.toml`) to specify the Python environment that should be used for checking this script.

---

_Closed by @carljm on 2025-10-08 16:40_

---

_Comment by @Hyask on 2025-10-08 21:57_

Kind of duplicate, but also not completely. My original test was with a `ty` installed with `snap install --edge astral-ty`, but it behaves exactly the same with `pip install --break-system-packages ty`.

In any of those situations, I can't manage to make `ty` discover the system packages, even manually with `--python`.

I believe that's because the system packages don't live in a `site-packages` folder, but rather in a `dist-packages` one: `/usr/lib/python3/dist-packages/apt`. No matter which path I give to `ty`, I always end-up with the following error:
```
  Cause: Failed to discover the site-packages directory
```

@carljm I'll leave you reopen the issue if you think that's a sufficiently different problem than #989, otherwise, thanks for the quick answer :-)

---

_Comment by @carljm on 2025-10-08 22:03_

Good observation! I think that's a different problem indeed. Our installed-packages discovery just doesn't know about `dist-packages` at all. That's not a standard upstream Python thing, it's a Debian-specific invention. But still, it exists on many real systems, and we should support it.

I'll reopen this issue and convert it to being about `dist-packages` support. It should be easy enough to add support for it.

---

_Reopened by @carljm on 2025-10-08 22:03_

---

_Label `question` removed by @carljm on 2025-10-08 22:03_

---

_Renamed from "`ty` doesn't find system packages" to "support finding packages installed into Debian `dist-packages` directories" by @carljm on 2025-10-08 22:36_

---

_Added to milestone `GA` by @carljm on 2025-10-08 22:36_

---

_Comment by @bendini on 2026-01-05 16:45_

I was just about to report this issue and discovered it already existed.

This came up as an issue for me in Ubuntu 24.04 LTS when looking at code in a new folder that didn't already contain a .venv subfolder (it otherwise works fine when one has been configured), which then made the ty settings invalid and generated hundreds of false-positive issues in my code that are already on the ignore list in the pyproject.toml because it was no longer able to recognize the list of issues to be ignored.

---

_Comment by @BrianSipos on 2026-01-08 20:12_

Is there any workaround for this on Debian/Ubuntu using existing command options? Or must a virtualenv be used to avoid the problem?

---

_Comment by @carljm on 2026-01-08 23:28_

You could use `PYTHONPATH` or the `extra-paths` config and point it at the dist-packages directory; that's not an exact 1:1 (it won't process `.pth` files / editable installs) but it would probably work for most real-world cases. Or you could use a venv.

I think this issue is actually pretty easy to fix -- in fact I just had Claude put up a PR that should fix it: https://github.com/astral-sh/ruff/pull/22466

But this PR still requires validation on a real Debian/Ubuntu system, and I don't have time to do that at the moment -- so if anyone is motivated to see this issue fixed faster, your help validating and reviewing the PR would be welcome!

---
