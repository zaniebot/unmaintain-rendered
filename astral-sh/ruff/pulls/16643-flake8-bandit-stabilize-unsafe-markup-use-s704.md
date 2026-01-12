```yaml
number: 16643
title: "[`flake8-bandit`] Stabilize `unsafe-markup-use` (`S704`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/s704-0.10
created_at: 2025-03-11T19:04:59Z
updated_at: 2025-03-12T18:48:37Z
url: https://github.com/astral-sh/ruff/pull/16643
synced_at: 2026-01-12T15:55:55Z
```

# [`flake8-bandit`] Stabilize `unsafe-markup-use` (`S704`)

---

_@ntBre_

Summary
--

Stabilizes S704, which is also being recoded from RUF035 in 0.10.

Test Plan
--
Existing tests with `PreviewMode` removed from the settings.

There was one issue closed on 2024-12-20 calling the rule noisy and asking for a config option, but the option was added and then there were no more issues or PRs.

---

_Label `rule` added by @ntBre on 2025-03-11 19:04_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 19:04_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 19:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fs704-0.10)

### Merging #16643 will **degrade performances by 12.73%**

<sub>Comparing <code>brent/s704-0.10</code> (5678e15) with <code>micha/ruff-0.10</code> (85f7871)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fs704-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.5 ms | -12.73% |


---

_Comment by @github-actions[bot] on 2025-03-11 19:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+30 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/0675231060ce71583df08d36ca42f14e2d821451/providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py#L2307'>providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py:2307:19:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/connectors/sqla/models.py#L1308'>superset/connectors/sqla/models.py:1308:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/models/dashboard.py#L225'>superset/models/dashboard.py:225:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/models/helpers.py#L538'>superset/models/helpers.py:538:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/models/helpers.py#L567'>superset/models/helpers.py:567:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/models/slice.py#L338'>superset/models/slice.py:338:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/models/sql_lab.py#L445'>superset/models/sql_lab.py:445:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/utils/core.py#L485'>superset/utils/core.py:485:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/account.py#L100'>securedrop/journalist_app/account.py:100:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/account.py#L87'>securedrop/journalist_app/account.py:87:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/admin.py#L224'>securedrop/journalist_app/admin.py:224:32:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/admin.py#L279'>securedrop/journalist_app/admin.py:279:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/admin.py#L295'>securedrop/journalist_app/admin.py:295:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/col.py#L103'>securedrop/journalist_app/col.py:103:17:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/col.py#L75'>securedrop/journalist_app/col.py:75:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/main.py#L169'>securedrop/journalist_app/main.py:169:17:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/main.py#L192'>securedrop/journalist_app/main.py:192:21:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/main.py#L203'>securedrop/journalist_app/main.py:203:21:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/utils.py#L267'>securedrop/journalist_app/utils.py:267:9:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/utils.py#L337'>securedrop/journalist_app/utils.py:337:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/utils.py#L366'>securedrop/journalist_app/utils.py:366:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/utils.py#L380'>securedrop/journalist_app/utils.py:380:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/utils.py#L503'>securedrop/journalist_app/utils.py:503:17:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/utils.py#L519'>securedrop/journalist_app/utils.py:519:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/source_app/utils.py#L37'>securedrop/source_app/utils.py:37:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/source_app/utils.py#L44'>securedrop/source_app/utils.py:44:11:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/source_app/utils.py#L51'>securedrop/source_app/utils.py:51:22:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/source_app/utils.py#L65'>securedrop/source_app/utils.py:65:11:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/template_filters.py#L24'>securedrop/template_filters.py:24:21:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b4bb708c1453d0edfe2c4e82902480371c47134e/zerver/views/documentation.py#L292'>zerver/views/documentation.py:292:35:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S704 | 30 | 30 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 20:28_

There appear to be a few varieties of false positives here:

