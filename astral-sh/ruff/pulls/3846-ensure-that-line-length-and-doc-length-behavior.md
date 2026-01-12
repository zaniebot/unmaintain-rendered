```yaml
number: 3846
title: Ensure that line-length and doc-length behavior is consistent with Flake8 
type: pull_request
state: closed
author: JBLDKY
labels: []
assignees: []
draft: true
base: main
head: maxdoclen
created_at: 2023-04-01T12:37:52Z
updated_at: 2023-04-02T11:05:50Z
url: https://github.com/astral-sh/ruff/pull/3846
synced_at: 2026-01-12T04:28:19Z
```

# Ensure that line-length and doc-length behavior is consistent with Flake8 

---

_Pull request opened by @JBLDKY on 2023-04-01 12:37_

Here is my proposed fix for:
https://github.com/charliermarsh/ruff/issues/2756

It's certainly not the most elegant solution, it's just a proposal and i'm curious to hear if anyone sees a better solution.

I'm also very much unsure how to test this at the moment. Currently I'm using a custom pyproject.toml like:
```toml
[tool.ruff]
line-length = 120

[tool.ruff.pycodestyle]
max-doc-length = 180
```

Then I include it in the command:
```
cargo run -p ruff_cli -- check crates/ruff/resources/test/fixtures/pycodestyle/W505.py --no-cache --config crates/ruff/resources/test/fixtures/pycodestyle/pyproject.toml --select W505 --extend-select E501
```

Help would be very much appreciated!

---

_Comment by @github-actions[bot] on 2023-04-01 13:02_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+17, -142, 0 error(s))

<details><summary>airflow (+9, -0)</summary>
<p>

```diff
+ airflow/providers/amazon/aws/hooks/ecs.py:229:120: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ airflow/providers/amazon/aws/hooks/ecs.py:233:122: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ airflow/providers/amazon/aws/hooks/ecs.py:237:126: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ airflow/providers/amazon/aws/hooks/ecs.py:241:121: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ airflow/providers/amazon/aws/hooks/ecs.py:245:136: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ airflow/providers/amazon/aws/hooks/ecs.py:249:122: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ airflow/providers/google/cloud/hooks/bigquery.py:1676:131: RUF100 [*] Unused blanket `noqa` directive
+ airflow/providers/google/cloud/hooks/bigquery.py:2086:116: RUF100 [*] Unused blanket `noqa` directive
+ airflow/providers/google/cloud/operators/dataproc.py:138:10: RUF100 [*] Unused `noqa` directive (unused: `E501`)
```

</p>
</details>
<details><summary>bokeh (+8, -0)</summary>
<p>

```diff
+ examples/basic/data/color_mappers.py:9:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ examples/interaction/js_callbacks/color_sliders.py:9:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ examples/models/buttons.py:15:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ examples/models/calendars.py:14:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ examples/models/sliders.py:9:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ examples/models/widgets.py:11:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ examples/plotting/sprint.py:13:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ examples/topics/stats/splom.py:13:5: RUF100 [*] Unused `noqa` directive (unused: `E501`)
```

</p>
</details>
<details><summary>zulip (+0, -142)</summary>
<p>

