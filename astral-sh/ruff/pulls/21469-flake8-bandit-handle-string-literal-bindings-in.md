```yaml
number: 21469
title: "[`flake8-bandit`] Handle string literal bindings in suspicious-url-open-usage (`S310`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-21462
created_at: 2025-11-15T04:48:50Z
updated_at: 2025-11-26T13:44:28Z
url: https://github.com/astral-sh/ruff/pull/21469
synced_at: 2026-01-10T16:48:02Z
```

# [`flake8-bandit`] Handle string literal bindings in suspicious-url-open-usage (`S310`)

---

_Pull request opened by @danparizher on 2025-11-15 04:48_

## Summary

Fixes false positive in `S310` when URL arguments are assigned to variables that resolve to string literals with safe schemes (`http://` or `https://`).

Fixes #21462.

## Problem

The rule flagged `urllib.request.urlretrieve(path, ...)` when `path` was assigned a string literal like `"https://example.com/data.csv"` because it only checked direct string literals, f-strings, and concatenation, not variable bindings.

## Approach

Added `resolve_to_string_literal()` helper that uses `find_binding_value()` to resolve variable bindings to their string literal values, following the pattern in `unnecessary_regular_expression.rs`. Updated the `urlopen`/`urlretrieve` handler to check resolved string literals before falling back to `leading_chars()`.

## Test Plan

Added test case to `S310.py` for the variable binding scenario.

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 04:56_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -4 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8d121e175d33e34021b03c6425023d9d2a02eacd/dev/breeze/src/airflow_breeze/utils/constraints_version_check.py#L364'>dev/breeze/src/airflow_breeze/utils/constraints_version_check.py:364:14:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/ibis/examples/gen_registry.py#L175'>ibis/examples/gen_registry.py:175:59:</a> RUF100 Unused `noqa` directive (unused: `S310`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/81a22684a0494b874f3daa39bfa8ba4309998a68/package.py#L350'>package.py:350:52:</a> RUF100 [*] Unused `noqa` directive (unused: `S310`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f06f717b759d043638e0d6ea2a54dcfe0860c956/tools/droplets/create.py#L49'>tools/droplets/create.py:49:20:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
- <a href='https://github.com/zulip/zulip/blob/f06f717b759d043638e0d6ea2a54dcfe0860c956/tools/droplets/create.py#L63'>tools/droplets/create.py:63:20:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
- <a href='https://github.com/zulip/zulip/blob/f06f717b759d043638e0d6ea2a54dcfe0860c956/tools/droplets/create.py#L82'>tools/droplets/create.py:82:20:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S310 | 4 | 0 | 4 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>





---

_@edgarrmondragon reviewed on 2025-11-17 05:12_

---

_Review comment by @edgarrmondragon on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1222 on 2025-11-17 05:12_

It might also be worth similarly resolving the `url` argument of `urllib.request.Request`

---

_@amyreese approved on 2025-11-17 23:07_

---

_Comment by @amyreese on 2025-11-17 23:09_

The one ecosystem change is from a project where this previously triggered no longer getting a false positive, and therefore getting an "unused suppression" from their noqa.

This might be worth gating behind preview flag.

---

_Review requested from @ntBre by @amyreese on 2025-11-17 23:09_

---

_Label `rule` added by @MichaReiser on 2025-11-18 08:12_

---

_Label `accepted` added by @MichaReiser on 2025-11-18 08:12_

---

_Label `accepted` removed by @MichaReiser on 2025-11-18 08:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1230 on 2025-11-18 20:07_

This version of `resolve_to_string_literal` actually seems too restrictive in this case since `leading_chars` just takes any `Expr`. I guess we should just resolve an `Expr::Name` to whatever its binding is and pass that directly to `leading_chars`. Something like:

```rust
                        Some(expr) => {
                            let expr = resolve_name(expr);

                            if leading_chars(expr).is_some_and(has_http_prefix) {
                                return;
                            }
                        }
```

Then we could add tests showing that we handle resolved f-strings and concatenated string literals.

---

_@ntBre reviewed on 2025-11-18 20:10_

Thanks, this looks reasonable to me. But I think we can resolve names more generally, and I agree with the suggestion to apply this to the `url` argument too.

I also agree that this should be a preview change.

---

_Review requested from @ntBre by @danparizher on 2025-11-19 23:57_

---

_Comment by @edgarrmondragon on 2025-11-20 05:02_

@danparizher I opened https://github.com/danparizher/ruff/pull/1 against your fork to handle calls to `urllib.request.Request` outside of `urllib.request.urlopen`, and also process concatenated string literals recursively.

---

_Comment by @ntBre on 2025-11-21 19:57_

Oh, I was waiting to see if the PR against the fork was merged, but I think the URL resolution was also fixed in the last commit. Is that right?

---

_Comment by @edgarrmondragon on 2025-11-21 20:16_

> Oh, I was waiting to see if the PR against the fork was merged, but I think the URL resolution was also fixed in the last commit. Is that right?

IIUC, the last commit in this PR fixed the `urllib.request.urlopen(urllib.request.Request(url))` case, but not `urllib.request.Request(url)` on its own.

It also didn't fully address this comment (https://github.com/astral-sh/ruff/pull/21469#discussion_r2539560233)

> handle resolved f-strings and concatenated string literals.

But my all means, feel free to merge without my updates, which I can open a new PR for.

---

_Label `preview` added by @MichaReiser on 2025-11-26 08:41_

---

_@MichaReiser approved on 2025-11-26 09:17_

Thank you

---

_Merged by @MichaReiser on 2025-11-26 09:21_

---

_Closed by @MichaReiser on 2025-11-26 09:21_

---

_Branch deleted on 2025-11-26 13:44_

---
