```yaml
number: 2384
title: Support installation on CI (GitHub Actions)
type: issue
state: open
author: amotl
labels:
  - question
assignees: []
created_at: 2026-01-07T22:21:41Z
updated_at: 2026-01-07T22:48:54Z
url: https://github.com/astral-sh/ty/issues/2384
synced_at: 2026-01-10T01:56:41Z
```

# Support installation on CI (GitHub Actions)

---

_Issue opened by @amotl on 2026-01-07 22:21_

Hi. First things first: Thanks a stack for conceiving `ty`, and a happy new year!

We'd like to invoke `ty` in a traditional CI/GHA environment where we are not using `uv` yet. As such, the package and its dependencies are installed into the global system environment, because it's ephemeral anyway. Currently, `ty` doesn't find third-party modules, and we don't know how to configure it properly, because we don't know upfront where GHA (`actions/setup-python`) locates its Python environment.

- https://github.com/crate/pytest-cratedb/actions/runs/20797404169/job/59734360168

Maybe you know a good trick how to make `ty` work in this environment? We tried to configure [`env.pythonLocation`](https://github.com/actions/setup-python/blob/main/docs/advanced-usage.md#environment-variables), but it didn't work.

```yaml
env:
  VIRTUAL_ENV: ${{ env.pythonLocation }}
```

```
ty failed
  Cause: Failed to discover local Python environment
  Cause: Invalid `VIRTUAL_ENV` environment variable `/opt/hostedtoolcache/Python/3.14.2/x64`: points to a broken venv with no pyvenv.cfg file
```

#### References
- GH-2068


---

_Comment by @carljm on 2026-01-07 22:36_

Thanks for the report! I think this is basically a duplicate of #2068, since that's how ty should support this case. But I'll leave it open as a question, to help others maybe find it.

---

_Label `question` added by @carljm on 2026-01-07 22:36_

---

_Renamed from "Support installation on CI/GHA" to "Support installation on CI (GitHub Actions)" by @amotl on 2026-01-07 22:46_

---

_Comment by @amotl on 2026-01-07 22:48_

Thank you Carl, you are right. I first posted this message at https://github.com/astral-sh/ty/issues/2068#issuecomment-3720992492, but then decided to use a separate issue, because others might know some tricks how to configure `ty` properly on GHA without modifying it, and without using a virtualenv.

---