```diff
- corporate/lib/stripe.py:1153:101: E501 Line too long (114 > 100 characters)
- corporate/tests/test_stripe.py:1532:101: E501 Line too long (103 > 100 characters)
- corporate/tests/test_stripe.py:1702:101: E501 Line too long (103 > 100 characters)
- corporate/tests/test_stripe.py:2469:101: E501 Line too long (106 > 100 characters)
- corporate/tests/test_stripe.py:2612:101: E501 Line too long (106 > 100 characters)
- corporate/tests/test_stripe.py:2613:101: E501 Line too long (105 > 100 characters)
- tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:104:101: E501 Line too long (117 > 100 characters)
- tools/lib/pretty_print.py:52:101: E501 Line too long (164 > 100 characters)
- tools/linter_lib/custom_check.py:10:101: E501 Line too long (118 > 100 characters)
- tools/linter_lib/custom_check.py:11:101: E501 Line too long (111 > 100 characters)
- tools/linter_lib/custom_check.py:6:101: E501 Line too long (109 > 100 characters)
- tools/linter_lib/custom_check.py:9:101: E501 Line too long (117 > 100 characters)
- zerver/actions/message_edit.py:590:101: E501 Line too long (118 > 100 characters)
- zerver/actions/message_edit.py:591:101: E501 Line too long (117 > 100 characters)
- zerver/actions/message_edit.py:592:101: E501 Line too long (118 > 100 characters)
- zerver/actions/message_edit.py:593:101: E501 Line too long (116 > 100 characters)
- zerver/actions/message_edit.py:594:101: E501 Line too long (116 > 100 characters)
- zerver/actions/message_edit.py:595:101: E501 Line too long (122 > 100 characters)
- zerver/actions/message_send.py:1331:101: E501 Line too long (107 > 100 characters)
- zerver/actions/message_send.py:736:101: E501 Line too long (101 > 100 characters)
- zerver/actions/message_send.py:737:101: E501 Line too long (101 > 100 characters)
- zerver/data_import/mattermost.py:725:101: E501 Line too long (105 > 100 characters)
- zerver/data_import/mattermost.py:729:101: E501 Line too long (101 > 100 characters)
- zerver/data_import/rocketchat.py:638:101: E501 Line too long (108 > 100 characters)
- zerver/data_import/slack.py:155:101: E501 Line too long (103 > 100 characters)
- zerver/data_import/slack.py:499:101: E501 Line too long (106 > 100 characters)
- zerver/forms.py:199:101: E501 Line too long (101 > 100 characters)
- zerver/forms.py:305:101: E501 Line too long (101 > 100 characters)
- zerver/lib/avatar.py:136:101: E501 Line too long (104 > 100 characters)
- zerver/lib/markdown/__init__.py:2043:101: E501 Line too long (103 > 100 characters)
- zerver/lib/markdown/__init__.py:2407:101: E501 Line too long (117 > 100 characters)
- zerver/lib/markdown/__init__.py:2408:101: E501 Line too long (107 > 100 characters)
- zerver/lib/markdown/__init__.py:2454:101: E501 Line too long (103 > 100 characters)
- zerver/lib/markdown/__init__.py:2458:101: E501 Line too long (102 > 100 characters)
- zerver/lib/markdown/__init__.py:854:101: E501 Line too long (113 > 100 characters)
- zerver/lib/name_restrictions.py:127:101: E501 Line too long (101 > 100 characters)
- zerver/lib/push_notifications.py:844:101: E501 Line too long (102 > 100 characters)
- zerver/lib/rate_limiter.py:443:101: E501 Line too long (103 > 100 characters)
- zerver/lib/rate_limiter.py:479:101: E501 Line too long (102 > 100 characters)
- zerver/lib/send_email.py:112:101: E501 Line too long (102 > 100 characters)
- zerver/lib/topic.py:277:101: E501 Line too long (106 > 100 characters)
- zerver/lib/validator.py:168:101: E501 Line too long (101 > 100 characters)
- zerver/middleware.py:552:101: E501 Line too long (104 > 100 characters)
- zerver/migrations/0237_rename_zulip_realm_to_zulipinternal.py:29:101: E501 Line too long (102 > 100 characters)
- zerver/models.py:4691:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_auth_backends.py:1976:101: E501 Line too long (102 > 100 characters)
- zerver/tests/test_auth_backends.py:2747:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_auth_backends.py:4787:101: E501 Line too long (113 > 100 characters)
- zerver/tests/test_auth_backends.py:4800:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_bots.py:1120:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_bots.py:269:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_bots.py:77:101: E501 Line too long (110 > 100 characters)
- zerver/tests/test_custom_profile_data.py:221:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_decorators.py:2214:101: E501 Line too long (110 > 100 characters)
- zerver/tests/test_email_notifications.py:1044:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_email_notifications.py:1047:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_email_notifications.py:432:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_event_queue.py:242:101: E501 Line too long (106 > 100 characters)
- zerver/tests/test_event_queue.py:281:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_events.py:2550:101: E501 Line too long (110 > 100 characters)
- zerver/tests/test_events.py:2551:101: E501 Line too long (108 > 100 characters)
- zerver/tests/test_gitter_importer.py:25:101: E501 Line too long (102 > 100 characters)
- zerver/tests/test_gitter_importer.py:26:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_home.py:780:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_home.py:786:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_home.py:801:101: E501 Line too long (108 > 100 characters)
- zerver/tests/test_home.py:864:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_home.py:874:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_home.py:882:101: E501 Line too long (108 > 100 characters)
- zerver/tests/test_i18n.py:45:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_i18n.py:48:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_i18n.py:72:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_import_export.py:516:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_import_export.py:535:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_invite.py:212:101: E501 Line too long (102 > 100 characters)
- zerver/tests/test_link_embed.py:454:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_markdown.py:1539:101: E501 Line too long (129 > 100 characters)
- zerver/tests/test_markdown.py:1540:101: E501 Line too long (140 > 100 characters)
- zerver/tests/test_markdown.py:1604:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_markdown.py:1649:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_markdown.py:1658:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_markdown.py:673:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_message_edit.py:2130:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_message_edit.py:3380:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_message_edit.py:3403:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_message_fetch.py:1418:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_message_send.py:383:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_message_send.py:450:101: E501 Line too long (102 > 100 characters)
- zerver/tests/test_message_send.py:474:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_messages.py:65:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_muted_users.py:364:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_outgoing_webhook_system.py:275:101: E501 Line too long (117 > 100 characters)
- zerver/tests/test_outgoing_webhook_system.py:276:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_queue_worker.py:283:101: E501 Line too long (106 > 100 characters)
- zerver/tests/test_queue_worker.py:284:101: E501 Line too long (106 > 100 characters)
- zerver/tests/test_queue_worker.py:286:101: E501 Line too long (106 > 100 characters)
- zerver/tests/test_queue_worker.py:287:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_queue_worker.py:288:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_queue_worker.py:289:101: E501 Line too long (108 > 100 characters)
- zerver/tests/test_queue_worker.py:290:101: E501 Line too long (110 > 100 characters)
- zerver/tests/test_retention.py:341:101: E501 Line too long (117 > 100 characters)
- zerver/tests/test_retention.py:994:101: E501 Line too long (108 > 100 characters)
- zerver/tests/test_retention.py:995:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_retention.py:996:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_retention.py:997:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_scim.py:676:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_slack_importer.py:661:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_subs.py:1413:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_subs.py:2234:101: E501 Line too long (114 > 100 characters)
- zerver/tests/test_subs.py:3880:101: E501 Line too long (118 > 100 characters)
- zerver/tests/test_subs.py:6072:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_subs.py:721:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_upload.py:1253:101: E501 Line too long (115 > 100 characters)
- zerver/tests/test_upload.py:1370:101: E501 Line too long (107 > 100 characters)
- zerver/tests/test_user_groups.py:525:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_user_groups.py:689:101: E501 Line too long (102 > 100 characters)
- zerver/tests/test_user_groups.py:703:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_user_groups.py:733:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_user_groups.py:876:101: E501 Line too long (102 > 100 characters)
- zerver/tests/test_users.py:2313:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_users.py:2320:101: E501 Line too long (101 > 100 characters)
- zerver/tests/test_users.py:2401:101: E501 Line too long (101 > 100 characters)
- zerver/views/upload.py:46:101: E501 Line too long (104 > 100 characters)
- zerver/webhooks/azuredevops/view.py:153:101: E501 Line too long (103 > 100 characters)
- zerver/webhooks/circleci/view.py:118:101: E501 Line too long (103 > 100 characters)
- zerver/webhooks/clubhouse/view.py:92:101: E501 Line too long (119 > 100 characters)
- zerver/webhooks/freshstatus/tests.py:128:101: E501 Line too long (108 > 100 characters)
- zerver/webhooks/solano/view.py:54:101: E501 Line too long (103 > 100 characters)
- zerver/webhooks/stripe/view.py:170:101: E501 Line too long (104 > 100 characters)
- zproject/backends.py:1291:101: E501 Line too long (108 > 100 characters)
- zproject/backends.py:1300:101: E501 Line too long (104 > 100 characters)
- zproject/backends.py:1302:101: E501 Line too long (106 > 100 characters)
- zproject/backends.py:1303:101: E501 Line too long (104 > 100 characters)
- zproject/backends.py:1452:101: E501 Line too long (101 > 100 characters)
- zproject/backends.py:1586:101: E501 Line too long (108 > 100 characters)
- zproject/backends.py:1664:101: E501 Line too long (102 > 100 characters)
- zproject/backends.py:2383:101: E501 Line too long (105 > 100 characters)
- zproject/backends.py:2462:101: E501 Line too long (104 > 100 characters)
- zproject/backends.py:2605:101: E501 Line too long (102 > 100 characters)
- zproject/backends.py:650:101: E501 Line too long (103 > 100 characters)
- zproject/prod_settings_template.py:643:101: E501 Line too long (103 > 100 characters)
- zproject/prod_settings_template.py:645:101: E501 Line too long (101 > 100 characters)
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.17ms     2.5 MB/sec    1.02     16.6±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.03      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    527.8±7.10µs     5.6 MB/sec    1.01    535.0±8.60µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.02      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.09ms     4.8 MB/sec    1.04      8.7±0.09ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1928.6±22.08µs     8.6 MB/sec    1.02  1960.3±24.29µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    210.8±3.24µs    14.0 MB/sec    1.03    216.6±4.62µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.04ms     6.5 MB/sec    1.02      4.0±0.04ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.10ms     2.7 MB/sec    1.01     15.5±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.02      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   487.8±16.10µs     6.0 MB/sec    1.01    494.0±9.91µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.05ms     3.9 MB/sec    1.01      6.7±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.01      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1770.1±14.89µs     9.4 MB/sec    1.02  1804.4±34.74µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.9±2.51µs    15.5 MB/sec    1.02    195.0±5.18µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-01 14:16_

I'm trying to recall the details of this... I'm confused because:

```py
❯ flake8 crates/ruff/resources/test/fixtures/pycodestyle/W505.py --select E501,W505 --max-doc-length 120
crates/ruff/resources/test/fixtures/pycodestyle/W505.py:15:80: E501 line too long (173 > 79 characters)
crates/ruff/resources/test/fixtures/pycodestyle/W505.py:18:80: E501 line too long (241 > 79 characters)
crates/ruff/resources/test/fixtures/pycodestyle/W505.py:18:121: W505 doc line too long (241 > 120 characters)
```

So Flake8 does seem to flag line 18 for _both_ W505 and E501, but the comment in the issue suggested that it wouldn't flag E501 in that case.


---

_Comment by @JBLDKY on 2023-04-01 15:20_

> I'm trying to recall the details of this... I'm confused because:
> 
> ```python
> ❯ flake8 crates/ruff/resources/test/fixtures/pycodestyle/W505.py --select E501,W505 --max-doc-length 120
> crates/ruff/resources/test/fixtures/pycodestyle/W505.py:15:80: E501 line too long (173 > 79 characters)
> crates/ruff/resources/test/fixtures/pycodestyle/W505.py:18:80: E501 line too long (241 > 79 characters)
> crates/ruff/resources/test/fixtures/pycodestyle/W505.py:18:121: W505 doc line too long (241 > 120 characters)
> ```
> 
> So Flake8 does seem to flag line 18 for _both_ W505 and E501, but the comment in the issue suggested that it wouldn't flag E501 in that case.

Agreed, I can't find anything of the sort in their code either.


---

_Comment by @charliermarsh on 2023-04-01 15:28_

Hmm, ok, let's close this and the corresponding issue. I'm sorry you spent time, I thought I double-checked the issue at the time but I must be misremembering!

---

_Comment by @JBLDKY on 2023-04-01 16:02_

No worries. It was fun - will pick another tomorrow :D

---

_Closed by @JBLDKY on 2023-04-01 16:02_

---

_Comment by @sterlinm on 2023-04-02 03:20_

I'm very sorry for the confusion on this! I'll have to look into the configuration that led me to misunderstand how these rules interact.

---

_Comment by @charliermarsh on 2023-04-02 03:40_

Don’t sweat it! If you find that we are in fact doing something inconsistent, feel free to follow up :)

---

_Comment by @sterlinm on 2023-04-02 04:48_

Thanks :)

I looked into what made me think there's an issue and I think I figured it out, though I'm not sure that what flake8 is doing is sensible. It looks like if you have multi-line docstring and you set `noqa` flake8 will disable the check for the entire docstring while ruff will only apply it to the specific line within the docstring and flag other lines in the docstring.

<details>
  <summary>pyproject.toml</summary>

```toml
[tool.ruff]
line-length = 120

