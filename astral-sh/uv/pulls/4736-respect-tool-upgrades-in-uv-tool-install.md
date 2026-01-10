```yaml
number: 4736
title: "Respect tool upgrades in `uv tool install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/tool-upgrade
created_at: 2024-07-02T19:15:31Z
updated_at: 2024-07-02T20:46:33Z
url: https://github.com/astral-sh/uv/pull/4736
synced_at: 2026-01-10T13:48:28Z
```

# Respect tool upgrades in `uv tool install`

---

_Pull request opened by @charliermarsh on 2024-07-02 19:15_

## Summary

For now the semantics are such that if the requested requirements from the command line don't match the receipt (or if any `--reinstall` or `--upgrade` is requested), we proceed with an install, passing the `--reinstall` and `--upgrade` to the underlying Python environment.

This may lead to some unintuitive behaviors, but it's simplest for now. For example:

- `uv tool install black<24` followed by `uv tool install black --upgrade` will install the latest version of `black`, removing the `<24` constraint.
- `uv tool install black --with black-plugin` followed by `uv tool install black` will remove `black-plugin`.

Closes https://github.com/astral-sh/uv/issues/4659.


---

_Renamed from "Respect tool upgrades in uv tool install" to "Respect tool upgrades in `uv tool install`" by @charliermarsh on 2024-07-02 19:15_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-02 19:15_

---

_@charliermarsh reviewed on 2024-07-02 19:17_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:1241 on 2024-07-02 19:17_

This is a behavior change but I personally think it's ok to copy over the entrypoints even on a no-op. I could probably figure out a way to avoid this as an optimization though.

---

_@charliermarsh reviewed on 2024-07-02 19:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:183 on 2024-07-02 19:17_

It seems ok to me to only remove this when `--force` is provided?

---

_@charliermarsh reviewed on 2024-07-02 19:22_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:183 on 2024-07-02 19:22_

In other words: can't we defer to our underlying virtualenv update machinery to manage this by reinstalling packages as required?

---

_Comment by @charliermarsh on 2024-07-02 19:28_

Don't understand the dreaded Windows failures yet but I'll look at them once approved.

---

_@zanieb reviewed on 2024-07-02 19:34_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:1241 on 2024-07-02 19:34_

I agree it's "okay" but it feels a little weird to me.

---

_@charliermarsh reviewed on 2024-07-02 19:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:1241 on 2024-07-02 19:35_

I think it's safer and simpler to copy them over every time, avoids any weird issues around trying to detect if the package changed in the environment or the entrypoints getting out-of-sync with the environment.


---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:183 on 2024-07-02 19:35_

That seems okay, yeah.

---

_@zanieb reviewed on 2024-07-02 19:35_

---

_@zanieb reviewed on 2024-07-02 19:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 19:37_

I don't think `force` needs to imply deletion of the environment though? This seems a little weird. I think we should only remove the existing environment if the requested Python interpreter is different.

---

_@charliermarsh reviewed on 2024-07-02 19:45_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 19:45_

We can do that… I feel like force is sorta useful too personally.

---

_@charliermarsh reviewed on 2024-07-02 19:45_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 19:45_

Force would be like: completely ignore whatever already exists.

---

_@zanieb reviewed on 2024-07-02 19:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 19:54_

Hm. It feels weird to mix the purpose for me. I agree removing the environment _sounds_ useful but in what case would someone actually need to do that?

---

_@zanieb approved on 2024-07-02 20:05_

---

_@zanieb reviewed on 2024-07-02 20:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 20:05_

(it seems safe to discuss and address this separately from this pull request)

---

_@charliermarsh reviewed on 2024-07-02 20:22_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 20:22_

I guess if the environment was broken and the receipt was out-of-sync, even `uninstall` wouldn't remove it...? Maybe I'll change `uninstall` to handle that edge case, then remove this.

---

_Label `preview` added by @zanieb on 2024-07-02 20:22_

---

_@zanieb reviewed on 2024-07-02 20:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 20:34_

If the environment is broken then `--force` totally could / should remove it.

---

_@charliermarsh reviewed on 2024-07-02 20:37_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:177 on 2024-07-02 20:37_

Maybe: if there's no receipt, and the environment exists, we remove it? :)

---

_Merged by @charliermarsh on 2024-07-02 20:46_

---

_Closed by @charliermarsh on 2024-07-02 20:46_

---

_Branch deleted on 2024-07-02 20:46_

---
