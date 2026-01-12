```yaml
number: 7393
title: Reflect conversion reason in UP012 messages
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/UP012
created_at: 2023-09-14T19:59:10Z
updated_at: 2023-09-14T20:14:58Z
url: https://github.com/astral-sh/ruff/pull/7393
synced_at: 2026-01-12T02:39:10Z
```

# Reflect conversion reason in UP012 messages

---

_Pull request opened by @charliermarsh on 2023-09-14 19:59_

Closes https://github.com/astral-sh/ruff/issues/7254.

---

_Label `documentation` added by @charliermarsh on 2023-09-14 19:59_

---

_Merged by @charliermarsh on 2023-09-14 20:08_

---

_Closed by @charliermarsh on 2023-09-14 20:08_

---

_Branch deleted on 2023-09-14 20:08_

---

_Comment by @github-actions[bot] on 2023-09-14 20:14_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -3, 0 error(s))

<details><summary>content (+3, -3)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/f50398cb0796460af4f7296f8b1e0d320c2a4a29/Packs/CarbonBlackDefense/Integrations/CarbonBlackLiveResponseCloud/CarbonBlackLiveResponseCloud_test.py#L169'>Packs/CarbonBlackDefense/Integrations/CarbonBlackLiveResponseCloud/CarbonBlackLiveResponseCloud_test.py:169:6:</a> UP012 [*] Unnecessary UTF-8 `encoding` argument to `encode`
- <a href='https://github.com/demisto/content/blob/f50398cb0796460af4f7296f8b1e0d320c2a4a29/Packs/CarbonBlackDefense/Integrations/CarbonBlackLiveResponseCloud/CarbonBlackLiveResponseCloud_test.py#L169'>Packs/CarbonBlackDefense/Integrations/CarbonBlackLiveResponseCloud/CarbonBlackLiveResponseCloud_test.py:169:6:</a> UP012 [*] Unnecessary call to `encode` as UTF-8
+ <a href='https://github.com/demisto/content/blob/f50398cb0796460af4f7296f8b1e0d320c2a4a29/Packs/CortexXDR/Integrations/XQLQueryingEngine/XQLQueryingEngine.py#L829'>Packs/CortexXDR/Integrations/XQLQueryingEngine/XQLQueryingEngine.py:829:20:</a> UP012 [*] Unnecessary UTF-8 `encoding` argument to `encode`
- <a href='https://github.com/demisto/content/blob/f50398cb0796460af4f7296f8b1e0d320c2a4a29/Packs/CortexXDR/Integrations/XQLQueryingEngine/XQLQueryingEngine.py#L829'>Packs/CortexXDR/Integrations/XQLQueryingEngine/XQLQueryingEngine.py:829:20:</a> UP012 [*] Unnecessary call to `encode` as UTF-8
+ <a href='https://github.com/demisto/content/blob/f50398cb0796460af4f7296f8b1e0d320c2a4a29/Packs/CrowdStrikeFalconX/Integrations/CrowdStrikeFalconX/CrowdStrikeFalconX.py#L252'>Packs/CrowdStrikeFalconX/Integrations/CrowdStrikeFalconX/CrowdStrikeFalconX.py:252:22:</a> UP012 [*] Unnecessary UTF-8 `encoding` argument to `encode`
- <a href='https://github.com/demisto/content/blob/f50398cb0796460af4f7296f8b1e0d320c2a4a29/Packs/CrowdStrikeFalconX/Integrations/CrowdStrikeFalconX/CrowdStrikeFalconX.py#L252'>Packs/CrowdStrikeFalconX/Integrations/CrowdStrikeFalconX/CrowdStrikeFalconX.py:252:22:</a> UP012 [*] Unnecessary call to `encode` as UTF-8
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| UP012 | 6 | 3 | 3 |



---