[tool.ruff.pycodestyle]
max-doc-length = 180
```

</details>

<details>
  <summary>W505.py</summary>

```python
#!/usr/bin/env python3
"""
Here's a top-level docstring that's over the limit.

This line is greater than 180 characters and no noqa. This line is greater than 180 characters and no noqa. This line is greater than 180 characters and no noqa. This line is greater than 180 characters and no noqa. 

This line is greater than 180 characters and noqa. This line is greater than 180 characters and noqa. This line is greater than 180 characters and noqa. This line is greater than 180 characters and noqa. # noqa
"""


def f():
    """Here's a docstring that's also over the limit.
    
    Here's a line in the docstring that's over the limit and does not have noqa.  Here's a line in the docstring that's over the limit and does not have noqa.  Here's a line in the docstring that's over the limit and does not have noqa.
    
    Here's a line in the docstring that's over the limit and DOES have noqa. Here's a line in the docstring that's over the limit and DOES have noqa. Here's a line in the docstring that's over the limit and DOES have noqa.  # noqa
    """

    x = 1  # Here's a comment that's over the limit, but it's not standalone.

    # Here's a standalone comment that's over the limit.

    print("Here's a string that's over the limit, but it's not a docstring.")
    
def f2():
    """Here's a docstring that's also over the limit. noqa is after the closing quotation mark.
    
    Here's a line in the docstring that's over the limit and does not have noqa.  Here's a line in the docstring that's over the limit and does not have noqa.  Here's a line in the docstring that's over the limit and does not have noqa.
    """  # noqa

