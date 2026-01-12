```yaml
number: 4558
title: Change TODO directive detection to work with multiple pound signs on the same line
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 4549_flake8_todo_falseneg
created_at: 2023-05-21T03:59:04Z
updated_at: 2023-05-25T17:12:11Z
url: https://github.com/astral-sh/ruff/pull/4558
synced_at: 2026-01-12T15:55:15Z
```

# Change TODO directive detection to work with multiple pound signs on the same line

---

_@evanrittenhouse_

Should fix #4549. 

Basically, the initial implementation of `flake8-todos` hinged on a regex anchoring the `#` in a Python comment to the start of a line. If no TODO tags were found just after that `#`, the line would not throw a diagnostic. 

In the case of something like `# noqa: A003 # TODO: something`, the TODO is actually demarcated by a second `#`. As such, we need to check the characters after each `#` in the comment in order to find `Directives`.

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules.rs`:245 on 2023-05-21 04:00_

Is there a more idiomatic way to do this? I think we have to move away from match since the first comment (#-delimited section) could be of variable length, but not sure if this if block is a best practice. 

---

_@evanrittenhouse reviewed on 2023-05-21 04:00_

---

_Marked ready for review by @evanrittenhouse on 2023-05-21 04:04_

---

_Comment by @github-actions[bot] on 2023-05-21 04:09_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+95, -105, 0 error(s))

<details><summary>airflow (+15, -11)</summary>
<p>

```diff
- airflow/cli/commands/task_command.py:456:7: TD005 Missing issue description after `TODO`
- airflow/cli/commands/task_command.py:663:11: TD005 Missing issue description after `TODO`
- airflow/decorators/base.py:216:11: TD005 Missing issue description after `TODO`
- airflow/jobs/local_task_job_runner.py:79:41: TD005 Missing issue description after `TODO`
+ airflow/jobs/scheduler_job_runner.py:1183:11: TD004 Missing colon in TODO
+ airflow/jobs/scheduler_job_runner.py:1427:11: TD004 Missing colon in TODO
+ airflow/jobs/scheduler_job_runner.py:402:15: TD004 Missing colon in TODO
+ airflow/jobs/scheduler_job_runner.py:592:19: TD004 Missing colon in TODO
- airflow/models/baseoperator.py:1235:40: TD005 Missing issue description after `TODO`
- airflow/models/dag.py:1477:19: TD005 Missing issue description after `TODO`
+ airflow/models/dagrun.py:1184:15: TD004 Missing colon in TODO
+ airflow/providers/google/cloud/sensors/tasks.py:80:11: TD004 Missing colon in TODO
- airflow/providers/google/cloud/sensors/tasks.py:80:11: TD007 Missing space after colon in TODO
- airflow/providers/jenkins/operators/jenkins_job_trigger.py:153:11: TD005 Missing issue description after `TODO`
- airflow/task/task_runner/cgroup_task_runner.py:171:11: TD005 Missing issue description after `TODO`
- airflow/www/views.py:4091:11: TD005 Missing issue description after `TODO`
+ airflow/www/views.py:5196:22: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:5196:22: TD003 Missing issue link on the line following this TODO
+ airflow/www/views.py:5196:22: TD006 [*] Invalid TODO capitalization: `todo` should be `TODO`
+ airflow/www/views.py:5223:21: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:5223:21: TD003 Missing issue link on the line following this TODO
+ airflow/www/views.py:5223:21: TD006 [*] Invalid TODO capitalization: `todo` should be `TODO`
+ airflow/www/views.py:5594:22: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:5594:22: TD003 Missing issue link on the line following this TODO
+ airflow/www/views.py:5594:22: TD006 [*] Invalid TODO capitalization: `todo` should be `TODO`
- tests/utils/test_config.py:27:3: TD005 Missing issue description after `TODO`
```

</p>
</details>
<details><summary>bokeh (+62, -84)</summary>
<p>

```diff
- release/credentials.py:77:7: TD005 Missing issue description after `TODO`
- release/credentials.py:84:7: TD005 Missing issue description after `TODO`
- src/bokeh/application/application.py:187:15: TD005 Missing issue description after `TODO`
- src/bokeh/application/handlers/code.py:168:11: TD005 Missing issue description after `TODO`
- src/bokeh/application/handlers/directory.py:313:15: TD005 Missing issue description after `TODO`
- src/bokeh/application/handlers/server_lifecycle.py:127:15: TD005 Missing issue description after `TODO`
- src/bokeh/application/handlers/server_request_handler.py:120:15: TD005 Missing issue description after `TODO`
- src/bokeh/client/connection.py:349:19: TD005 Missing issue description after `TODO`
+ src/bokeh/core/has_props.py:440:99: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:440:99: TD003 Missing issue link on the line following this TODO
- src/bokeh/core/has_props.py:630:15: TD005 Missing issue description after `TODO`
- src/bokeh/core/property/wrappers.py:466:11: TD005 Missing issue description after `TODO`
- src/bokeh/core/property_mixins.py:374:41: TD005 Missing issue description after `TODO`
- src/bokeh/document/document.py:401:11: TD005 Missing issue description after `TODO`
- src/bokeh/document/models.py:241:7: TD005 Missing issue description after `TODO`
+ src/bokeh/embed/standalone.py:137:79: TD001 Invalid TODO tag: `XXX`
+ src/bokeh/embed/standalone.py:137:79: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:137:79: TD003 Missing issue link on the line following this TODO
+ src/bokeh/embed/standalone.py:144:89: TD001 Invalid TODO tag: `XXX`
+ src/bokeh/embed/standalone.py:144:89: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:144:89: TD003 Missing issue link on the line following this TODO
+ src/bokeh/embed/standalone.py:151:90: TD001 Invalid TODO tag: `XXX`
+ src/bokeh/embed/standalone.py:151:90: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:151:90: TD003 Missing issue link on the line following this TODO
+ src/bokeh/embed/standalone.py:255:61: TD001 Invalid TODO tag: `XXX`
+ src/bokeh/embed/standalone.py:255:61: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:255:61: TD003 Missing issue link on the line following this TODO
- src/bokeh/io/showing.py:148:48: TD005 Missing issue description after `TODO`
+ src/bokeh/layouts.py:303:3: TD004 Missing colon in TODO
- src/bokeh/layouts.py:303:3: TD007 Missing space after colon in TODO
- src/bokeh/models/map_plots.py:172:7: TD005 Missing issue description after `TODO`
- src/bokeh/models/sources.py:224:11: TD005 Missing issue description after `TODO`
- src/bokeh/models/tools.py:884:7: TD005 Missing issue description after `TODO`
- src/bokeh/plotting/_docstring.py:107:7: TD005 Missing issue description after `TODO`
+ src/bokeh/server/tornado.py:432:86: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:432:86: TD003 Missing issue link on the line following this TODO
+ src/bokeh/server/tornado.py:449:82: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:449:82: TD003 Missing issue link on the line following this TODO
+ src/bokeh/server/tornado.py:452:78: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:452:78: TD003 Missing issue link on the line following this TODO
+ src/bokeh/server/tornado.py:458:67: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:458:67: TD003 Missing issue link on the line following this TODO
- src/bokeh/server/tornado.py:653:11: TD005 Missing issue description after `TODO`
- src/bokeh/sphinxext/util.py:47:3: TD005 Missing issue description after `TODO`
- src/bokeh/util/callback_manager.py:61:3: TD005 Missing issue description after `TODO`
- src/bokeh/util/serialization.py:202:7: TD005 Missing issue description after `TODO`
- src/bokeh/util/serialization.py:239:7: TD005 Missing issue description after `TODO`
+ src/typings/IPython/core/history.pyi:5:89: TD001 Invalid TODO tag: `XXX`
+ src/typings/IPython/core/history.pyi:5:89: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/typings/IPython/core/history.pyi:5:89: TD003 Missing issue link on the line following this TODO
- tests/integration/tools/test_range_tool.py:36:3: TD005 Missing issue description after `TODO`
- tests/integration/tools/test_range_tool.py:37:3: TD005 Missing issue description after `TODO`
- tests/integration/tools/test_reset_tool.py:104:11: TD005 Missing issue description after `TODO`
- tests/integration/tools/test_reset_tool.py:125:61: TD005 Missing issue description after `TODO`
- tests/integration/tools/test_tap_tool.py:34:3: TD007 Missing space after colon in TODO
+ tests/integration/widgets/tables/test_cell_editors.py:101:13: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/integration/widgets/tables/test_cell_editors.py:101:13: TD003 Missing issue link on the line following this TODO
+ tests/integration/widgets/tables/test_cell_editors.py:101:13: TD004 Missing colon in TODO
+ tests/integration/widgets/tables/test_cell_editors.py:101:13: TD005 Missing issue description after `TODO`
- tests/integration/widgets/tables/test_cell_editors.py:351:3: TD005 Missing issue description after `TODO`
- tests/integration/widgets/tables/test_cell_editors.py:59:3: TD005 Missing issue description after `TODO`
+ tests/integration/widgets/tables/test_cell_editors.py:79:13: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/integration/widgets/tables/test_cell_editors.py:79:13: TD003 Missing issue link on the line following this TODO
+ tests/integration/widgets/tables/test_cell_editors.py:79:13: TD004 Missing colon in TODO
+ tests/integration/widgets/tables/test_cell_editors.py:79:13: TD005 Missing issue description after `TODO`
- tests/integration/widgets/tables/test_copy_paste.py:143:11: TD005 Missing issue description after `TODO`
+ tests/integration/widgets/test_color_picker.py:107:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_color_picker.py:107:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_daterange_slider.py:160:13: TD001 Invalid TODO tag: `XXX`
+ tests/integration/widgets/test_daterange_slider.py:160:13: TD003 Missing issue link on the line following this TODO
+ tests/integration/widgets/test_daterange_slider.py:160:13: TD004 Missing colon in TODO
+ tests/integration/widgets/test_daterange_slider.py:169:13: TD001 Invalid TODO tag: `XXX`
+ tests/integration/widgets/test_daterange_slider.py:169:13: TD003 Missing issue link on the line following this TODO
+ tests/integration/widgets/test_daterange_slider.py:169:13: TD004 Missing colon in TODO
+ tests/integration/widgets/test_daterange_slider.py:192:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_daterange_slider.py:192:11: TD007 Missing space after colon in TODO
- tests/integration/widgets/test_dateslider.py:154:11: TD005 Missing issue description after `TODO`
+ tests/integration/widgets/test_dateslider.py:163:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_dateslider.py:163:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_dateslider.py:197:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_dateslider.py:197:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_dateslider.py:220:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_dateslider.py:220:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_dateslider.py:243:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_dateslider.py:243:11: TD007 Missing space after colon in TODO
- tests/integration/widgets/test_dropdown.py:42:3: TD005 Missing issue description after `TODO`
- tests/integration/widgets/test_range_slider.py:211:11: TD005 Missing issue description after `TODO`
+ tests/integration/widgets/test_range_slider.py:220:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_range_slider.py:220:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_range_slider.py:243:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_range_slider.py:243:11: TD007 Missing space after colon in TODO
- tests/integration/widgets/test_slider.py:177:11: TD005 Missing issue description after `TODO`
+ tests/integration/widgets/test_slider.py:186:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_slider.py:186:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_slider.py:220:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_slider.py:220:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_slider.py:243:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_slider.py:243:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_slider.py:266:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_slider.py:266:11: TD007 Missing space after colon in TODO
+ tests/integration/widgets/test_spinner.py:228:11: TD004 Missing colon in TODO
- tests/integration/widgets/test_spinner.py:228:11: TD007 Missing space after colon in TODO
- tests/integration/widgets/test_toggle.py:105:7: TD005 Missing issue description after `TODO`
+ tests/support/plugins/bokeh_server.py:103:66: TD001 Invalid TODO tag: `XXX`
+ tests/support/plugins/bokeh_server.py:103:66: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/bokeh_server.py:103:66: TD003 Missing issue link on the line following this TODO
+ tests/support/plugins/bokeh_server.py:86:48: TD001 Invalid TODO tag: `XXX`
+ tests/support/plugins/bokeh_server.py:86:48: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/bokeh_server.py:86:48: TD003 Missing issue link on the line following this TODO
+ tests/support/plugins/project.py:162:15: TD004 Missing colon in TODO
- tests/support/plugins/project.py:162:15: TD007 Missing space after colon in TODO
- tests/unit/bokeh/core/property/test_aliases.py:78:3: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_bases.py:156:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_bases.py:188:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_bases.py:205:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_container.py:62:3: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_datetime.py:170:3: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_either.py:66:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_numeric.py:122:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_numeric.py:160:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_numeric.py:199:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_numeric.py:237:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_numeric.py:286:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_numeric.py:53:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_numeric.py:94:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_primitive.py:155:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_primitive.py:214:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_primitive.py:344:11: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_validation__property.py:120:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/property/test_validation__property.py:264:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/core/test_properties.py:152:3: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/document/test_callbacks__document.py:179:52: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/document/test_callbacks__document.py:378:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/document/test_callbacks__document.py:379:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/document/test_document.py:1014:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/document/test_document.py:1016:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/document/test_events__document.py:159:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/model/test_util_model.py:127:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/model/test_util_model.py:61:3: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/models/test_tools.py:7:3: TD005 Missing issue description after `TODO`
+ tests/unit/bokeh/plotting/test__renderer.py:115:5: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/plotting/test__renderer.py:115:5: TD003 Missing issue link on the line following this TODO
- tests/unit/bokeh/plotting/test_figure.py:536:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/test___init__.py:101:7: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/test_client_server.py:670:58: TD005 Missing issue description after `TODO`
- tests/unit/bokeh/test_tile_providers.py:79:3: TD005 Missing issue description after `TODO`
```

</p>
</details>
<details><summary>zulip (+18, -10)</summary>
<p>

```diff
- corporate/lib/stripe.py:603:7: TD005 Missing issue description after `TODO`
- corporate/models.py:264:7: TD005 Missing issue description after `TODO`
- tools/lib/provision.py:290:15: TD005 Missing issue description after `TODO`
- zerver/data_import/gitter.py:132:7: TD005 Missing issue description after `TODO`
- zerver/data_import/slack.py:570:15: TD005 Missing issue description after `TODO`
+ zerver/lib/compatibility.py:150:7: TD004 Missing colon in TODO
+ zerver/lib/email_notifications.py:610:11: TD004 Missing colon in TODO
+ zerver/lib/events.py:470:11: TD004 Missing colon in TODO
- zerver/lib/import_realm.py:682:7: TD005 Missing issue description after `TODO`
+ zerver/lib/message.py:1157:35: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/message.py:1157:35: TD003 Missing issue link on the line following this TODO
+ zerver/lib/narrow.py:278:15: TD004 Missing colon in TODO
+ zerver/lib/push_notifications.py:1049:11: TD004 Missing colon in TODO
- zerver/lib/push_notifications.py:155:7: TD005 Missing issue description after `TODO`
- zerver/lib/push_notifications.py:227:11: TD005 Missing issue description after `TODO`
+ zerver/lib/retention.py:365:32: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/retention.py:365:32: TD003 Missing issue link on the line following this TODO
+ zerver/lib/retention.py:365:32: TD004 Missing colon in TODO
+ zerver/lib/retention.py:365:32: TD005 Missing issue description after `TODO`
+ zerver/lib/sessions.py:66:54: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/sessions.py:66:54: TD003 Missing issue link on the line following this TODO
- zerver/lib/test_helpers.py:535:28: TD005 Missing issue description after `TODO`
- zerver/management/commands/convert_gitter_data.py:46:15: TD005 Missing issue description after `TODO`
+ zerver/tornado/event_queue.py:1141:7: TD004 Missing colon in TODO
+ zerver/tornado/event_queue.py:1153:7: TD004 Missing colon in TODO
+ zerver/tornado/event_queue.py:1326:11: TD004 Missing colon in TODO
+ zerver/tornado/event_queue.py:1345:15: TD004 Missing colon in TODO
+ zerver/tornado/event_queue.py:1360:15: TD004 Missing colon in TODO
```

</p>
</details>
Rules changed: 7

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TD005 | 91 | 3 | 88 |
| TD004 | 36 | 36 | 0 |
| TD003 | 23 | 23 | 0 |
| TD002 | 21 | 21 | 0 |
| TD007 | 17 | 0 | 17 |
| TD001 | 9 | 9 | 0 |
| TD006 | 3 | 3 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.03ms     2.7 MB/sec    1.01     15.1±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.8±1.55µs     7.9 MB/sec    1.00    375.8±0.96µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.5±0.01ms     5.4 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1616.3±4.32µs    10.3 MB/sec    1.00   1590.0±3.30µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    177.3±0.99µs    16.6 MB/sec    1.00    175.8±0.21µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.01      5.8±0.01ms     7.0 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1135.6±1.94µs    14.7 MB/sec    1.00   1123.6±0.54µs    14.8 MB/sec
parser/numpy/globals.py                    1.01    115.7±0.25µs    25.5 MB/sec    1.00    114.6±0.34µs    25.7 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.3±0.27ms     2.3 MB/sec    1.00     17.1±0.26ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.00      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    508.4±9.85µs     5.8 MB/sec    1.00   510.6±11.45µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.12ms     3.5 MB/sec    1.00      7.2±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.10ms     4.8 MB/sec    1.00      8.5±0.11ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1806.0±27.22µs     9.2 MB/sec    1.00  1811.4±59.24µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    210.4±4.30µs    14.0 MB/sec    1.00    205.4±4.83µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.07      7.0±0.05ms     5.8 MB/sec    1.00      6.5±0.07ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.05  1316.9±30.12µs    12.6 MB/sec    1.00  1251.7±21.56µs    13.3 MB/sec
parser/numpy/globals.py                    1.05    132.0±2.41µs    22.3 MB/sec    1.00    126.3±2.38µs    23.4 MB/sec
parser/pydantic/types.py                   1.06      3.0±0.04ms     8.6 MB/sec    1.00      2.8±0.06ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@sladyn98 reviewed on 2023-05-22 07:28_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/flake8_todos/rules.rs`:245 on 2023-05-22 07:28_

