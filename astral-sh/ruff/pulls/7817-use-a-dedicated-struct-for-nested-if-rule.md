```yaml
number: 7817
title: "Use a dedicated struct for \"nested if\" rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/elif
created_at: 2023-10-04T17:54:40Z
updated_at: 2023-10-04T18:25:29Z
url: https://github.com/astral-sh/ruff/pull/7817
synced_at: 2026-01-12T02:32:41Z
```

# Use a dedicated struct for "nested if" rule

---

_Pull request opened by @charliermarsh on 2023-10-04 17:54_

Internal refactor -- finding this rule hard to understand.

---

_Label `internal` added by @charliermarsh on 2023-10-04 17:54_

---

_Merged by @charliermarsh on 2023-10-04 18:18_

---

_Closed by @charliermarsh on 2023-10-04 18:18_

---

_Branch deleted on 2023-10-04 18:19_

---

_Comment by @codspeed-hq[bot] on 2023-10-04 18:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/elif)

### Merging #7817 will **improve performances by 3%**

<sub>Comparing <code>charlie/elif</code> (3c2eb26) with <code>main</code> (a0c846f)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/elif` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 39 ms | 37.8 ms | +3% |


---

_Comment by @github-actions[bot] on 2023-10-04 18:25_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>content (+0, -5)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/CiscoUmbrellaReporting/Integrations/CiscoUmbrellaReporting/CiscoUmbrellaReporting.py#L222'>Packs/CiscoUmbrellaReporting/Integrations/CiscoUmbrellaReporting/CiscoUmbrellaReporting.py:222:5:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Imperva_WAF/Integrations/ImpervaWAF/ImpervaWAF.py#L106'>Packs/Imperva_WAF/Integrations/ImpervaWAF/ImpervaWAF.py:106:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/SailPointIdentityIQ/Integrations/SailPointIdentityIQ/SailPointIdentityIQ.py#L175'>Packs/SailPointIdentityIQ/Integrations/SailPointIdentityIQ/SailPointIdentityIQ.py:175:5:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Slack/Scripts/SlackBlockBuilder/SlackBlockBuilder.py#L120'>Packs/Slack/Scripts/SlackBlockBuilder/SlackBlockBuilder.py:120:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/SymantecManagementCenter/Integrations/SymantecManagementCenter/SymantecManagementCenter.py#L989'>Packs/SymantecManagementCenter/Integrations/SymantecManagementCenter/SymantecManagementCenter.py:989:5:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM102 | 5 | 0 | 5 |



---
