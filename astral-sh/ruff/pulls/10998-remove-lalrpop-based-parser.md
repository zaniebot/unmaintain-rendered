```yaml
number: 10998
title: Remove LALRPOP based parser
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/farewell-lalrpop
created_at: 2024-04-17T11:42:43Z
updated_at: 2024-04-17T13:20:41Z
url: https://github.com/astral-sh/ruff/pull/10998
synced_at: 2026-01-10T22:37:01Z
```

# Remove LALRPOP based parser

---

_Pull request opened by @dhruvmanila on 2024-04-17 11:42_

_No description provided._

---

_Label `parser` added by @dhruvmanila on 2024-04-17 11:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-17 11:42_

---

_Comment by @codspeed-hq[bot] on 2024-04-17 11:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/farewell-lalrpop)

### Merging #10998 will **degrade performances by 4.92%**

<sub>Comparing <code>dhruv/farewell-lalrpop</code> (6868313) with <code>dhruv/parser</code> (c30057a)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/farewell-lalrpop)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/farewell-lalrpop` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.9 ms | 27.1 ms | +6.59% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.84% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -4.92% |


---

_@MichaReiser approved on 2024-04-17 12:05_

:partying_face: 

---

_Comment by @dhruvmanila on 2024-04-17 12:49_

I'm going to make sure the ecosystem checks are all good and then merge even though I've ran them locally

---

_Comment by @github-actions[bot] on 2024-04-17 13:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/fa0af92c6b1ebd7fd2716612f42a95f5dc274121/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:12:</a> E999 SyntaxError: Multiple exception types must be parenthesized
- <a href='https://github.com/demisto/content/blob/fa0af92c6b1ebd7fd2716612f42a95f5dc274121/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:21:</a> E999 SyntaxError: Unexpected token ,
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/fa0af92c6b1ebd7fd2716612f42a95f5dc274121/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:12:</a> E999 SyntaxError: Multiple exception types must be parenthesized
- <a href='https://github.com/demisto/content/blob/fa0af92c6b1ebd7fd2716612f42a95f5dc274121/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:21:</a> E999 SyntaxError: Unexpected token ,
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @dhruvmanila on 2024-04-17 13:15_

The ecosystem changes are as expected, updating the syntax error message.

---

_Merged by @dhruvmanila on 2024-04-17 13:20_

---

_Closed by @dhruvmanila on 2024-04-17 13:20_

---

_Branch deleted on 2024-04-17 13:20_

---
