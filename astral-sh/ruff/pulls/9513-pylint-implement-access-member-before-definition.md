```yaml
number: 9513
title: "[pylint] - implement  access-member-before-definition / E0203 rule"
type: pull_request
state: closed
author: Glyphack
labels:
  - accepted
assignees: []
base: main
head: linter-pylint-E0203
created_at: 2024-01-14T09:39:41Z
updated_at: 2025-05-21T20:26:49Z
url: https://github.com/astral-sh/ruff/pull/9513
synced_at: 2026-01-12T15:55:29Z
```

# [pylint] - implement  access-member-before-definition / E0203 rule

---

_@Glyphack_

## Summary

Part of pylint rules: https://github.com/astral-sh/ruff/issues/970

<!-- What's the purpose of the change? What does it do, and why? -->
- Add --preview to running test command because all the newly added rules are preview and I think we want this flag to be there to run them.
- Implement [ pylint E0203 ](https://pylint.pycqa.org/en/latest/user_guide/messages/error/access-member-before-definition.html) rule.
- Implement load & store for attribute expressions & store assignments to instance and 
attributes in class scope. I left the support for `cls` access for later because this rule does not depend on it. My plan is that with this new feature in checker we can try to tackle rules like E1101 after this PR.

The rule runs on __init__ function scopes. It goes over all the unresolved attributes(new) in the scope and finds the attribute assignment and emits the diagnostic if member is initialized later. If the member is accessed but not initialized at all it won't emit any errors.

Also this rule will not apply for classes with base classes, since base classes might contain the unresolved attributes. The problem with that is we need to support multi file analysis. Which I think should be for another PR.


Questions:

Should we only check `__init__` method?

I checked pylint and seems like this is only triggered on `__init__` function.

```
class Unicorn:
    def __init__(self, fluffiness_level):
        pass

    def is_magical(self):
        self.fluffiness_level += 100
```

Note the no-member is a different error here.
```
test.py:6:8: E1101: Instance of 'Unicorn' has no 'fluffiness_level' member (no-member)
```

Should we only check for accesses where the definition is not in the same scope?
I checked an edge case in pylint where the accessed variable is in an inner function:

```
class Unicorn:
    def __init__(self, fluffiness_level):
        def f():
            self.fluffiness_level # this does not trigger the warning
        self.fluffiness_level = fluffiness_level
```

Rule source code in pylint:
https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/classes/class_checker.py#L1960

## Test Plan


Checked new ecosystem results:

- [securedrop/pretty_bad_protocol/_meta.py:174:42:](https://github.com/freedomofpress/securedrop/blob/7eab3b8d3efa03c893e4dcf548584d5e3b5d2f0a/securedrop/pretty_bad_protocol/_meta.py#L174) Seems ok
- 

---

_Renamed from "Add pylint E0203 rule" to "[pylint] - implement  access-member-before-definition / E0203 rule" by @Glyphack on 2024-01-14 09:46_

---

_@Glyphack reviewed on 2024-01-15 23:16_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/pylint/rules/access_member_before_definition.rs`:89 on 2024-01-15 23:16_

I feel this code should not be here because we calculate attributes every time.

---

_@charliermarsh reviewed on 2024-01-16 01:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/access_member_before_definition.rs`:89 on 2024-01-16 01:28_

I might suggest reframing the rule to run on each _scope_, and then check if the current scope is an `__init__ ` method. `unused_arguments` is one example of that pattern.

https://github.com/astral-sh/ruff/blob/da275b85725f501443fe73d5e2a5cf182a541881/crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs#L316

---

_@charliermarsh reviewed on 2024-01-16 01:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/access_member_before_definition.rs`:92 on 2024-01-16 01:29_

(Nit: you can do `stmt.start()` and `expr.start()`. Anything that has `.range()` automatically gets `.start()` and `.end()`.)

---

_@charliermarsh reviewed on 2024-01-16 01:30_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/access_member_before_definition.rs`:89 on 2024-01-16 01:30_

(Gonna hold off on reviewing all the logic below since some of it could change based on that refactor. But it would mean we analyze all assignments in one pass, rather than analyzing each attribute access.)

---

_@Glyphack reviewed on 2024-01-16 23:18_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/pylint/rules/access_member_before_definition.rs`:89 on 2024-01-16 23:18_

I changed to rule to run on every scope.

---

_@Glyphack reviewed on 2024-01-21 23:17_

I tried to implement the rule with minimal changes in the checker. I'm open to any feedback to change the implementation. I just wanted to get something working beforehand.

---

_@Glyphack reviewed on 2024-01-22 22:39_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/rules/flake8_builtins/snapshots/ruff_linter__rules__flake8_builtins__tests__A003_A003.py.snap`:13 on 2024-01-22 22:39_

The reason this is happening is that the self.id is shadowing class attribute `id` which shadows Python builtin ID. In the A003 rule we go over all the references of the binding that shadows the builtin, and in this case self.id is one reference to class attribute id so this warning is triggered.

I was thinking what should be the right thing to do here, we can do either of:

1. Not count self.id as a class attribute id shadowing so the self.id is a completely new binding. https://michaelgoerz.net/notes/a-note-on-python-class-attributes.html
2. When going over references in the rule exclude the instance attribute references. This would fix this rule but I don't want to leak a wrong data model to ruff and just fix the symptom.

---

_Comment by @codspeed-hq[bot] on 2024-01-22 22:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Glyphack:linter-pylint-E0203)

### Merging #9513 will **not alter performance**

<sub>Comparing <code>Glyphack:linter-pylint-E0203</code> (fb356e7) with <code>main</code> (549cc1e)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-01-22 23:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+41 -0 violations, +0 -0 fixes in 5 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/airflow/models/variable.py#L59'>airflow/models/variable.py:59:9:</a> F811 Redefinition of unused `val` from line 96
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/api_connexion/schemas/test_role_and_permission_schema.py#L47'>tests/api_connexion/schemas/test_role_and_permission_schema.py:47:9:</a> F811 Redefinition of unused `role` from line 34
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/providers/google/cloud/log/test_gcs_task_handler.py#L57'>tests/providers/google/cloud/log/test_gcs_task_handler.py:57:9:</a> F811 Redefinition of unused `gcs_task_handler` from line 55
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/utils/log/test_log_reader.py#L60'>tests/utils/log/test_log_reader.py:60:13:</a> F811 Redefinition of unused `log_dir` from line 58
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/utils/log/test_log_reader.py#L68'>tests/utils/log/test_log_reader.py:68:13:</a> F811 Redefinition of unused `settings_folder` from line 65
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/f8e76871c4c70ee9da4be6c13b37dc3a03e44265/Packs/Endace/Integrations/Endace/Endace_test.py#L69'>Packs/Endace/Integrations/Endace/Endace_test.py:69:13:</a> F811 Redefinition of unused `status_code` from line 74
+ <a href='https://github.com/demisto/content/blob/f8e76871c4c70ee9da4be6c13b37dc3a03e44265/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2_test.py#L64'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2_test.py:64:17:</a> F811 Redefinition of unused `parent` from line 59
+ <a href='https://github.com/demisto/content/blob/f8e76871c4c70ee9da4be6c13b37dc3a03e44265/Packs/qualys/Integrations/Qualysv2/Qualysv2_test.py#L971'>Packs/qualys/Integrations/Qualysv2/Qualysv2_test.py:971:9:</a> F811 Redefinition of unused `json` from line 975
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/f91fa3d9fc7262e0467e4b2f84fe463f8f8d23cf/_distutils_hack/__init__.py#L141'>_distutils_hack/__init__.py:141:9:</a> F811 Redefinition of unused `spec_for_distutils` from line 92
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/colors.py#L104'>indico/core/db/sqlalchemy/colors.py:104:26:</a> F811 Redefinition of unused `background_color` from line 87
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/colors.py#L104'>indico/core/db/sqlalchemy/colors.py:104:9:</a> F811 Redefinition of unused `text_color` from line 79
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L322'>indico/core/db/sqlalchemy/links.py:322:9:</a> F811 Redefinition of unused `category` from line 209
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:29:</a> F811 Redefinition of unused `event` from line 223
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:42:</a> F811 Redefinition of unused `session` from line 250
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:57:</a> F811 Redefinition of unused `session_block` from line 264
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:9:</a> F811 Redefinition of unused `linked_event` from line 235
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L324'>indico/core/db/sqlalchemy/links.py:324:29:</a> F811 Redefinition of unused `subcontribution` from line 292
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L324'>indico/core/db/sqlalchemy/links.py:324:9:</a> F811 Redefinition of unused `contribution` from line 278
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L184'>indico/core/db/sqlalchemy/locations.py:184:9:</a> F811 Redefinition of unused `own_venue` from line 149
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L198'>indico/core/db/sqlalchemy/locations.py:198:9:</a> F811 Redefinition of unused `own_room` from line 161
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L212'>indico/core/db/sqlalchemy/locations.py:212:9:</a> F811 Redefinition of unused `own_venue_name` from line 122
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L246'>indico/core/db/sqlalchemy/locations.py:246:9:</a> F811 Redefinition of unused `own_room_name` from line 131
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L262'>indico/core/db/sqlalchemy/locations.py:262:9:</a> F811 Redefinition of unused `own_address` from line 140
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L287'>indico/core/db/sqlalchemy/locations.py:287:9:</a> F811 Redefinition of unused `inherit_location` from line 91
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L389'>indico/core/db/sqlalchemy/principals.py:389:9:</a> F811 Redefinition of unused `type` from line 176
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L390'>indico/core/db/sqlalchemy/principals.py:390:9:</a> F811 Redefinition of unused `email` from line 228
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L391'>indico/core/db/sqlalchemy/principals.py:391:9:</a> F811 Redefinition of unused `user` from line 282
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L392'>indico/core/db/sqlalchemy/principals.py:392:9:</a> F811 Redefinition of unused `local_group` from line 295
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L393'>indico/core/db/sqlalchemy/principals.py:393:41:</a> F811 Redefinition of unused `multipass_group_name` from line 220
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L393'>indico/core/db/sqlalchemy/principals.py:393:9:</a> F811 Redefinition of unused `multipass_group_provider` from line 212
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L394'>indico/core/db/sqlalchemy/principals.py:394:9:</a> F811 Redefinition of unused `ip_network_group` from line 308
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L395'>indico/core/db/sqlalchemy/principals.py:395:9:</a> F811 Redefinition of unused `event_role` from line 323
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L396'>indico/core/db/sqlalchemy/principals.py:396:9:</a> F811 Redefinition of unused `category_role` from line 338
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L397'>indico/core/db/sqlalchemy/principals.py:397:9:</a> F811 Redefinition of unused `registration_form` from line 353
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L206'>indico/core/storage/models.py:206:9:</a> F811 Redefinition of unused `storage_backend` from line 145
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L207'>indico/core/storage/models.py:207:31:</a> F811 Redefinition of unused `md5` from line 134
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L207'>indico/core/storage/models.py:207:9:</a> F811 Redefinition of unused `storage_file_id` from line 152
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L208'>indico/core/storage/models.py:208:9:</a> F811 Redefinition of unused `size` from line 123
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/modules/events/models/persons.py#L552'>indico/modules/events/models/persons.py:552:9:</a> F811 Redefinition of unused `person` from line 492
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/modules/events/models/persons.py#L59'>indico/modules/events/models/persons.py:59:9:</a> F811 Redefinition of unused `person_links` from line 31
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/f85289ba872a26eb41836cdaefe97a93b311bf29/src/_pytest/junitxml.py#L265'>src/_pytest/junitxml.py:265:9:</a> F811 Redefinition of unused `to_xml` from line 151
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 41 | 41 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+46 -0 violations, +0 -0 fixes in 7 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c39cceefdbb32294362089e5b741b309e3e73771/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L141'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:141:46:</a> PLE0203 Accessed `_is_adaptive_time_step` before it was initialized at line 219
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c39cceefdbb32294362089e5b741b309e3e73771/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L153'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:153:17:</a> PLE0203 Accessed `_require_synchronized_time` before it was initialized at line 201
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c39cceefdbb32294362089e5b741b309e3e73771/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L153'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:153:57:</a> PLE0203 Accessed `_is_synchronized_time_step` before it was initialized at line 218
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/airflow/models/variable.py#L59'>airflow/models/variable.py:59:9:</a> F811 Redefinition of unused `val` from line 96
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/api_connexion/schemas/test_role_and_permission_schema.py#L47'>tests/api_connexion/schemas/test_role_and_permission_schema.py:47:9:</a> F811 Redefinition of unused `role` from line 34
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/providers/google/cloud/log/test_gcs_task_handler.py#L57'>tests/providers/google/cloud/log/test_gcs_task_handler.py:57:9:</a> F811 Redefinition of unused `gcs_task_handler` from line 55
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/utils/log/test_log_reader.py#L60'>tests/utils/log/test_log_reader.py:60:13:</a> F811 Redefinition of unused `log_dir` from line 58
+ <a href='https://github.com/apache/airflow/blob/fc4fbb3dcf1f2c241ed65b564214aaf853d14a9a/tests/utils/log/test_log_reader.py#L68'>tests/utils/log/test_log_reader.py:68:13:</a> F811 Redefinition of unused `settings_folder` from line 65
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/f8e76871c4c70ee9da4be6c13b37dc3a03e44265/Packs/Endace/Integrations/Endace/Endace_test.py#L69'>Packs/Endace/Integrations/Endace/Endace_test.py:69:13:</a> F811 Redefinition of unused `status_code` from line 74
+ <a href='https://github.com/demisto/content/blob/f8e76871c4c70ee9da4be6c13b37dc3a03e44265/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2_test.py#L64'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2_test.py:64:17:</a> F811 Redefinition of unused `parent` from line 59
+ <a href='https://github.com/demisto/content/blob/f8e76871c4c70ee9da4be6c13b37dc3a03e44265/Packs/qualys/Integrations/Qualysv2/Qualysv2_test.py#L971'>Packs/qualys/Integrations/Qualysv2/Qualysv2_test.py:971:9:</a> F811 Redefinition of unused `json` from line 975
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/securedrop/pretty_bad_protocol/_meta.py#L174'>securedrop/pretty_bad_protocol/_meta.py:174:42:</a> PLE0203 Accessed `_homedir` before it was initialized at line 423
+ <a href='https://github.com/freedomofpress/securedrop/blob/59be919d6bc0487344952b8436f7efb0ec6d3806/securedrop/pretty_bad_protocol/_meta.py#L175'>securedrop/pretty_bad_protocol/_meta.py:175:42:</a> PLE0203 Accessed `_homedir` before it was initialized at line 423
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/f91fa3d9fc7262e0467e4b2f84fe463f8f8d23cf/_distutils_hack/__init__.py#L141'>_distutils_hack/__init__.py:141:9:</a> F811 Redefinition of unused `spec_for_distutils` from line 92
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/colors.py#L104'>indico/core/db/sqlalchemy/colors.py:104:26:</a> F811 Redefinition of unused `background_color` from line 87
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/colors.py#L104'>indico/core/db/sqlalchemy/colors.py:104:9:</a> F811 Redefinition of unused `text_color` from line 79
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L322'>indico/core/db/sqlalchemy/links.py:322:9:</a> F811 Redefinition of unused `category` from line 209
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:29:</a> F811 Redefinition of unused `event` from line 223
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:42:</a> F811 Redefinition of unused `session` from line 250
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:57:</a> F811 Redefinition of unused `session_block` from line 264
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L323'>indico/core/db/sqlalchemy/links.py:323:9:</a> F811 Redefinition of unused `linked_event` from line 235
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L324'>indico/core/db/sqlalchemy/links.py:324:29:</a> F811 Redefinition of unused `subcontribution` from line 292
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/links.py#L324'>indico/core/db/sqlalchemy/links.py:324:9:</a> F811 Redefinition of unused `contribution` from line 278
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L184'>indico/core/db/sqlalchemy/locations.py:184:9:</a> F811 Redefinition of unused `own_venue` from line 149
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L198'>indico/core/db/sqlalchemy/locations.py:198:9:</a> F811 Redefinition of unused `own_room` from line 161
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L212'>indico/core/db/sqlalchemy/locations.py:212:9:</a> F811 Redefinition of unused `own_venue_name` from line 122
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L246'>indico/core/db/sqlalchemy/locations.py:246:9:</a> F811 Redefinition of unused `own_room_name` from line 131
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L262'>indico/core/db/sqlalchemy/locations.py:262:9:</a> F811 Redefinition of unused `own_address` from line 140
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/locations.py#L287'>indico/core/db/sqlalchemy/locations.py:287:9:</a> F811 Redefinition of unused `inherit_location` from line 91
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L389'>indico/core/db/sqlalchemy/principals.py:389:9:</a> F811 Redefinition of unused `type` from line 176
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L390'>indico/core/db/sqlalchemy/principals.py:390:9:</a> F811 Redefinition of unused `email` from line 228
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L391'>indico/core/db/sqlalchemy/principals.py:391:9:</a> F811 Redefinition of unused `user` from line 282
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L392'>indico/core/db/sqlalchemy/principals.py:392:9:</a> F811 Redefinition of unused `local_group` from line 295
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L393'>indico/core/db/sqlalchemy/principals.py:393:41:</a> F811 Redefinition of unused `multipass_group_name` from line 220
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L393'>indico/core/db/sqlalchemy/principals.py:393:9:</a> F811 Redefinition of unused `multipass_group_provider` from line 212
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L394'>indico/core/db/sqlalchemy/principals.py:394:9:</a> F811 Redefinition of unused `ip_network_group` from line 308
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L395'>indico/core/db/sqlalchemy/principals.py:395:9:</a> F811 Redefinition of unused `event_role` from line 323
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L396'>indico/core/db/sqlalchemy/principals.py:396:9:</a> F811 Redefinition of unused `category_role` from line 338
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/db/sqlalchemy/principals.py#L397'>indico/core/db/sqlalchemy/principals.py:397:9:</a> F811 Redefinition of unused `registration_form` from line 353
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L206'>indico/core/storage/models.py:206:9:</a> F811 Redefinition of unused `storage_backend` from line 145
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L207'>indico/core/storage/models.py:207:31:</a> F811 Redefinition of unused `md5` from line 134
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L207'>indico/core/storage/models.py:207:9:</a> F811 Redefinition of unused `storage_file_id` from line 152
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/core/storage/models.py#L208'>indico/core/storage/models.py:208:9:</a> F811 Redefinition of unused `size` from line 123
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/modules/events/models/persons.py#L552'>indico/modules/events/models/persons.py:552:9:</a> F811 Redefinition of unused `person` from line 492
+ <a href='https://github.com/indico/indico/blob/6a2ccc782fcc2506efcc8456eea669b42a4d8bc6/indico/modules/events/models/persons.py#L59'>indico/modules/events/models/persons.py:59:9:</a> F811 Redefinition of unused `person_links` from line 31
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/f85289ba872a26eb41836cdaefe97a93b311bf29/src/_pytest/junitxml.py#L265'>src/_pytest/junitxml.py:265:9:</a> F811 Redefinition of unused `to_xml` from line 151
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 41 | 41 | 0 | 0 | 0 |
| PLE0203 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @Glyphack on 2024-01-24 23:40_

---

_Converted to draft by @Glyphack on 2024-01-27 16:32_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/checkers/ast/mod.rs`:1917 on 2024-01-28 21:25_

I'm not sure if this is the best way to do this. Let me know if I can improve this part.

---

_Review comment by @Glyphack on `crates/ruff_python_semantic/src/model.rs`:1934 on 2024-01-28 21:28_

I added a new unresolved attributes. This is similar to how unresolved references is populated

The reason I decided to create a new unresolved attributes was that for attributes we need the scope they happened in to report them correctly.

Imagine there are two classes and one of them has unresolved attribute x. We add this attribute name to the unresolved attributes and the lint rule runs in both classes. In the first class the rule emits error which is fine. But when the lint rule runs on the second class, it iterates over the unresolved attributes again and emits the error, because the x was in the attributes references and it's defined.

```
class A:
    def __init__(self):
        print(self.x)
	self.x = 1

class B:
    def __init__(self):
        self.x = 1
	print(self.x)
```

---

_Marked ready for review by @Glyphack on 2024-01-28 21:34_

---

_Review requested from @charliermarsh by @Glyphack on 2024-01-28 21:52_

---

_Comment by @Glyphack on 2024-03-09 21:07_

Hi, should I rebase this PR with main? Not sure if you want to move forward with it.

---

_@MichaReiser reviewed on 2024-03-20 09:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/access_member_before_definition.rs`:22 on 2024-03-20 09:31_

I think the example here is incorrect
```suggestion
///     def __init__(self, x):
///         if self.x > 9000:  # [access-member-before-definition]
///             pass
///         self.x = x
```

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:16_

---

_Review comment by @Glyphack on `crates/ruff_linter/src/checkers/ast/mod.rs`:1158 on 2024-06-09 15:49_

I don't think anything should happen on these cases.

---

_@Glyphack reviewed on 2024-06-09 15:49_

---

_Comment by @Glyphack on 2024-06-09 15:51_

I realized this was marked as accepted so I rebased with main.

---

_Comment by @Glyphack on 2024-11-10 11:02_

I'm closing this PR. red knot can identify this so hopefully soon using that.

---

_Closed by @Glyphack on 2024-11-10 11:02_

---

_Branch deleted on 2025-05-21 20:26_

---