An alternative approach could be to split the comment into sections, then iterate over those sections. We can also use a HashMap to map the directive strings to their corresponding variants, which allows us to eliminate the if else chain.

```
impl FromStr for Directive {
    type Err = ();

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        match s {
            "fixme" => Ok(Directive::Fixme),
            "xxx" => Ok(Directive::Xxx),
            "todo" => Ok(Directive::Todo),
            _ => Err(()),
        }
    }
}

fn from_comment(comment: &str) -> Option<(Directive, TextSize)> {
    let comment_parts = comment.split('#').collect::<Vec<&str>>();

    let mut total_offset = TextSize::new(0);

    for part in comment_parts {
        let trimmed = part.trim().to_lowercase();
        total_offset += TextSize::of_str("#") + TextSize::try_from(trimmed.len()).unwrap();
        
        match trimmed.parse::<Directive>() {
            Ok(directive) => return Some((directive, total_offset)),
            Err(_) => continue,
        }
    }

    None
}
```

---

_@MichaReiser reviewed on 2023-05-22 08:40_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:245 on 2023-05-22 08:40_

I liked that. I think we could avoid collecting into a `Vec` since we're only iterating once. 

Using `spilt` is probably slightly slower because it iterates twice over the string but I wouldn't mind that in favor of readability. 

