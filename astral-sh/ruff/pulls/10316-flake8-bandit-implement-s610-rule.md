```yaml
number: 10316
title: "[`flake8-bandit`]: Implement `S610` rule"
type: pull_request
state: merged
author: mkniewallner
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/add-flake8-bandit-S610
created_at: 2024-03-09T18:14:49Z
updated_at: 2024-03-12T00:23:00Z
url: https://github.com/astral-sh/ruff/pull/10316
synced_at: 2026-01-10T22:47:01Z
```

# [`flake8-bandit`]: Implement `S610` rule

---

_Pull request opened by @mkniewallner on 2024-03-09 18:14_

Part of https://github.com/astral-sh/ruff/issues/1646.

## Summary

Implement `S610` rule from `flake8-bandit`. 

Upstream references:
- Implementation: https://github.com/PyCQA/bandit/blob/1.7.8/bandit/plugins/django_sql_injection.py#L20-L97
- Test cases: https://github.com/PyCQA/bandit/blob/1.7.8/examples/django_sql_injection_extra.py
- Test assertion: https://github.com/PyCQA/bandit/blob/1.7.8/tests/functional/test_functional.py#L517-L524

The implementation in `bandit` targets additional arguments (`params`, `order_by` and `select_params`) but doesn't seem to do anything with them in the end, so I did not include them in the implementation.

Note that this rule could be prone to false positives, as ideally we would want to check if `extra()` is tied to a [Django queryset](https://docs.djangoproject.com/en/5.0/ref/models/querysets/), but AFAIK Ruff is not able to resolve classes outside of the current module.

## Test Plan

Snapshot tests

---

_Review comment by @mkniewallner on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:40 on 2024-03-09 18:19_

This is also done for [django_raw_sql](https://github.com/astral-sh/ruff/blob/0c84fbb6db646550e00eed5560a692f2ea1b6224/crates/ruff_linter/src/rules/flake8_bandit/rules/django_raw_sql.rs#L39-L41), but I'm wondering if this should also be done for this rule.

In `django_raw_sql`, `RawSQL` import comes from `django` module, so this is reliable, but in our case, `extra` can be used even if no `django` imports occurs, if for instance a model defined in another file in a codebase is used, so this could skip the check for several valid cases.

On the other hand, the entire rule has the potential of having several false positives, so this could be seen as a way to know if a file belongs to a project using Django. But if I'm not mistaken, `seen_module` only applies to the current file, not the whole codebase, and in this specific case the latter would probably be preferable.

---

_Comment by @github-actions[bot] on 2024-03-09 18:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+22 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+22 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/actions/message_flags.py#L108'>zerver/actions/message_flags.py:108:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/actions/message_flags.py#L160'>zerver/actions/message_flags.py:160:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/actions/message_flags.py#L232'>zerver/actions/message_flags.py:232:15:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/actions/message_flags.py#L42'>zerver/actions/message_flags.py:42:15:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/actions/message_flags.py#L56'>zerver/actions/message_flags.py:56:23:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/lib/message.py#L536'>zerver/lib/message.py:536:15:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/lib/message.py#L576'>zerver/lib/message.py:576:36:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/lib/push_notifications.py#L1076'>zerver/lib/push_notifications.py:1076:15:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/lib/query_helpers.py#L26'>zerver/lib/query_helpers.py:26:24:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/lib/users.py#L200'>zerver/lib/users.py:200:89:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/models/streams.py#L265'>zerver/models/streams.py:265:47:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_flags.py#L267'>zerver/tests/test_message_flags.py:267:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_flags.py#L300'>zerver/tests/test_message_flags.py:300:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_flags.py#L577'>zerver/tests/test_message_flags.py:577:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_flags.py#L681'>zerver/tests/test_message_flags.py:681:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_flags.py#L692'>zerver/tests/test_message_flags.py:692:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_move_topic.py#L1505'>zerver/tests/test_message_move_topic.py:1505:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_move_topic.py#L1512'>zerver/tests/test_message_move_topic.py:1512:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_move_topic.py#L1577'>zerver/tests/test_message_move_topic.py:1577:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/tests/test_message_move_topic.py#L1584'>zerver/tests/test_message_move_topic.py:1584:19:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/views/read_receipts.py#L80'>zerver/views/read_receipts.py:80:15:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
+ <a href='https://github.com/zulip/zulip/blob/3a0621fb66a1805dacb267fff5c9c75773a9d985/zerver/worker/queue_processors.py#L1079'>zerver/worker/queue_processors.py:1079:93:</a> S610 Use of Django `extra` can lead to SQL injection vulnerabilities
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S610 | 22 | 22 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@mkniewallner reviewed on 2024-03-09 19:26_

---

_@mkniewallner reviewed on 2024-03-09 19:38_

---

_Review comment by @mkniewallner on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:40 on 2024-03-09 19:38_

Case in point, the ecosystem checks detects 18 errors while `bandit` detects 22. That's because there are 4 things that would have been flagged in https://github.com/zulip/zulip/blob/29ca4ba6620f16598e1171be7495356f56b84328/zerver/tests/test_message_move_topic.py, but are skipped because there is no import from `django` in this file.

---

_@mkniewallner reviewed on 2024-03-09 19:40_

---

_Review comment by @mkniewallner on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:58 on 2024-03-09 19:40_

`flatten()` is used to remove optional keys here. Not sure that this is really a clean way to do this, let me know if not.

---

_Marked ready for review by @mkniewallner on 2024-03-09 19:40_

---

_Label `rule` added by @charliermarsh on 2024-03-11 22:21_

---

_Label `preview` added by @charliermarsh on 2024-03-11 22:21_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-11 22:21_

---

_@charliermarsh reviewed on 2024-03-11 22:33_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:40 on 2024-03-11 22:33_

I think I'm okay with leaving this in for now.

---

_@charliermarsh reviewed on 2024-03-11 22:33_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:40 on 2024-03-11 22:33_

I could go either way though. `extra` isn't a common method name.

---

_@mkniewallner reviewed on 2024-03-11 22:38_

---

_Review comment by @mkniewallner on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:40 on 2024-03-11 22:38_

Yeah, I think that the combination of `extra` and `select`/`where`/`tables` arguments should not be that common outside of Django. Worst case, for the few times it would get triggered, it's always possible for users to ignore the false positives, while skipping the check entirely if Ruff does not find a `django` import would leave the user with no clue that the check was not run because of that. So I think it might be best in the end to not check if Django was seen.

---

_Review comment by @mkniewallner on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:40 on 2024-03-11 22:48_

I went ahead and removed the check in https://github.com/astral-sh/ruff/pull/10316/commits/4e881428c95628d03504b2d7ce5001256a2a26bf. Also updated the error message so that in case of false positives, the user at least knows that this is related to Django.

---

_@mkniewallner reviewed on 2024-03-11 22:48_

---

_@charliermarsh reviewed on 2024-03-11 23:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:58 on 2024-03-11 23:12_

Do we want to flag on `select(**kwargs)`? It seems like this wouldn't, but it probably _should_, right?

---

_@charliermarsh reviewed on 2024-03-12 00:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:58 on 2024-03-12 00:21_

Nevermind, I misread.

---

_Merged by @charliermarsh on 2024-03-12 00:22_

---

_Closed by @charliermarsh on 2024-03-12 00:22_

---

_@charliermarsh reviewed on 2024-03-12 00:22_

Thanks!

---

_@charliermarsh reviewed on 2024-03-12 00:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs`:58 on 2024-03-12 00:23_

We might want to consider allowing: `select(**{"foo": "bar"})`

---