* The [first case](https://github.com/apache/airflow/blob/0675231060ce71583df08d36ca42f14e2d821451/providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py#L2307) I clicked on is inside of a function called `_cli_safe_flash`. I guess this would need a `noqa` because it doesn't seem like the rule offers another way to mark an *enclosing* function as safe. This one seems pretty fair because the caller is supposed to guarantee the safety, based on the name of the function.
* There are two f-string expressions in [this case](https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/models/dashboard.py#L225), but the second calls `markupsafe.escape`, which I don't think the rule handles.
* The argument [here](https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/utils/core.py#L485) is generated by `nh3.clean` but assigned to a variable. As far as I can tell the rule doesn't do any variable resolution, it only checks for string literals, bytes literals, and calls directly in the `Markdown()` call, so this also won't work with the allowlist.
* This [case](https://github.com/freedomofpress/securedrop/blob/9542cf78760246667ab90dbe1a39845d0fc7f47f/securedrop/journalist_app/col.py#L103) calls `.format()` with the results of two `escape` calls
  * securedrop has ~12 other calls like that

These seem pretty tricky to handle in general, so I don't necessarily think they should halt stabilization. The main case that would be nice to handle is probably checking if a variable passed to `Markdown` has just had `escape` called on it.

---

_Comment by @MichaReiser on 2025-03-12 08:24_

> These seem pretty tricky to handle in general, so I don't necessarily think they should halt stabilization. The main case that would be nice to handle is probably checking if a variable passed to Markdown has just had escape called on it.

I agree; supporting this would be nice. I'm less concerned about keeping `nh3` clean because it's a third-party library. I also think it's fine to stabilize the rule regardless and creating a follow up issue to support tracing the `escape` call because the implementation matches upstream https://github.com/PyCQA/bandit/pull/1225. It's also a security rule where we optimize for fewer false negatives over avoiding false-positives.

---

_@MichaReiser approved on 2025-03-12 08:25_

---

_Comment by @Daverball on 2025-03-12 08:34_

>   The [first case](https://github.com/apache/airflow/blob/0675231060ce71583df08d36ca42f14e2d821451/providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py#L2307) I clicked on is inside of a function called `_cli_safe_flash`. I guess this would need a `noqa` because it doesn't seem like the rule offers another way to mark an _enclosing_ function as safe. This one seems pretty fair because the caller is supposed to guarantee the safety, based on the name of the function.

I would consider this a true positive, considering you can inject markup with your username there. They pass f-strings into that function. It might be possible for them to add `airflow.providers.fab.auth_manager.security_manager.override.FabAirflowSecurityManagerOverride._cli_safe_flash` to `extend-markup-names` to detect that. Which would turn it into an actual false positive.

>  There are two f-string expressions in [this case](https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/models/dashboard.py#L225), but the second calls `markupsafe.escape`, which I don't think the rule handles.

Indeed, people should be directed towards `Markup.format` for safe interpolation even when they have remembered to manually use `escape`, since it's less error-prone. It might be slightly slower at the moment though, since the C-Extension currently only accelerates the inner-most escape. It doesn't currently accelerate the custom formatter logic. Although if/once it does, it might be slightly faster, so it doesn't feel like a strong enough argument to me, to let people live dangerously.

>  The argument [here](https://github.com/apache/superset/blob/d3ba2755e8ed3d5a2fe5b5e6eba1bc629ad84f44/superset/utils/core.py#L485) is generated by `nh3.clean` but assigned to a variable. As far as I can tell the rule doesn't do any variable resolution, it only checks for string literals, bytes literals, and calls directly in the `Markdown()` call, so this also won't work with the allowlist.

Correct, the rule does not support indirect assignment yet, it's part of and documented in the test cases.

>  securedrop has ~12 other calls like that
> These seem pretty tricky to handle in general, so I don't necessarily think they should halt stabilization. The main case that would be nice to handle is probably checking if a variable passed to `Markdown` has just had `escape` called on it.

Yeah, I've seen those. Once again I think it's good to direct them towards `Markup.format`, so they can build safer habits. But I realize it's a balancing act, since causing too much churn might motivate people to just disable the rule entirely. 

Maybe we could add a strictness setting as a compromise, if there's sufficient demand for allowing f-strings and `str.format` as long as all the interpolations are using either `markupsafe.escape` or a function from the whitelist.

---

_Comment by @Daverball on 2025-03-12 08:48_

Alternatively we could consider adding an unsafe fix for converting simple interpolations inside `Markup` calls via `str.format` or f-strings, into a safe `Markup.format` call. Since that is the most common source of errors.

---

_Comment by @ntBre on 2025-03-12 18:48_

Thanks again for the context, I think it does make sense to push people toward `Markup.format` and to err on the side of security as you both said.

Let's go ahead and stabilize this, and I'll open an issue to track the `escape` and autofix ideas!

---

_Merged by @ntBre on 2025-03-12 18:48_

---

_Closed by @ntBre on 2025-03-12 18:48_

---

_Branch deleted on 2025-03-12 18:48_

---