Nit: You can replace `TextSize::of_str("#")` with `'#'.text_len()` and `TextSize::try_from(trimmed.len()).unwrap()` with `trimmed.text_len()`

---

_@MichaReiser reviewed on 2023-05-22 08:41_

Would you mind adding a short description to your summary of what this PR fixes. I read the issue and I believe I know what it is about, but I'm unsure. 

@evanrittenhouse did you review the ecosystem checks. Do the new violations look legit?

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules.rs`:245 on 2023-05-22 12:42_

Yeah, was thinking about splitting but I didn't want to iterate over the string twice which is how I got to the approach in this PR. If we're fine with the slowdown, I can change it

---

_@evanrittenhouse reviewed on 2023-05-22 12:42_

---

_Comment by @evanrittenhouse on 2023-05-22 13:25_

@MichaReiser PR updated!

I reviewed some of the violations yesterday, but finished looking at them today. The only one that looks slightly wonky is [this one](https://github.com/bokeh/bokeh/blob/branch-3.2/tests/integration/widgets/test_daterange_slider.py#L160), where `MissingSpaceAfterColon` is thrown in a URL _when there's no colon after the author block_. It's technically correct, since the first colon after the author block isn't followed by a colon, but obviously incorrect in spirit. This could also occur outside of URLs: `# TODO (evanrittenhouse) fruits:apple, banana, cranberry`.

