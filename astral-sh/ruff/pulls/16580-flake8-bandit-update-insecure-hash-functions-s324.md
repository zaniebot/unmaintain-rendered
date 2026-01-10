```yaml
number: 16580
title: "[flake8-bandit] Update insecure hash functions (S324)"
type: pull_request
state: closed
author: VascoSch92
labels: []
assignees: []
base: main
head: S324
created_at: 2025-03-09T20:19:05Z
updated_at: 2025-09-25T08:28:26Z
url: https://github.com/astral-sh/ruff/pull/16580
synced_at: 2026-01-10T17:40:28Z
```

# [flake8-bandit] Update insecure hash functions (S324)

---

_Pull request opened by @VascoSch92 on 2025-03-09 20:19_

This PR solves issue #16572 

Could be an idea to define a HashSet containing the names of the insicure hash functions? In this way would be easier to update in case other insicure hash functions are added to the list.


---

_Comment by @github-actions[bot] on 2025-03-09 20:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/223a741869505ad31c38310f307bf2f0f0f193fb/dev/breeze/src/airflow_breeze/utils/path_utils.py#L107'>dev/breeze/src/airflow_breeze/utils/path_utils.py:107:32:</a> S324 Probable use of insecure hash functions in `hashlib`: `blake2b`
+ <a href='https://github.com/apache/airflow/blob/223a741869505ad31c38310f307bf2f0f0f193fb/scripts/ci/pre_commit/update_breeze_config_hash.py#L44'>scripts/ci/pre_commit/update_breeze_config_hash.py:44:32:</a> S324 Probable use of insecure hash functions in `hashlib`: `blake2b`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S324 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/223a741869505ad31c38310f307bf2f0f0f193fb/dev/breeze/src/airflow_breeze/utils/path_utils.py#L107'>dev/breeze/src/airflow_breeze/utils/path_utils.py:107:32:</a> S324 Probable use of insecure hash functions in `hashlib`: `blake2b`
+ <a href='https://github.com/apache/airflow/blob/223a741869505ad31c38310f307bf2f0f0f193fb/scripts/ci/pre_commit/update_breeze_config_hash.py#L44'>scripts/ci/pre_commit/update_breeze_config_hash.py:44:32:</a> S324 Probable use of insecure hash functions in `hashlib`: `blake2b`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S324 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "[flake8-bandit] Update insicure hash functions (S324)" to "[flake8-bandit] Update insecure hash functions (S324)" by @AlexWaygood on 2025-03-09 22:02_

---

_@MichaReiser reviewed on 2025-03-10 08:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hashlib_insecure_hash_functions.rs`:150 on 2025-03-10 08:36_

I'd like to have a link to a reference documenting that these are references. I also think that they are so obscure (the number once), that I don't feel like we have to support them. 

A reference is important because I otherwise find it impossible to know what these obscure numbers refer to and if they are indeed insecure.


Edit: I took a closer look at the issue. I think it's perfectly fine if Ruff only supports the guaranteed hash functions and that we, instead, should update the documentation to state so.

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_bandit/rules/hashlib_insecure_hash_functions.rs`:150 on 2025-03-16 19:53_

Hey,

Thanks for the feedback. I tried to find a clear reference, but it's hard to find one which resume all the insicure hash-functions (or I'm not good in searching ;-)) . I agree that it makes sense for Ruff to only support reliably secure hash functions.

---

_@VascoSch92 reviewed on 2025-03-16 19:53_

---

_@MichaReiser requested changes on 2025-03-17 07:49_

See inline comment

---

_@Skylion007 reviewed on 2025-03-17 16:33_

Aren't we doing this the wrong way? We should be maintaining a whitelist instead of a blacklist of FIPS supported hashlib functions. Maybe with a way to specify additional safe ones through the pyproject.toml? As far I understand it, FIPS specification is a whitelist of supported functions, not a blacklist anyway.

---

_Comment by @VascoSch92 on 2025-04-18 09:37_

> Aren't we doing this the wrong way? We should be maintaining a whitelist instead of a blacklist of FIPS supported hashlib functions. Maybe with a way to specify additional safe ones through the pyproject.toml? As far I understand it, FIPS specification is a whitelist of supported functions, not a blacklist anyway.

Should I update the rule with respect to that?

---

_Review requested from @Skylion007 by @VascoSch92 on 2025-04-19 13:26_

---

_Review requested from @MichaReiser by @VascoSch92 on 2025-04-19 13:26_

---

_Comment by @VascoSch92 on 2025-04-19 13:28_

Hey @MichaReiser @Skylion007 

I update the rule to have a whitelist instead of a blacklist.

For instance, we have just two elements in the whitelist, namely, `sha256`, and `sha512`.

What do you think about that?

Should also update the description to mention these two hashlib?

---

_Comment by @mikeshardmind on 2025-04-19 13:43_

The FIPS-approved hashes are not the only secure hash functions in hashlib. FIPS is govermental compliance, and not an exclusive set of "Good" hashes. While it may make sense to have a lint rule for people who need FIPS compliance, that's not what this lint rule is documented for.

An example of this would be blake2. While blake3 is faster without becoming insecure, blake2 is not a weak, broken, or otherwise unfit for security related use as a hash function

---

_Comment by @ntBre on 2025-09-24 20:09_

Thanks for your work on this! I'll close it for now since we updated the documentation instead in #20534.

---

_Closed by @ntBre on 2025-09-24 20:09_

---

_Branch deleted on 2025-09-25 08:28_

---
