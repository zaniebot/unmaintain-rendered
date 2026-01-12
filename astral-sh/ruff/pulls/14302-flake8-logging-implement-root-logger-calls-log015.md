```yaml
number: 14302
title: "[`flake8-logging`] Implement `root-logger-calls` (`LOG015`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: LOG015
created_at: 2024-11-13T00:31:35Z
updated_at: 2024-11-15T10:33:10Z
url: https://github.com/astral-sh/ruff/pull/14302
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-logging`] Implement `root-logger-calls` (`LOG015`)

---

_@InSyncWithFoo_

## Summary

Part of #7248.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-13 00:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+284 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+52 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-05-27_15-03_1226819ee0e3_fix_wrong_constraint_on_table_columns.py#L64'>superset/migrations/versions/2016-05-27_15-03_1226819ee0e3_fix_wrong_constraint_on_table_columns.py:64:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-15_08-48_65903709c321_allow_dml.py#L43'>superset/migrations/versions/2016-09-15_08-48_65903709c321_allow_dml.py:43:9:</a> LOG015 `exception()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L116'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:116:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L128'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:128:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L135'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:135:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L63'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:63:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L72'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:72:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L77'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:77:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L83'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:83:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/apache/superset/blob/4e8eba802a179605405c0c05f906abdbd4f95d1e/superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py#L91'>superset/migrations/versions/2016-09-22_10-21_3b626e2a6783_sync_db_with_models.py:91:9:</a> LOG015 `warning()` call on root logger
... 42 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L33'>scripts/milestone.py:33:9:</a> LOG015 `debug()` call on root logger
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L41'>scripts/milestone.py:41:9:</a> LOG015 `debug()` call on root logger
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L377'>src/bokeh/document/document.py:377:17:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L581'>src/bokeh/io/notebook.py:581:5:</a> LOG015 `debug()` call on root logger
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L582'>src/bokeh/io/notebook.py:582:5:</a> LOG015 `debug()` call on root logger
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/397130c084872c56adcdc2a8624f4415641fca55/rotkehlchen/__main__.py#L28'>rotkehlchen/__main__.py:28:13:</a> LOG015 `critical()` call on root logger
+ <a href='https://github.com/rotki/rotki/blob/397130c084872c56adcdc2a8624f4415641fca55/rotkehlchen/__main__.py#L33'>rotkehlchen/__main__.py:33:9:</a> LOG015 `critical()` call on root logger
+ <a href='https://github.com/rotki/rotki/blob/397130c084872c56adcdc2a8624f4415641fca55/rotkehlchen/exchanges/htx.py#L183'>rotkehlchen/exchanges/htx.py:183:21:</a> LOG015 `debug()` call on root logger
+ <a href='https://github.com/rotki/rotki/blob/397130c084872c56adcdc2a8624f4415641fca55/rotkehlchen/logging.py#L66'>rotkehlchen/logging.py:66:9:</a> LOG015 `log()` call on root logger
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+223 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/analytics/views/stats.py#L485'>analytics/views/stats.py:485:13:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/scripts/lib/setup_venv.py#L203'>scripts/lib/setup_venv.py:203:13:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/tools/lib/provision.py#L90'>tools/lib/provision.py:90:5:</a> LOG015 `critical()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/create_realm.py#L194'>zerver/actions/create_realm.py:194:9:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/invites.py#L117'>zerver/actions/invites.py:117:5:</a> LOG015 `log()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/message_send.py#L1901'>zerver/actions/message_send.py:1901:9:</a> LOG015 `exception()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/message_send.py#L516'>zerver/actions/message_send.py:516:13:</a> LOG015 `error()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/realm_settings.py#L574'>zerver/actions/realm_settings.py:574:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/scheduled_messages.py#L378'>zerver/actions/scheduled_messages.py:378:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/scheduled_messages.py#L395'>zerver/actions/scheduled_messages.py:395:17:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/scheduled_messages.py#L400'>zerver/actions/scheduled_messages.py:400:17:</a> LOG015 `exception()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/actions/uploads.py#L61'>zerver/actions/uploads.py:61:13:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/apps.py#L12'>zerver/apps.py:12:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L589'>zerver/data_import/import_util.py:589:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L590'>zerver/data_import/import_util.py:590:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L621'>zerver/data_import/import_util.py:621:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L632'>zerver/data_import/import_util.py:632:9:</a> LOG015 `exception()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L638'>zerver/data_import/import_util.py:638:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L646'>zerver/data_import/import_util.py:646:17:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L670'>zerver/data_import/import_util.py:670:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L671'>zerver/data_import/import_util.py:671:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L682'>zerver/data_import/import_util.py:682:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L723'>zerver/data_import/import_util.py:723:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L724'>zerver/data_import/import_util.py:724:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L744'>zerver/data_import/import_util.py:744:13:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/import_util.py#L767'>zerver/data_import/import_util.py:767:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/mattermost.py#L245'>zerver/data_import/mattermost.py:245:17:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/mattermost.py#L695'>zerver/data_import/mattermost.py:695:9:</a> LOG015 `warning()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/mattermost.py#L750'>zerver/data_import/mattermost.py:750:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/mattermost.py#L801'>zerver/data_import/mattermost.py:801:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L277'>zerver/data_import/rocketchat.py:277:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L334'>zerver/data_import/rocketchat.py:334:5:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L391'>zerver/data_import/rocketchat.py:391:9:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L395'>zerver/data_import/rocketchat.py:395:9:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L409'>zerver/data_import/rocketchat.py:409:9:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L413'>zerver/data_import/rocketchat.py:413:9:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L662'>zerver/data_import/rocketchat.py:662:13:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L729'>zerver/data_import/rocketchat.py:729:17:</a> LOG015 `info()` call on root logger
+ <a href='https://github.com/zulip/zulip/blob/048387da056641fa3cea04c01ee6179c385323c9/zerver/data_import/rocketchat.py#L766'>zerver/data_import/rocketchat.py:766:21:</a> LOG015 `info()` call on root logger
... 184 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG015 | 284 | 284 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-11-13 08:48_

---

_Label `preview` added by @MichaReiser on 2024-11-13 08:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_logging/LOG015.py`:46 on 2024-11-13 08:49_

Nit: Can we add a test where the logger is also called logging
```suggestion
logger = logging.getLogger("")

logger.debug("Lorem")
logger.info("ipsum")
logger.warn("dolor")
logger.warning("sit")
logger.error("amet")
logger.critical("consectetur")
logger.log("adipiscing")
logger.exception("elit.")


logging = logging.getLogger("")

logging.debug("Lorem")
logging.info("ipsum")
logging.warn("dolor")
logging.warning("sit")
logging.error("amet")
logging.critical("consectetur")
logging.log("adipiscing")
logging.exception("elit.")
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/root_logger_call.rs`:13 on 2024-11-13 08:50_

```suggestion
/// Using the root logger causes the messages to have no source information,
```

---

_@MichaReiser approved on 2024-11-13 08:50_

This is great. Did you review the ecosystem changes? 

---

_Comment by @InSyncWithFoo on 2024-11-13 09:07_

> Did you review the ecosystem changes?

I did. They all seem to be true positives.

---

_Merged by @MichaReiser on 2024-11-13 09:11_

---

_Closed by @MichaReiser on 2024-11-13 09:11_

---

_Branch deleted on 2024-11-13 09:42_

---

_Renamed from "[`flake8-logging`] Root logger calls (`LOG015`)" to "[`flake8-logging`] Implement `root-logger-calls` (`LOG015`)" by @dhruvmanila on 2024-11-15 10:33_

---