# E501 & not W505
looooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooong_var = 10

# W505 & not E501
"This is also considered a docstring, and is over the limit. This is also considered a docstring, and is over the limit. This is also considered a docstring, and is over the limit. This is also considered a docstring, and is over the limit."

```

</details>

Here's the output I get from flake8:

```shell
> flake8 --select E501 --max-line-length 120 --max-doc-length 180 W505.py
W505.py:32:121: E501 line too long (173 > 120 characters)
W505.py:35:121: E501 line too long (241 > 120 characters)
```

And here's the output I get from ruff with the pyproject.toml file above.

```shell
> ruff check --select E501 W505.py
W505.py:5:121: E501 Line too long (216 > 120 characters)
W505.py:7:121: E501 Line too long (210 > 120 characters)
W505.py:14:121: E501 Line too long (236 > 120 characters)
W505.py:16:121: E501 Line too long (230 > 120 characters)
W505.py:32:121: E501 Line too long (173 > 120 characters)
W505.py:35:121: E501 Line too long (241 > 120 characters)
Found 6 errors.
```

I can't recall if this is one of the documented intentional differences in ruff but will review the docs. Thanks!

Edit: I found the note in the docs about putting the noqa at the end of the block in a multiline string.

> Note that, for multi-line strings, the noqa directive should come at the end of the string, and will apply to the entire body

---

_Branch deleted on 2023-04-02 07:58_

---
