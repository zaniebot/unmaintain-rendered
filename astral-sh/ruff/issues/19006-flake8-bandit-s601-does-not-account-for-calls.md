---
number: 19006
title: "[`flake8-bandit`] `S601` does not account for calls through clients"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - rule
assignees: []
created_at: 2025-06-28T00:13:41Z
updated_at: 2025-07-04T04:49:00Z
url: https://github.com/astral-sh/ruff/issues/19006
synced_at: 2026-01-10T01:23:00Z
---

# [`flake8-bandit`] `S601` does not account for calls through clients

---

_Issue opened by @MeGaGiGaGon on 2025-06-28 00:13_

### Summary

I found this while working on #18972

[paramiko-call (S601)](https://docs.astral.sh/ruff/rules/paramiko-call/#paramiko-call-s601) only checks for a `paramiko.exec_command`, as seen in the test case:
https://github.com/astral-sh/ruff/blob/90cb0d3a7b0d1b9528854dad64dd17330bfd5f36/crates/ruff_linter/resources/test/fixtures/flake8_bandit/S601.py#L1-L3

And the given example code does not raise: https://play.ruff.rs/6b4aa322-f3b1-49cd-997d-f14b5c1bac41

However, as both the example in `S601`'s docs show, and on [`paramiko`'s docs](https://docs.paramiko.org/en/stable/api/client.html#paramiko.client.SSHClient), this call can also be done through an intermediary client object:
```py
client = SSHClient()
client.load_system_host_keys()
client.connect('ssh.example.com')
stdin, stdout, stderr = client.exec_command('ls -l')
```

The rule needs to either be more general like how [suspicious-subprocess-import (S404)](https://docs.astral.sh/ruff/rules/suspicious-subprocess-import/#suspicious-subprocess-import-s404) is a blanket search for importing `subprocess`, or have additional cases added, since reading through the `paramiko` docs it looks like `exec_command` is just a convenience wrapper around functionality provided in the [`Client`](https://docs.paramiko.org/en/stable/api/client.html#paramiko.client.SSHClient.exec_command) object, which would share the same security concerns. I'm also not certain `paramiko.exec_command` is even valid code using `paramiko`, as a [github search](https://github.com/search?q=%22paramiko.exec_command%22&type=code&p=1) for `"paramiko.exec_command"` only returned it in `flake8-bandit`/`ruff` test cases.

### Version

playground

---

_Referenced in [astral-sh/ruff#18972](../../astral-sh/ruff/issues/18972.md) on 2025-06-28 00:14_

---

_Comment by @ntBre on 2025-06-30 13:24_

I think this makes sense and is arguably a bug in the rule. We only check for the qualified path `paramiko.exec_command`, as you said:

https://github.com/astral-sh/ruff/blob/db3dcd8ad60a352b175837063e71b321eab53fe6/crates/ruff_linter/src/rules/flake8_bandit/rules/paramiko_calls.rs#L41-L46

whereas I think upstream is checking for _any_ call named `exec_command` if `paramiko` has been imported:

https://github.com/PyCQA/bandit/blob/ffed1bbf0adb2d259005aca6da506e86a291c987/bandit/plugins/injection_paramiko.py#L56-L58

I think we could try to resolve the call through a `Client`, at least within a single file.

---

_Label `rule` added by @ntBre on 2025-06-30 13:24_

---

_Comment by @CodeMan62 on 2025-07-04 04:49_

I am fixing it 

---

_Referenced in [astral-sh/ruff#19149](../../astral-sh/ruff/pulls/19149.md) on 2025-07-04 17:50_

---
