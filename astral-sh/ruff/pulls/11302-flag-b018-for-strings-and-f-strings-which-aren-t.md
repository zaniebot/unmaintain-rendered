```yaml
number: 11302
title: "Flag `B018` for strings and f-strings which aren't docstrings"
type: pull_request
state: open
author: dhruvmanila
labels:
  - bug
assignees: []
draft: true
base: main
head: dhruv/b018-string
created_at: 2024-05-06T06:54:18Z
updated_at: 2025-03-14T09:22:32Z
url: https://github.com/astral-sh/ruff/pull/11302
synced_at: 2026-01-10T19:49:01Z
```

# Flag `B018` for strings and f-strings which aren't docstrings

---

_Pull request opened by @dhruvmanila on 2024-05-06 06:54_

## Summary

This PR updates the `B018` heuristic to consider standalone string literals, except for those used as docstring and attribute docstring, as useless expression.

fixes: #11292 

## Test Plan

Add a test file full of attribute docstring and verified the snapshot.


---

_Label `bug` added by @dhruvmanila on 2024-05-06 06:54_

---

_Comment by @github-actions[bot] on 2024-05-06 07:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3446 -0 violations, +0 -0 fixes in 16 projects; 34 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/async_migrations/runner.py#L19'>housewatch/async_migrations/runner.py:19:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/a933be36cc8d20f7fe0af13f1621bacb29b7db8e/docs/_cff_to_rst.py#L196'>docs/_cff_to_rst.py:196:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/a933be36cc8d20f7fe0af13f1621bacb29b7db8e/src/plasmapy/formulary/dielectric.py#L31'>src/plasmapy/formulary/dielectric.py:31:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/a933be36cc8d20f7fe0af13f1621bacb29b7db8e/tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py#L162'>tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py:162:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/a933be36cc8d20f7fe0af13f1621bacb29b7db8e/tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py#L777'>tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py:777:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/a933be36cc8d20f7fe0af13f1621bacb29b7db8e/tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py#L891'>tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py:891:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/a933be36cc8d20f7fe0af13f1621bacb29b7db8e/tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py#L939'>tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py:939:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+46 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/api/auth/backend/kerberos_auth.py#L48'>airflow/api/auth/backend/kerberos_auth.py:48:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/migrations/versions/0115_2_4_0_remove_smart_sensors.py#L47'>airflow/migrations/versions/0115_2_4_0_remove_smart_sensors.py:47:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/models/dag.py#L4121'>airflow/models/dag.py:4121:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/models/dagrun.py#L574'>airflow/models/dagrun.py:574:9:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/providers/amazon/aws/hooks/lambda_function.py#L168'>airflow/providers/amazon/aws/hooks/lambda_function.py:168:9:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/providers/amazon/aws/sensors/s3.py#L112'>airflow/providers/amazon/aws/sensors/s3.py:112:9:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/providers/celery/executors/celery_executor.py#L91'>airflow/providers/celery/executors/celery_executor.py:91:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L214'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:214:9:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/providers/fab/auth_manager/models/__init__.py#L47'>airflow/providers/fab/auth_manager/models/__init__.py:47:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/apache/airflow/blob/79cf7e0609ef0640088ee80117e95046fa48063b/airflow/providers/fab/auth_manager/security_manager/override.py#L1396'>airflow/providers/fab/auth_manager/security_manager/override.py:1396:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_serialization.py#L864'>tests/unit/bokeh/core/test_serialization.py:864:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/test_notebook__embed.py#L42'>tests/unit/bokeh/embed/test_notebook__embed.py:42:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+3299 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AHA/Integrations/AHA/AHA.py#L111'>Packs/AHA/Integrations/AHA/AHA.py:111:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AHA/Integrations/AHA/AHA.py#L12'>Packs/AHA/Integrations/AHA/AHA.py:12:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AHA/Integrations/AHA/AHA.py#L163'>Packs/AHA/Integrations/AHA/AHA.py:163:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AHA/Integrations/AHA/AHA.py#L234'>Packs/AHA/Integrations/AHA/AHA.py:234:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AHA/Integrations/AHA/AHA.py#L283'>Packs/AHA/Integrations/AHA/AHA.py:283:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AHA/Integrations/AHA/AHA.py#L52'>Packs/AHA/Integrations/AHA/AHA.py:52:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/AMPv2/AMPv2.py#L1164'>Packs/AMP/Integrations/AMPv2/AMPv2.py:1164:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/AMPv2/AMPv2.py#L14'>Packs/AMP/Integrations/AMPv2/AMPv2.py:14:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3220'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3220:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3703'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3703:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/CiscoAMPEventCollector/CiscoAMPEventCollector.py#L194'>Packs/AMP/Integrations/CiscoAMPEventCollector/CiscoAMPEventCollector.py:194:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/CiscoAMPEventCollector/CiscoAMPEventCollector.py#L236'>Packs/AMP/Integrations/CiscoAMPEventCollector/CiscoAMPEventCollector.py:236:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/AMP/Integrations/CiscoAMPEventCollector/CiscoAMPEventCollector.py#L9'>Packs/AMP/Integrations/CiscoAMPEventCollector/CiscoAMPEventCollector.py:9:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L15'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:15:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L60'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:60:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L6'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:6:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L708'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:708:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L904'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:904:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/APIVoid/Integrations/APIVoid/APIVoid.py#L14'>Packs/APIVoid/Integrations/APIVoid/APIVoid.py:14:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/APIVoid/Integrations/APIVoid/APIVoid.py#L6'>Packs/APIVoid/Integrations/APIVoid/APIVoid.py:6:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L115'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:115:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L1288'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:1288:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L139'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:139:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L159'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:159:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L15'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:15:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L2047'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:2047:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L2081'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:2081:1:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L222'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:222:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L238'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:238:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L295'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:295:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L32'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:32:9:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L353'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:353:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L37'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:37:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L44'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:44:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L522'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:522:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L573'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:573:13:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L623'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:623:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L67'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:67:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L699'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:699:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L751'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:751:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L772'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:772:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L783'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:783:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L799'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:799:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L812'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:812:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L830'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:830:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
+ <a href='https://github.com/demisto/content/blob/3df2caa171ae91ccb269043ade19919d178e486b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L845'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:845:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
... 3252 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B018 | 3446 | 3446 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @dhruvmanila on 2024-05-06 07:38_

