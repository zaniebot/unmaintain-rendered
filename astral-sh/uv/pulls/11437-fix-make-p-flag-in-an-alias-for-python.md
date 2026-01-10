```yaml
number: 11437
title: "fix: make -p flag in  an alias for --python"
type: pull_request
state: closed
author: Aditya-PS-05
labels:
  - cli
  - breaking
assignees: []
base: main
head: fix/pip-compile-python-flag
created_at: 2025-02-12T06:44:51Z
updated_at: 2025-02-13T18:52:32Z
url: https://github.com/astral-sh/uv/pull/11437
synced_at: 2026-01-10T11:10:38Z
```

# fix: make -p flag in  an alias for --python

---

_Pull request opened by @Aditya-PS-05 on 2025-02-12 06:44_

closes #11285 

## Changes
- Modified the argument parsing in `PipCompileArgs` to make `-p` alias to `--python`
- Removed short form from `--python-version` option
- Added explicit long and short form specifications to avoid ambiguity

---

_@j178 reviewed on 2025-02-12 07:12_

---

_Review comment by @j178 on `crates/uv-cli/src/lib.rs`:1346 on 2025-02-12 07:12_

Why `long` and `env = EnvVars::UV_PYTHON` are removed?

---

_@Aditya-PS-05 reviewed on 2025-02-12 07:20_

---

_Review comment by @Aditya-PS-05 on `crates/uv-cli/src/lib.rs`:1346 on 2025-02-12 07:20_

a typo, I wanted to change in `PipCompileArgs` but accidently also changed in `PipSyncArgs`.

---

_Review requested from @j178 by @Aditya-PS-05 on 2025-02-12 09:19_

---

_@zanieb reviewed on 2025-02-12 14:52_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1092 on 2025-02-12 14:52_

I don't think we need the verbose `long` and `short` options — we don't use that anywhere and coherence with the existing style is important.

This also seems to add support for the `UV_PYTHON` variable. It's fine to leave here, but please be careful with changes like that / note them in the summary.

---

_Comment by @zanieb on 2025-02-12 14:53_

Unfortunately this is a breaking change and we'll need to do some tricks to make this roughly backwards compatible. I can own that.

---

_Added to milestone `v0.6.0` by @zanieb on 2025-02-12 14:53_

---

_Label `breaking` added by @zanieb on 2025-02-12 14:53_

---

_Label `cli` added by @zanieb on 2025-02-12 14:55_

---

_Comment by @zanieb on 2025-02-12 14:57_

The failure mode can be seen in CI

```
    9       │-warning: The requested Python version 3.8 is not available; 3.12.[X] will be used to build dependencies instead.
   10       │-Resolved 1 package in [TIME]
          5 │+error: No interpreter found for Python 3.8 in virtual environments, managed installations, or search path
```

We could decide this is okay and just hint to use `--python-version` instead there? I worry we'll break a fair number of workflows though. Thoughts @charliermarsh ?

The alternative is `--python` falls back to the semantics of `--python-version` if it can't find the version — but then there's not a great way to say "Fail if this Python version cannot be found".

---

_@Aditya-PS-05 reviewed on 2025-02-13 06:22_

---

_Review comment by @Aditya-PS-05 on `crates/uv-cli/src/lib.rs`:1092 on 2025-02-13 06:22_

Ok 

---

_Comment by @zanieb on 2025-02-13 18:52_

I tried a different approach in #11486 

---

_Closed by @zanieb on 2025-02-13 18:52_

---
