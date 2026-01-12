```yaml
number: 19143
title: Config file discovery on Mac OS inconsistent with documentation
type: issue
state: closed
author: gkowzan
labels:
  - documentation
assignees: []
created_at: 2025-07-04T09:08:41Z
updated_at: 2025-08-17T23:35:38Z
url: https://github.com/astral-sh/ruff/issues/19143
synced_at: 2026-01-12T15:54:56Z
```

# Config file discovery on Mac OS inconsistent with documentation

---

_@gkowzan_

### Summary

On Mac OS ruff looks for global `pyproject.toml` in $HOME/.config/ruff per #10739, but item 3 in [Config file discovery](https://docs.astral.sh/ruff/configuration/#config-file-discovery) says that ruff will use [etcetera's native strategy](https://docs.rs/etcetera/latest/etcetera/#native-strategy). The native strategy would look for `pyproject.toml` under $HOME/Library/Preferences/ruff.

### Version

ruff 0.9.2 (c20255abe 2025-01-16)

---

_Comment by @ntBre on 2025-07-04 14:54_

It looks like we do still support the old location, but we'll emit a deprecation warning:

https://github.com/astral-sh/ruff/blob/411cccb35eafbf91787d36ca6092101328211e2c/crates/ruff_workspace/src/pyproject.rs#L130-L135

but I agree that we should update the docs! Would you be interested in opening a PR? No worries if not, I can add the `help wanted` tag or handle it myself :)

---

_Label `documentation` added by @ntBre on 2025-07-04 14:54_

---

_Comment by @gkowzan on 2025-07-07 18:18_

The deprecated behavior uses etcetera's data_dir (`~/Library/Application Support`) instead of config_dir (`~/Library/Preferences`), so I believe that even in this case the documentation is confusing. I did not mention in the pull request the deprecated behavior. Should that be included along with deprecation warning?

---

_Comment by @ntBre on 2025-07-07 21:09_

Oh nice catch on the `data_dir` vs `config_dir`, I didn't notice that part. I think it could be more confusing to try to describe the deprecated behavior, so your PR looks perfect to me!

---

_Closed by @dylwil3 on 2025-08-17 23:35_

---
