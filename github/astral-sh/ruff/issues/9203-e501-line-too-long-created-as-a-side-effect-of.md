---
number: 9203
title: E501 line-too-long created as a side effect of applying UP015
type: issue
state: closed
author: eli-schwartz
labels: []
assignees: []
created_at: 2023-12-19T22:49:18Z
updated_at: 2024-01-02T17:59:22Z
url: https://github.com/astral-sh/ruff/issues/9203
synced_at: 2026-01-07T13:12:15-06:00
---

# E501 line-too-long created as a side effect of applying UP015

---

_Issue opened by @eli-schwartz on 2023-12-19 22:49_

- Given a ruff.toml that declares the line length to be 79.
- On the https://github.com/openstack/ironic-python-agent codebase:

```
$ ruff check . --select E501
# success

$ ruff check . --select UP015,E501 --fix
ironic_python_agent/hardware.py:96:80: E501 Line too long (82 > 79)
Found 15 errors (14 fixed, 1 remaining).
```

```diff
diff --git a/ironic_python_agent/hardware.py b/ironic_python_agent/hardware.py
index 95faabca..a48f9dc1 100644
--- a/ironic_python_agent/hardware.py
+++ b/ironic_python_agent/hardware.py
@@ -93,8 +93,7 @@ def _get_device_info(dev, devclass, field):
     """Get the device info according to device class and field."""
     try:
         devname = os.path.basename(dev)
-        with open('/sys/class/%s/%s/device/%s' % (devclass, devname, field),
-                  'r') as f:
+        with open('/sys/class/%s/%s/device/%s' % (devclass, devname, field)) as f:
             return f.read().strip()
     except IOError:
         LOG.warning("Can't find field %(field)s for "
```

Ruff is responsible for reformatting this line when applying *lint* fixes. It should respect the max line length, and avoid folding the two lines together as a side effect of removing the open mode.

---

_Comment by @jayofdoom on 2023-12-19 22:51_

Thanks for proxying this bugreport; I can confirm I saw this behavior when trialing ruff on the ironic-python-agent codebase.

---

_Comment by @eli-schwartz on 2023-12-19 23:05_

If ruff isn't confident it can apply the UP015 fixer it should just refuse to do it at all without --unsafe-fixes, of course. Either solution is valid, though I suspect users would be okay with just removing the `'r'` anyways.

---

_Comment by @charliermarsh on 2023-12-20 00:29_

The ideal behavior here is for us to re-format the fix, but in lieu of that, I don't know that _not_ offering the fix is actually better -- we've had a lot of confusion in the past when adding that behavior to specific rules (it's respected in _some_ cases): https://github.com/astral-sh/ruff/issues/8106

---

_Comment by @eli-schwartz on 2023-12-20 00:47_

Maybe there needs to be:
1) a generic mechanism for autofixes to detect when they would rewrite code which formerly obeyed the line-length setting, to violate it, and mark that autofix as long-line-unsafe
2) a dedicated option, similar to --unsafe-fixes but *not actually called unsafe-fixes*, e.g. `--long-line-fixes`

I definitely do not think it's appropriate to violate the line length.
- not everyone uses, or wants to use, a formatter
- ruff combined with autofixes can be very useful for performing one-off migrations
- many projects have CI which is gated on flake8, but not ruff, so it would be advantageous to be able to run `ruff check --fix` and get fewer fixes, but passing CI
- but mostly, "In the face of ambiguity, refuse the temptation to guess." If ruff is going to emit an error code either way, then there is no point in *both* modifying the file *and* emitting an error code. Ruff should not make a decision about whether you prefer to get an error due to the UP015 code or get a different error due to the E501 code. Instead, ruff should simply report whichever error your codebase already has, and let *you* make the judgment call.

---

_Comment by @charliermarsh on 2024-01-02 17:59_

I'm going to merge this into https://github.com/astral-sh/ruff/issues/8106 and make the issue more general -- bear with me...

---

_Closed by @charliermarsh on 2024-01-02 17:59_

---

_Referenced in [astral-sh/ruff#8106](../../astral-sh/ruff/issues/8106.md) on 2024-01-02 18:00_

---

_Referenced in [astral-sh/ruff#15820](../../astral-sh/ruff/issues/15820.md) on 2025-01-30 01:33_

---
