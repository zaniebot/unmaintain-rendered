---
number: 5567
title: uv 0.2.30 started installing some dependencies non-editably even though they are installed with -e
type: issue
state: closed
author: gibsondan
labels:
  - bug
assignees: []
created_at: 2024-07-29T17:44:05Z
updated_at: 2024-07-29T17:54:48Z
url: https://github.com/astral-sh/uv/issues/5567
synced_at: 2026-01-07T13:12:17-06:00
---

# uv 0.2.30 started installing some dependencies non-editably even though they are installed with -e

---

_Issue opened by @gibsondan on 2024-07-29 17:44_

Still working on an MRE for you, but we are seeing that after the 0.2.30 upgrade, our 'dev install' command that tries to install a bunch of dagster packages editably is no longer installing them editably.

Here's the script that we run to install our modules: https://github.com/dagster-io/dagster/blob/master/scripts/install_dev_python_modules.py#L50-L105

We've reliably bisected to this uv release, after this command is no longer returning an empty string as expected after running that script (i.e. not every dependency was installed editably, even though they were all installed with the `-e` flag):

```
pip list --exclude-editable | grep -e dagster | grep -v dagster-hex | grep -v dagster-hightouch
```

Will update with an MRE once we have one, but based on multiple repros are fairly confident that something regressed (or at least changed in a breaking way) in uv 0.2.30

---

_Comment by @charliermarsh on 2024-07-29 17:45_

I believe this is fixed #5545. I can cut a release shortly.

---

_Comment by @gibsondan on 2024-07-29 17:50_

Ah I should have checked closed issues, that does look very likely to be the same thing.

---

_Comment by @charliermarsh on 2024-07-29 17:50_

No worries, this was a confusing one.

---

_Comment by @charliermarsh on 2024-07-29 17:51_

I have a new release going out shortly: https://github.com/astral-sh/uv/pull/5568

---

_Label `bug` added by @charliermarsh on 2024-07-29 17:54_

---

_Comment by @charliermarsh on 2024-07-29 17:54_

Closed by https://github.com/astral-sh/uv/pull/5545.

---

_Closed by @charliermarsh on 2024-07-29 17:54_

---
