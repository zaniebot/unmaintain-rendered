```yaml
number: 14944
title: "[red-knot] Tests for 'while' loop boundness"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/while-loop-boundness
created_at: 2024-12-12T19:45:27Z
updated_at: 2024-12-12T20:06:58Z
url: https://github.com/astral-sh/ruff/pull/14944
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Tests for 'while' loop boundness

---

_@sharkdp_

## Summary

Regression test(s) for something that broken while implementing #14759. We have similar tests for other control flow elements, but feel free to let me know if this seems superfluous.

## Test Plan

New mdtests


---

_Label `red-knot` added by @sharkdp on 2024-12-12 19:45_

---

_Review requested from @carljm by @sharkdp on 2024-12-12 19:45_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-12 19:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-12 19:45_

---

_@carljm approved on 2024-12-12 19:47_

Definitely useful to have these, thank you!

---

_@AlexWaygood approved on 2024-12-12 19:49_

---

_Comment by @github-actions[bot] on 2024-12-12 19:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +16 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/13e2df0d7074cbc1a8d59d7044d5bfcb69147a3d/pandas/core/groupby/groupby.py#L4082'>pandas/core/groupby/groupby.py:4082:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/13e2df0d7074cbc1a8d59d7044d5bfcb69147a3d/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/billing_page.py#L326'>corporate/views/billing_page.py:326:24:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/billing_page.py#L326'>corporate/views/billing_page.py:326:24:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L158'>corporate/views/remote_billing_page.py:158:26:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L158'>corporate/views/remote_billing_page.py:158:26:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:42:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:42:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L160'>corporate/views/remote_billing_page.py:160:48:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L160'>corporate/views/remote_billing_page.py:160:48:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L678'>corporate/views/remote_billing_page.py:678:26:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L678'>corporate/views/remote_billing_page.py:678:26:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L679'>corporate/views/remote_billing_page.py:679:42:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L679'>corporate/views/remote_billing_page.py:679:42:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L680'>corporate/views/remote_billing_page.py:680:48:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L680'>corporate/views/remote_billing_page.py:680:48:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:5:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/zulip/zulip/blob/2138885fbf9f747e437ab7f455caa6b5fa68cd8a/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:5:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI061 | 18 | 0 | 2 | 16 | 0 |

</p>
</details>




---

_Merged by @sharkdp on 2024-12-12 20:06_

---

_Closed by @sharkdp on 2024-12-12 20:06_

---

_Branch deleted on 2024-12-12 20:06_

---
