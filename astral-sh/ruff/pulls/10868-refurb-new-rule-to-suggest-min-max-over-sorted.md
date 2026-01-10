```yaml
number: 10868
title: "[`refurb`] New rule to suggest min/max over sorted() (`FURB192`)"
type: pull_request
state: merged
author: ottaviohartman
labels:
  - rule
  - accepted
assignees: []
merged: true
base: main
head: thartman/furb192
created_at: 2024-04-11T00:17:56Z
updated_at: 2024-04-23T01:14:11Z
url: https://github.com/astral-sh/ruff/pull/10868
synced_at: 2026-01-10T22:37:01Z
```

# [`refurb`] New rule to suggest min/max over sorted() (`FURB192`)

---

_Pull request opened by @ottaviohartman on 2024-04-11 00:17_

## Summary

Fixes #10463

Add `FURB192` which detects violations like this:

```python
# Bad
a = sorted(l)[0]

# Good
a = min(l)
```

There is a caveat that @Skylion007 has pointed out, which is that violations with `reverse=True` technically aren't compatible with this change, in the edge case where the unstable behavior is intended. For example:

```python
from operator import itemgetter
data = [('red', 1), ('blue', 1), ('red', 2), ('blue', 2)]

min(data, key=itemgetter(0))  # ('blue', 1)
sorted(data, key=itemgetter(0))[0]  # ('blue', 1)
sorted(data, key=itemgetter(0), reverse=True)[-1]  # ('blue, 2')
```

This seems like a rare edge case, but I can make the `reverse=True` fixes unsafe if that's best.

## Test Plan

This is unit tested.

## References

https://github.com/dosisod/refurb/pull/333/files


---

_@ottaviohartman reviewed on 2024-04-11 00:19_

---

_Review comment by @ottaviohartman on `ruff.schema.json`:3080 on 2024-04-11 00:19_

This auto-generated `FURB19` but I can't figure out why

---

_@ottaviohartman reviewed on 2024-04-11 00:23_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/refurb/rules/sorted_min_max.rs`:169 on 2024-04-11 00:23_

Is there a cleaner or more canonical way to generate this?

---

_Comment by @github-actions[bot] on 2024-04-11 00:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bcbcb8e39cccab4cb375c6f972add9762a151ae8/airflow/providers/yandex/secrets/lockbox.py#L258'>airflow/providers/yandex/secrets/lockbox.py:258:16:</a> FURB192 [*] Prefer `min` over `sorted()` to compute the minimum value in a sequence
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a10139e2e26d35553f26d7479999ff79adaed6cc/rotkehlchen/db/dbhandler.py#L269'>rotkehlchen/db/dbhandler.py:269:25:</a> FURB192 [*] Prefer `max` over `sorted()` to compute the maximum value in a sequence
+ <a href='https://github.com/rotki/rotki/blob/a10139e2e26d35553f26d7479999ff79adaed6cc/rotkehlchen/globaldb/handler.py#L127'>rotkehlchen/globaldb/handler.py:127:21:</a> FURB192 [*] Prefer `max` over `sorted()` to compute the maximum value in a sequence
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/841bc6039dd426a31eaf78c797e84e4f5b8f110c/indico/modules/events/registration/util.py#L272'>indico/modules/events/registration/util.py:272:16:</a> FURB192 [*] Prefer `min` over `sorted()` to compute the minimum value in a sequence
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB192 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-04-11 06:15_

---

_Label `accepted` added by @MichaReiser on 2024-04-11 06:15_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB192.py`:23 on 2024-04-11 10:50_

Why is this example not an error? Because the list is already sorted?

---

_@VascoSch92 reviewed on 2024-04-11 10:50_

---

_@ottaviohartman reviewed on 2024-04-11 21:27_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/resources/test/fixtures/refurb/FURB192.py`:23 on 2024-04-11 21:27_

Oops - it should be. I forgot to match on `ExprList` as well [here](https://github.com/astral-sh/ruff/blob/b73e47c81cac55ada626532fca795a3b7e2e3eb6/crates/ruff_linter/src/rules/refurb/rules/sorted_min_max.rs#L82)

---

_@ottaviohartman reviewed on 2024-04-12 13:35_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/resources/test/fixtures/refurb/FURB192.py`:23 on 2024-04-12 13:35_

I haven't found another example of outputting an `ast::ExprList` into a String, if there is an example to follow that would be great. I'll keep looking

---

_Comment by @Skylion007 on 2024-04-12 14:18_

I'd definitely advocate for making unstable sort fixes unsafe. Otherwise, it could unknowingly change user code. I'd also argue we may want add to add a config option for this rule that ignores unstable sorts.

---

_@ottaviohartman reviewed on 2024-04-16 01:23_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/refurb/rules/sorted_min_max.rs`:161 on 2024-04-16 01:23_

Is there a cleaner or more canonical way to generate this?

---

_@ottaviohartman reviewed on 2024-04-16 01:36_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/registry/rule_set.rs`:6 on 2024-04-16 01:36_

Had to increase this, otherwise received

```
Rule index out of bounds. Increase the size of the bitset array.
```

---

_Comment by @ottaviohartman on 2024-04-16 01:40_

@Skylion007 
> I'd definitely advocate for making unstable sort fixes unsafe. Otherwise, it could unknowingly change user code. 

Agreed, and updated!

> I'd also argue we may want add to add a config option for this rule that ignores unstable sorts.

Shall I do this too? Haven't heard anyone else chime in yet.

---

_Review requested from @VascoSch92 by @ottaviohartman on 2024-04-16 01:40_

---

_Comment by @codspeed-hq[bot] on 2024-04-16 01:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ottaviohartman:thartman/furb192)

### Merging #10868 will **not alter performance**

<sub>Comparing <code>ottaviohartman:thartman/furb192</code> (da36bfc) with <code>main</code> (925c7f8)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@ottaviohartman reviewed on 2024-04-16 01:54_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/registry/rule_set.rs`:6 on 2024-04-16 01:54_

I think this slowed down performance tests, but I'm continuing to look.

---

_@ottaviohartman reviewed on 2024-04-16 01:57_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/registry/rule_set.rs`:6 on 2024-04-16 01:57_

Yes, seems to have slowed down similar to the last PR #9384 that increased this value.

That benchmark is here: https://codspeed.io/astral-sh/ruff/branches/qdegraaf:rule/S504

Please correct me if I'm wrong!

---

_Comment by @zanieb on 2024-04-16 03:29_

> Shall I do this too? Haven't heard anyone else chime in yet.

I probably wouldn't implement it now since the rule is just going into preview. We can use preview feedback to consider adding an option. I'd just open a tracking issue when this is merged.

---

_@zanieb reviewed on 2024-04-16 03:32_

---

_Review comment by @zanieb on `crates/ruff_linter/src/registry/rule_set.rs`:6 on 2024-04-16 03:32_

cc @MichaReiser who I believe designed this in https://github.com/astral-sh/ruff/pull/3606

---

_@MichaReiser reviewed on 2024-04-16 07:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/registry/rule_set.rs`:6 on 2024-04-16 07:15_

Bumping is the right step here and I don't think there's much we can do about perf (although I'm surprised by the amount). I would need to do some profiling to exactly understand why it slows down expressions so significantly (we may now hit a case where optimisations get disabled because of the size of the bitset?)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-23 00:15_

---

_@charliermarsh approved on 2024-04-23 00:59_

Thanks!

---

_Merged by @charliermarsh on 2024-04-23 01:13_

---

_Closed by @charliermarsh on 2024-04-23 01:13_

---