The line causing the issue is [here](https://github.com/charliermarsh/ruff/blob/main/crates/ruff/src/rules/flake8_todos/rules.rs#L417), where we `split_once()` after the author block is concluded. I'm thinking that, for a fix, we could check the first non-whitespace character after the author block's closing `)`. That character has to a be colon in a valid TODO. We could then treat that byte offset as we do the current `split_once()` call results. What do you think?

---

_@MichaReiser reviewed on 2023-05-23 06:20_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:245 on 2023-05-23 06:20_

I wouldn't expect the slowdown to be that significant because it only runs on comments but I'm okay either way

---

_Comment by @MichaReiser on 2023-05-23 06:27_

@evanrittenhouse thanks for the explanation. To double check. Do you mean this line https://github.com/bokeh/bokeh/blob/f4c7a9787f0fd93db6987cfbb46409098ddb3107/tests/integration/widgets/test_daterange_slider.py#L169 ?

> The line causing the issue is [here](https://github.com/charliermarsh/ruff/blob/main/crates/ruff/src/rules/flake8_todos/rules.rs?rgh-link-date=2023-05-22T13%3A25%3A37Z#L417), where we split_once() after the author block is concluded. I'm thinking that, for a fix, we could check the first non-whitespace character after the author block's closing )

That makes sense. Trimming the leading whitespace and then testing the character should give us better results.

---

_Comment by @evanrittenhouse on 2023-05-23 14:20_

@MichaReiser Yep, that's the one! Sorry, misclicked the copy-paste.

I've pushed a fix to this PR just now. Seems like there have been some changes, so I'll have to investigate them after work and make sure that they're valid

E: Seems like the big issue is a bunch of `TD005` (missing issue link) rules are disappearing now. Have to figure out why that's the case since the changes here shouldn't touch any of the logic around detecting the issue...

---

_Renamed from "Change tag detection to work with multiple pound signs on the same line" to "Change TODO directive detection to work with multiple pound signs on the same line" by @evanrittenhouse on 2023-05-23 14:58_

---

_Comment by @evanrittenhouse on 2023-05-23 21:14_

Hmm. So I copied one of the files that changed according to the ecosystem check ([`credentials.py`](https://github.com/bokeh/bokeh/blob/branch-3.2/release/credentials.py)) locally to see if I could reproduce the issues. The ecosystem checks report that the TD003 diagnostics on lines 77 and 84 are both gone. When I run `cargo run -p ruff_cli -- test.py --select ALL`, they both show up as expected. 

@MichaReiser do you have any idea what could be happening?

---

_Comment by @MichaReiser on 2023-05-24 06:48_

@evanrittenhouse the results from the ecosystem check seem correct. 

On [Add test for colons that don't terminate the tag/author block](https://github.com/charliermarsh/ruff/pull/4558/commits/7b07e7665c0ea55016e6b5f1f0ffc52b2ad3f776).

<pre><span style="color:#005FD7">cargo</span> <span style="color:#00AFFF">run</span> <span style="color:#00AFFF">--bin</span> <span style="color:#00AFFF">ruff</span> <span style="color:#00AFFF">--</span> <span style="color:#00AFFF">check</span> <span style="color:#00AFFF">--no-cache</span> <span style="color:#00A6B2"><u style="text-decoration-style:single">~</u></span><span style="color:#00AFFF"><u style="text-decoration-style:single">/Downloads/credentials.py</u></span> <span style="color:#00AFFF">--select</span> <span style="color:#00AFFF">TD</span>
<span style="color:#26A269"><b>    Finished</b></span> dev [unoptimized + debuginfo] target(s) in 0.10s
<span style="color:#26A269"><b>     Running</b></span> `target/debug/ruff check --no-cache /home/micha/Downloads/credentials.py --select TD`
/home/micha/Downloads/credentials.py:77:7: TD002 Missing author in TODO; try: `# TODO(&lt;author_name&gt;): ...`
/home/micha/Downloads/credentials.py:77:7: TD003 Missing issue link on the line following this TODO
/home/micha/Downloads/credentials.py:77:7: TD004 Missing colon in TODO
/home/micha/Downloads/credentials.py:77:7: TD005 Missing issue description after `TODO`
/home/micha/Downloads/credentials.py:84:7: TD002 Missing author in TODO; try: `# TODO(&lt;author_name&gt;): ...`
/home/micha/Downloads/credentials.py:84:7: TD003 Missing issue link on the line following this TODO
/home/micha/Downloads/credentials.py:84:7: TD004 Missing colon in TODO
/home/micha/Downloads/credentials.py:84:7: TD005 Missing issue description after `TODO`
Found 8 errors.
</pre>

On the latest commit
<pre><span style="color:#005FD7">cargo</span> <span style="color:#00AFFF">run</span> <span style="color:#00AFFF">--bin</span> <span style="color:#00AFFF">ruff</span> <span style="color:#00AFFF">--</span> <span style="color:#00AFFF">check</span> <span style="color:#00AFFF">--no-cache</span> <span style="color:#00A6B2"><u style="text-decoration-style:single">~</u></span><span style="color:#00AFFF"><u style="text-decoration-style:single">/Downloads/credentials.py</u></span> <span style="color:#00AFFF">--select</span> <span style="color:#00AFFF">TD</span>
<span style="color:#26A269"><b>   Compiling</b></span> ruff v0.0.269 (/home/micha/astral/ruff/crates/ruff)
<span style="color:#26A269"><b>   Compiling</b></span> ruff_cli v0.0.269 (/home/micha/astral/ruff/crates/ruff_cli)
<span style="color:#26A269"><b>    Finished</b></span> dev [unoptimized + debuginfo] target(s) in 6.78s
<span style="color:#26A269"><b>     Running</b></span> `target/debug/ruff check --no-cache /home/micha/Downloads/credentials.py --select TD`
/home/micha/Downloads/credentials.py:77:7: TD002 Missing author in TODO; try: `# TODO(&lt;author_name&gt;): ...`
/home/micha/Downloads/credentials.py:77:7: TD003 Missing issue link on the line following this TODO
/home/micha/Downloads/credentials.py:77:7: TD004 Missing colon in TODO
/home/micha/Downloads/credentials.py:84:7: TD002 Missing author in TODO; try: `# TODO(&lt;author_name&gt;): ...`
/home/micha/Downloads/credentials.py:84:7: TD003 Missing issue link on the line following this TODO
/home/micha/Downloads/credentials.py:84:7: TD004 Missing colon in TODO
Found 6 errors.
</pre>

You can see that the diagnostics for TD005 are missing. I think this is correct because there's a description. 



---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:446 on 2023-05-24 06:51_

Nit: I find the use of the `let Some(char)` with a second branching on `char == ':'` more difficult to reason about than using a single match (also because it requires an early `return`). 

```rust
    match post_author_chars.next() {
        Some(':') => {
            match post_author_chars.peek() {
                Some(' ') => (),
                // TD-007
                Some(_) => diagnostics.push(Diagnostic::new(MissingSpaceAfterTodoColon, tag.range)),
                None => diagnostics.push(Diagnostic::new(MissingTodoDescription, tag.range)),
            }
        }
        Some(_) => {
            diagnostics.push(Diagnostic::new(MissingTodoColon, tag.range));
        }
        None => {
            // TD-004
            diagnostics.push(Diagnostic::new(MissingTodoColon, tag.range));
            // TD-005
            diagnostics.push(Diagnostic::new(MissingTodoDescription, tag.range));
        }
    }
```

The `match` makes it clear to me that there are three different branches and they are exclusive to each other. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:437 on 2023-05-24 06:52_

Do we need to test if all remaining characters are whitespace and if so, emit a `MissingTodoDescription` diagnostic?

I think we can do this with:

```rust
    let post_author = post_tag[usize::from(author_end)..].trim_start();

    if let Some(after) = post_author.strip_prefix(':') {
        if after.trim_start().is_empty() {
            diagnostics.push(Diagnostic::new(MissingTodoDescription, tag.range))
        } else if !after.starts_with(char::is_whitespace) {
            diagnostics.push(Diagnostic::new(MissingSpaceAfterTodoColon, tag.range))
        }
    } else {
        diagnostics.push(Diagnostic::new(MissingTodoColon, tag.range));

        if post_author.is_empty() {
            diagnostics.push(Diagnostic::new(MissingTodoDescription, tag.range));
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:436 on 2023-05-24 06:53_

I think we can use `next` here because we aren't using `post_author_chars` after this. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:424 on 2023-05-24 07:08_

Is the `to_owned` call necessary?

---

_@MichaReiser approved on 2023-05-24 07:08_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules.rs`:437 on 2023-05-25 13:13_

Will use this. Thanks!

---

_@evanrittenhouse reviewed on 2023-05-25 13:13_

---

_Comment by @evanrittenhouse on 2023-05-25 13:22_

@MichaReiser Thanks for the suggestions!

---

_Merged by @MichaReiser on 2023-05-25 14:51_

---

_Closed by @MichaReiser on 2023-05-25 14:51_

---

_Branch deleted on 2023-05-25 17:12_

---
