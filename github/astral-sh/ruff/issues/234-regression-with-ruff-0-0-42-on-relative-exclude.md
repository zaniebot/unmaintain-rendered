---
number: 234
title: Regression with ruff 0.0.42 on relative exclude files
type: issue
state: closed
author: amotl
labels: []
assignees: []
created_at: 2022-09-20T16:03:20Z
updated_at: 2022-09-20T20:15:20Z
url: https://github.com/astral-sh/ruff/issues/234
synced_at: 2026-01-07T13:12:14-06:00
---

# Regression with ruff 0.0.42 on relative exclude files

---

_Issue opened by @amotl on 2022-09-20 16:03_

Hi again,

apologies for creating so many issues and for sometimes missing to catch up, because the pace is so fast. I hope you still appreciate my feedback.

After adjusting our exclude pattern recently as outlined at https://github.com/charliermarsh/ruff/issues/203#issuecomment-1249055408 and https://github.com/jpmens/mqttwarn/commit/fad50af912e, we just had to downgrade to ruff 0.0.40 (https://github.com/jpmens/mqttwarn/commit/041a3f425d) to make it work again.

Currently, on 0.0.42, `ruff` would croak on both configuration variants of `extend-exclude`. There is probably another thing amongst the recent changes I am missing here?

```
tests/etc/functions_bad.py:0:0: E999 SyntaxError: unexpected indent at line 6 column 5
```
```toml
extend-exclude = [
  "./tests/etc/functions_bad.py"
]
```
```toml
extend-exclude = [
  "tests/etc/functions_bad.py"
]
```

Keep up the spirit and with kind regards,
Andreas.

P.S.: Just to make it clear: [`tests/etc/functions_bad.py`](https://github.com/jpmens/mqttwarn/blob/0.29.1/tests/etc/functions_bad.py) _intentionally_ has bad formatting, because we use it as a canary for running our plugin machinery on bad input files. That is why we need to make `ruff` skip it.


---

_Comment by @charliermarsh on 2022-09-20 16:13_

Sorry, will look into this now.

---

_Comment by @charliermarsh on 2022-09-20 16:15_

Legitimate bug thank you! I assume you're running `ruff .`?


---

_Comment by @amotl on 2022-09-20 16:21_

> I assume you're running `ruff .`?

We are actually running `ruff` only on a subdirectory of `mqttwarn` yet, per [`ruff tests`](https://github.com/jpmens/mqttwarn/blob/ee3a736e7dd88442c837e38b5600817d4e3591a6/pyproject.toml#L59). However, the same admonition, on the corresponding file, also happens when invoking `ruff .` -- amongst many others yet unfixed within the code base.


---

_Referenced in [astral-sh/ruff#235](../../astral-sh/ruff/pulls/235.md) on 2022-09-20 16:25_

---

_Closed by @charliermarsh on 2022-09-20 16:26_

---

_Comment by @charliermarsh on 2022-09-20 16:28_

Hopefully fixed in v0.0.43 (building now) but please let me know if not! I cloned your repo and tested on that directly.

---

_Comment by @charliermarsh on 2022-09-20 16:39_

Apologies for all the churn here. The strategy for inclusion / exclusion has stabilized a lot so I don't anticipate any more breakages or API changes (apart from backwards-compatible changes, like ignoring `.venv*`).

---

_Comment by @amotl on 2022-09-20 16:42_

No worries about those hiccups. Speaking for me, I was happy to help on the corresponding teething issues, and I am sure @jack1142 enjoys it equally well. Cheers!

P.S.: I will report back about the outcome on 0.0.43, but I am sure it will be fixed. Thanks for going the extra mile by cloning our repo.

---

_Comment by @amotl on 2022-09-20 17:39_

🌧️ Build failed, maybe just a hiccup. -- https://github.com/charliermarsh/ruff/actions/runs/3091788765/jobs/5002298427

---

_Comment by @amotl on 2022-09-20 20:15_

Hi again. @charliermarsh: I can confirm that ruff 0.0.43 works excellent again. Thank you once more for fixing it so quickly.

---
