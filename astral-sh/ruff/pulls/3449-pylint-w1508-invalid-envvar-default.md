```yaml
number: 3449
title: "pylint: W1508 invalid-envvar-default"
type: pull_request
state: merged
author: latonis
labels:
  - rule
assignees: []
merged: true
base: main
head: invalid-envvar-default-pylint
created_at: 2023-03-11T17:33:54Z
updated_at: 2023-03-12T17:31:40Z
url: https://github.com/astral-sh/ruff/pull/3449
synced_at: 2026-01-12T04:39:44Z
```

# pylint: W1508 invalid-envvar-default

---

_Pull request opened by @latonis on 2023-03-11 17:33_

Rule Reference: https://pylint.pycqa.org/en/latest/user_guide/messages/warning/invalid-envvar-default.html

Implemented for Pylint parent issue: #970 

---

_Comment by @github-actions[bot] on 2023-03-11 17:50_

ℹ️ ecosystem check **detected changes**. (+5, -0, 0 error(s))

<details><summary>zulip</summary>
<p>

```diff
+ zproject/dev_settings.py:168:63: PLW1508 Invalid type for environment variable default; expected `str` or `None`
```

</p>
</details>
<details><summary>bokeh</summary>
<p>

_No changes detected_.

</p>
</details>
<details><summary>scikit-build</summary>
<p>

_No changes detected_.

</p>
</details>
<details><summary>airflow</summary>
<p>

```diff
+ tests/system/providers/microsoft/azure/example_azure_service_bus.py:43:56: PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ tests/system/providers/microsoft/azure/example_azure_synapse.py:26:56: PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ tests/system/providers/microsoft/azure/example_azure_synapse.py:30:54: PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ tests/system/providers/microsoft/azure/example_azure_synapse.py:31:83: PLW1508 Invalid type for environment variable default; expected `str` or `None`
```

</p>
</details>

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Label `rule` added by @charliermarsh on 2023-03-11 21:32_

---

_Comment by @charliermarsh on 2023-03-11 21:33_

Thanks! I made the check a little more conservative. We don't do much type inference, so for (e.g.) `os.getenv("FOO", BAR)`, we don't have a way to know whether `BAR` is a string or `None`. For now, I stopped flagging those cases, since I think they'd end up with more false positives than false negatives.

---

_Merged by @charliermarsh on 2023-03-11 21:44_

---

_Closed by @charliermarsh on 2023-03-11 21:44_

---

_Branch deleted on 2023-03-12 17:31_

---