Going through the ecosystem changes, it seems like most of the violations is because the string is used as a docstring for a variable or a class attribute. While for [`demisto/content`](https://github.com/demisto/content), at least the ones I opened, used strings as a header which should instead be a comment.

---

_Comment by @dhruvmanila on 2024-05-06 07:41_

Oh, I just understood what Charlie meant by https://github.com/astral-sh/ruff/issues/11292#issuecomment-2094987829, I'll see on to avoid flagging these. I think one way would be to add a `ATTRIBUTE_DOCSTRING` in the semantic model.

---

_Comment by @charliermarsh on 2024-05-06 13:31_

Yeah we should not flag those. Thanks Dhruv!

---

_Marked ready for review by @dhruvmanila on 2024-05-10 11:23_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-10 11:23_

---

_Comment by @dhruvmanila on 2024-05-10 11:32_

Still quite a lot of usages of triple-quoted strings which could be comments instead. This makes me wonder if this should be behind preview?

---

_Comment by @charliermarsh on 2024-05-16 02:09_

I do think it should probably be in preview.

---

_@charliermarsh reviewed on 2024-05-16 02:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_expression.rs`:77 on 2024-05-16 02:10_

By the way, we could flag `EllipsisLiteral` I think? As long as we ignore stub methods...

---

_@charliermarsh reviewed on 2024-05-16 02:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_expression.rs`:77 on 2024-05-16 02:10_

But lets ignore that for this PR.

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-16 03:31_

---

_Comment by @adam-sikora on 2024-12-16 13:49_

What is holding this PR from being merged? Are you still debating whether this should be a separate rule or be under `B018`? Are there any unresolved edge cases?

If any help is needed to merge this I would like to contribute.

This is a very useful rule and it is the last one that forces us to use Pylint alongside Ruff.

---

_Comment by @dhruvmanila on 2024-12-17 04:49_

> What is holding this PR from being merged? Are you still debating whether this should be a separate rule or be under `B018`? Are there any unresolved edge cases?

It's mainly on me, I can find some time this week to move this forward.

---

_Comment by @adam-sikora on 2025-01-24 11:25_

@dhruvmanila any news about this PR?

Again I'm happy to help if any assistance is needed.

---

_Converted to draft by @dhruvmanila on 2025-02-19 05:44_

---

_Comment by @MichaReiser on 2025-03-14 09:22_

@dhruvmanila do you think this is a med/high impact change? If so, is it something that @dylwil3 or @ntBre could help you with?

---
