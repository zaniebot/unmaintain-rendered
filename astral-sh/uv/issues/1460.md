```yaml
number: 1460
title: "`uv pip install -C`/`--config-settings`"
type: issue
state: closed
author: nschloe
labels:
  - enhancement
  - help wanted
  - compatibility
assignees: []
created_at: 2024-02-16T09:26:34Z
updated_at: 2024-02-23T00:53:46Z
url: https://github.com/astral-sh/uv/issues/1460
synced_at: 2026-01-10T05:40:31Z
```

# `uv pip install -C`/`--config-settings`

---

_Issue opened by @nschloe on 2024-02-16 09:26_

`pip install -C` (or `--config-settings`) allows one to pass config settings to the build backend. From the pip `--help`:

```
  -C, --config-settings <settings>
                              Configuration settings to be passed to the PEP 517
                              build backend. Settings take the form KEY=VALUE. Use
                              multiple --config-settings options to pass multiple
                              keys to the backend.
```

Would be great to have that in uv.

---

_Label `enhancement` added by @zanieb on 2024-02-16 14:40_

---

_Label `compatibility` added by @zanieb on 2024-02-16 14:40_

---

_Comment by @zanieb on 2024-02-16 14:41_

Thanks for the issue! Yeah we don't pass anything to the build backend right now but should expose a way to.

---

_Label `help wanted` added by @zanieb on 2024-02-16 14:41_

---

_Comment by @henryiii on 2024-02-16 16:20_

Note this is called `-C / --config-setting` (no s) in build. And while it's pretty close to useless in setuptools, it's a key feature of alternative builders like scikit-build-core and meson-python.

---

_Comment by @smackesey on 2024-02-16 21:28_

We need this feature to cleanly port the [Dagster](https://github.com/dagster-io/dagster) dev/CI setup over to `uv` since we rely on `pip install -e <pkg> --config-settings editable-mode=compat`.

---

_Comment by @henryiii on 2024-02-17 17:17_

Any thoughts on the best way to do this? Currently it's just rendering a string `"{}"` and putting that into the commands. Would it be a hash table on `SourceBuild`, or an option added to all the methods?

---

_Comment by @ion-elgreco on 2024-02-21 09:27_

I would also like to add that would be useful for us, we have a couple editable installs at the moment in our project

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-21 19:46_

---

_Comment by @charliermarsh on 2024-02-21 19:47_

I can take this one, hopefully today.

---

_Comment by @charliermarsh on 2024-02-21 23:02_

PR is up.

---

_Closed by @charliermarsh on 2024-02-23 00:53_

---
