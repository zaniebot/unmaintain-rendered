```yaml
number: 3498
title: Make Clap an optional feature for ruff crate
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cpa
created_at: 2023-03-13T23:06:32Z
updated_at: 2023-03-14T15:02:07Z
url: https://github.com/astral-sh/ruff/pull/3498
synced_at: 2026-01-12T04:39:45Z
```

# Make Clap an optional feature for ruff crate

---

_Pull request opened by @charliermarsh on 2023-03-13 23:06_

## Summary

It's hard to remove the `clap` dependency entirely due to orphan rules. Open to ideas -- we could create wrapper structs in `ruff-cli`? But this at least removes `clap` for isolated testing, etc.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-13 23:06_

---

_@charliermarsh reviewed on 2023-03-13 23:06_

---

_Review comment by @charliermarsh on `crates/ruff_cli/Cargo.toml`:37 on 2023-03-13 23:06_

I bumped this while I was here.

---

_@charliermarsh reviewed on 2023-03-13 23:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/types.rs`:57 on 2023-03-13 23:08_

I think we had this custom implementation so that we could warn when `py33` and equivalent were provided, but IMO this is not a great tradeoff, since we lost (e.g.) the possible values in the CLI, at least out-of-the-box, by way of not using `ValueEnum`:

```diff
--- a/docs/configuration.md
+++ b/docs/configuration.md
@@ -211,7 +211,7 @@ Options:
       --format <FORMAT>
           Output serialization format for violations [env: RUFF_FORMAT=] [possible values: text, json, junit, grouped, github, gitlab, pylint, azure]
       --target-version <TARGET_VERSION>
-          The minimum Python version that should be supported
+          The minimum Python version that should be supported [possible values: py37, py38, py39, py310, py311]
```

---

_Comment by @github-actions[bot] on 2023-03-13 23:22_

ℹ️ ecosystem check **detected changes**. (+313, -313, 0 error(s))

<details><summary>zulip (+95, -95)</summary>
<p>

```diff
+ tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:185:36: PLC1901 `split_url.fragment != ""` can be simplified to `not split_url.fragment` as an empty string is falsey
- tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:185:36: PLC1901 `split_url.fragment != ""` can be simplified to `split_url.fragment` as an empty string is falsey
+ tools/lib/html_branches.py:66:21: PLC1901 `s != ""` can be simplified to `not s` as an empty string is falsey
- tools/lib/html_branches.py:66:21: PLC1901 `s != ""` can be simplified to `s` as an empty string is falsey
+ tools/lib/html_branches.py:71:13: PLC1901 `s != ""` can be simplified to `not s` as an empty string is falsey
- tools/lib/html_branches.py:71:13: PLC1901 `s != ""` can be simplified to `s` as an empty string is falsey
- tools/lib/template_parser.py:126:25: PLC1901 `s == ""` can be simplified to `not s` as an empty string is falsey
+ tools/lib/template_parser.py:126:25: PLC1901 `s == ""` can be simplified to `s` as an empty string is falsey
- tools/lib/template_parser.py:234:25: PLC1901 `s == ""` can be simplified to `not s` as an empty string is falsey
+ tools/lib/template_parser.py:234:25: PLC1901 `s == ""` can be simplified to `s` as an empty string is falsey
+ zerver/actions/default_streams.py:20:30: PLC1901 `group_name.strip() == ""` can be simplified to `group_name.strip()` as an empty string is falsey
- zerver/actions/default_streams.py:20:30: PLC1901 `group_name.strip() == ""` can be simplified to `not group_name.strip()` as an empty string is falsey
+ zerver/actions/invites.py:235:21: PLC1901 `email == ""` can be simplified to `email` as an empty string is falsey
- zerver/actions/invites.py:235:21: PLC1901 `email == ""` can be simplified to `not email` as an empty string is falsey
+ zerver/actions/message_edit.py:980:32: PLC1901 `content.rstrip() == ""` can be simplified to `content.rstrip()` as an empty string is falsey
- zerver/actions/message_edit.py:980:32: PLC1901 `content.rstrip() == ""` can be simplified to `not content.rstrip()` as an empty string is falsey
+ zerver/actions/streams.py:1173:31: PLC1901 `new_description == ""` can be simplified to `new_description` as an empty string is falsey
- zerver/actions/streams.py:1173:31: PLC1901 `new_description == ""` can be simplified to `not new_description` as an empty string is falsey
- zerver/actions/streams.py:1175:31: PLC1901 `old_description == ""` can be simplified to `not old_description` as an empty string is falsey
+ zerver/actions/streams.py:1175:31: PLC1901 `old_description == ""` can be simplified to `old_description` as an empty string is falsey
- zerver/data_import/slack.py:413:30: PLC1901 `value["value"] == ""` can be simplified to `not value["value"]` as an empty string is falsey
+ zerver/data_import/slack.py:413:30: PLC1901 `value["value"] == ""` can be simplified to `value["value"]` as an empty string is falsey
- zerver/lib/camo.py:19:29: PLC1901 `settings.CAMO_URI == ""` can be simplified to `not settings.CAMO_URI` as an empty string is falsey
+ zerver/lib/camo.py:19:29: PLC1901 `settings.CAMO_URI == ""` can be simplified to `settings.CAMO_URI` as an empty string is falsey
- zerver/lib/email_mirror.py:146:42: PLC1901 `settings.EMAIL_GATEWAY_PATTERN == ""` can be simplified to `not settings.EMAIL_GATEWAY_PATTERN` as an empty string is falsey
+ zerver/lib/email_mirror.py:146:42: PLC1901 `settings.EMAIL_GATEWAY_PATTERN == ""` can be simplified to `settings.EMAIL_GATEWAY_PATTERN` as an empty string is falsey
+ zerver/lib/email_mirror.py:184:21: PLC1901 `postamble != ""` can be simplified to `not postamble` as an empty string is falsey
- zerver/lib/email_mirror.py:184:21: PLC1901 `postamble != ""` can be simplified to `postamble` as an empty string is falsey
- zerver/lib/email_mirror_helpers.py:55:42: PLC1901 `settings.EMAIL_GATEWAY_PATTERN == ""` can be simplified to `not settings.EMAIL_GATEWAY_PATTERN` as an empty string is falsey
+ zerver/lib/email_mirror_helpers.py:55:42: PLC1901 `settings.EMAIL_GATEWAY_PATTERN == ""` can be simplified to `settings.EMAIL_GATEWAY_PATTERN` as an empty string is falsey
- zerver/lib/email_notifications.py:661:19: PLC1901 `user_tz == ""` can be simplified to `not user_tz` as an empty string is falsey
+ zerver/lib/email_notifications.py:661:19: PLC1901 `user_tz == ""` can be simplified to `user_tz` as an empty string is falsey
- zerver/lib/events.py:1347:31: PLC1901 `status_text == ""` can be simplified to `not status_text` as an empty string is falsey
+ zerver/lib/events.py:1347:31: PLC1901 `status_text == ""` can be simplified to `status_text` as an empty string is falsey
+ zerver/lib/events.py:1353:34: PLC1901 `emoji_name == ""` can be simplified to `emoji_name` as an empty string is falsey
- zerver/lib/events.py:1353:34: PLC1901 `emoji_name == ""` can be simplified to `not emoji_name` as an empty string is falsey
+ zerver/lib/events.py:1359:38: PLC1901 `emoji_code == ""` can be simplified to `emoji_code` as an empty string is falsey
- zerver/lib/events.py:1359:38: PLC1901 `emoji_code == ""` can be simplified to `not emoji_code` as an empty string is falsey
+ zerver/lib/events.py:1365:84: PLC1901 `emoji_name == ""` can be simplified to `emoji_name` as an empty string is falsey
- zerver/lib/events.py:1365:84: PLC1901 `emoji_name == ""` can be simplified to `not emoji_name` as an empty string is falsey
+ zerver/lib/generate_test_data.py:196:20: PLC1901 `text != ""` can be simplified to `not text` as an empty string is falsey
- zerver/lib/generate_test_data.py:196:20: PLC1901 `text != ""` can be simplified to `text` as an empty string is falsey
- zerver/lib/markdown/__init__.py:1224:28: PLC1901 `host != ""` can be simplified to `host` as an empty string is falsey
+ zerver/lib/markdown/__init__.py:1224:28: PLC1901 `host != ""` can be simplified to `not host` as an empty string is falsey
+ zerver/lib/markdown/__init__.py:1281:26: PLC1901 `netloc == ""` can be simplified to `netloc` as an empty string is falsey
- zerver/lib/markdown/__init__.py:1281:26: PLC1901 `netloc == ""` can be simplified to `not netloc` as an empty string is falsey
- zerver/lib/markdown/__init__.py:1549:18: PLC1901 `scheme == ""` can be simplified to `not scheme` as an empty string is falsey
+ zerver/lib/markdown/__init__.py:1549:18: PLC1901 `scheme == ""` can be simplified to `scheme` as an empty string is falsey
+ zerver/lib/markdown/__init__.py:1549:35: PLC1901 `netloc == ""` can be simplified to `netloc` as an empty string is falsey
- zerver/lib/markdown/__init__.py:1549:35: PLC1901 `netloc == ""` can be simplified to `not netloc` as an empty string is falsey
- zerver/lib/markdown/__init__.py:1551:20: PLC1901 `scheme == ""` can be simplified to `not scheme` as an empty string is falsey
+ zerver/lib/markdown/__init__.py:1551:20: PLC1901 `scheme == ""` can be simplified to `scheme` as an empty string is falsey
+ zerver/lib/markdown/__init__.py:1551:37: PLC1901 `netloc == ""` can be simplified to `netloc` as an empty string is falsey
- zerver/lib/markdown/__init__.py:1551:37: PLC1901 `netloc == ""` can be simplified to `not netloc` as an empty string is falsey
+ zerver/lib/markdown/api_arguments_table_generator.py:91:70: PLC1901 `get_parameters_description(endpoint, method) == ""` can be simplified to `get_parameters_description(endpoint, method)` as an empty string is falsey
- zerver/lib/markdown/api_arguments_table_generator.py:91:70: PLC1901 `get_parameters_description(endpoint, method) == ""` can be simplified to `not get_parameters_description(endpoint, method)` as an empty string is falsey
+ zerver/lib/markdown/fenced_code.py:439:46: PLC1901 `output[-2] != ""` can be simplified to `not output[-2]` as an empty string is falsey
- zerver/lib/markdown/fenced_code.py:439:46: PLC1901 `output[-2] != ""` can be simplified to `output[-2]` as an empty string is falsey
- zerver/lib/outgoing_webhook.py:320:25: PLC1901 `response_json == ""` can be simplified to `not response_json` as an empty string is falsey
+ zerver/lib/outgoing_webhook.py:320:25: PLC1901 `response_json == ""` can be simplified to `response_json` as an empty string is falsey
+ zerver/lib/outgoing_webhook.py:336:46: PLC1901 `content.strip() == ""` can be simplified to `content.strip()` as an empty string is falsey
- zerver/lib/outgoing_webhook.py:336:46: PLC1901 `content.strip() == ""` can be simplified to `not content.strip()` as an empty string is falsey
+ zerver/lib/rate_limiter.py:131:32: PLC1901 `self.rate_limits != ""` can be simplified to `not self.rate_limits` as an empty string is falsey
- zerver/lib/rate_limiter.py:131:32: PLC1901 `self.rate_limits != ""` can be simplified to `self.rate_limits` as an empty string is falsey
- zerver/lib/string_validation.py:37:31: PLC1901 `stream_name.strip() == ""` can be simplified to `not stream_name.strip()` as an empty string is falsey
+ zerver/lib/string_validation.py:37:31: PLC1901 `stream_name.strip() == ""` can be simplified to `stream_name.strip()` as an empty string is falsey
- zerver/lib/string_validation.py:53:25: PLC1901 `topic.strip() == ""` can be simplified to `not topic.strip()` as an empty string is falsey
+ zerver/lib/string_validation.py:53:25: PLC1901 `topic.strip() == ""` can be simplified to `topic.strip()` as an empty string is falsey
- zerver/lib/subdomains.py:79:28: PLC1901 `split_url.netloc == ""` can be simplified to `not split_url.netloc` as an empty string is falsey
+ zerver/lib/subdomains.py:79:28: PLC1901 `split_url.netloc == ""` can be simplified to `split_url.netloc` as an empty string is falsey
- zerver/lib/subdomains.py:79:55: PLC1901 `split_url.scheme == ""` can be simplified to `not split_url.scheme` as an empty string is falsey
+ zerver/lib/subdomains.py:79:55: PLC1901 `split_url.scheme == ""` can be simplified to `split_url.scheme` as an empty string is falsey
+ zerver/lib/upload/__init__.py:41:48: PLC1901 `content_type == ""` can be simplified to `content_type` as an empty string is falsey
- zerver/lib/upload/__init__.py:41:48: PLC1901 `content_type == ""` can be simplified to `not content_type` as an empty string is falsey
+ zerver/lib/url_preview/parsers/generic.py:20:46: PLC1901 `soup.title.text != ""` can be simplified to `not soup.title.text` as an empty string is falsey
- zerver/lib/url_preview/parsers/generic.py:20:46: PLC1901 `soup.title.text != ""` can be simplified to `soup.title.text` as an empty string is falsey
+ zerver/lib/url_preview/parsers/generic.py:22:40: PLC1901 `soup.h1.text != ""` can be simplified to `not soup.h1.text` as an empty string is falsey
- zerver/lib/url_preview/parsers/generic.py:22:40: PLC1901 `soup.h1.text != ""` can be simplified to `soup.h1.text` as an empty string is falsey
- zerver/lib/url_preview/parsers/generic.py:29:89: PLC1901 `meta_description.get("content", "") != ""` can be simplified to `meta_description.get("content", "")` as an empty string is falsey
+ zerver/lib/url_preview/parsers/generic.py:29:89: PLC1901 `meta_description.get("content", "") != ""` can be simplified to `not meta_description.get("content", "")` as an empty string is falsey
- zerver/lib/url_preview/parsers/generic.py:35:44: PLC1901 `first_p.text != ""` can be simplified to `first_p.text` as an empty string is falsey
+ zerver/lib/url_preview/parsers/generic.py:35:44: PLC1901 `first_p.text != ""` can be simplified to `not first_p.text` as an empty string is falsey
- zerver/lib/url_preview/parsers/generic.py:38:40: PLC1901 `first_p.text != ""` can be simplified to `first_p.text` as an empty string is falsey
+ zerver/lib/url_preview/parsers/generic.py:38:40: PLC1901 `first_p.text != ""` can be simplified to `not first_p.text` as an empty string is falsey
- zerver/lib/url_preview/parsers/generic.py:51:71: PLC1901 `first_image["src"] != ""` can be simplified to `first_image["src"]` as an empty string is falsey
+ zerver/lib/url_preview/parsers/generic.py:51:71: PLC1901 `first_image["src"] != ""` can be simplified to `not first_image["src"]` as an empty string is falsey
+ zerver/lib/utils.py:42:40: PLC1901 `settings.STATSD_HOST != ""` can be simplified to `not settings.STATSD_HOST` as an empty string is falsey
- zerver/lib/utils.py:42:40: PLC1901 `settings.STATSD_HOST != ""` can be simplified to `settings.STATSD_HOST` as an empty string is falsey
- zerver/lib/zephyr.py:14:31: PLC1901 `hesiod_name != ""` can be simplified to `hesiod_name` as an empty string is falsey
+ zerver/lib/zephyr.py:14:31: PLC1901 `hesiod_name != ""` can be simplified to `not hesiod_name` as an empty string is falsey
- zerver/management/commands/compilemessages.py:150:29: PLC1901 `value == ""` can be simplified to `not value` as an empty string is falsey
+ zerver/management/commands/compilemessages.py:150:29: PLC1901 `value == ""` can be simplified to `value` as an empty string is falsey
+ zerver/management/commands/list_realms.py:34:75: PLC1901 `realm.string_id != ""` can be simplified to `not realm.string_id` as an empty string is falsey
- zerver/management/commands/list_realms.py:34:75: PLC1901 `realm.string_id != ""` can be simplified to `realm.string_id` as an empty string is falsey
- zerver/management/commands/register_server.py:77:68: PLC1901 `tos_prompt.lower() == ""` can be simplified to `not tos_prompt.lower()` as an empty string is falsey
+ zerver/management/commands/register_server.py:77:68: PLC1901 `tos_prompt.lower() == ""` can be simplified to `tos_prompt.lower()` as an empty string is falsey
+ zerver/middleware.py:167:32: PLC1901 `settings.STATSD_HOST != ""` can be simplified to `not settings.STATSD_HOST` as an empty string is falsey
- zerver/middleware.py:167:32: PLC1901 `settings.STATSD_HOST != ""` can be simplified to `settings.STATSD_HOST` as an empty string is falsey
+ zerver/migrations/0371_invalid_characters_in_topics.py:53:35: PLC1901 `fixed_topic == ""` can be simplified to `fixed_topic` as an empty string is falsey
- zerver/migrations/0371_invalid_characters_in_topics.py:53:35: PLC1901 `fixed_topic == ""` can be simplified to `not fixed_topic` as an empty string is falsey
- zerver/migrations/0373_fix_deleteduser_dummies.py:13:21: PLC1901 `subdomain == ""` can be simplified to `not subdomain` as an empty string is falsey
+ zerver/migrations/0373_fix_deleteduser_dummies.py:13:21: PLC1901 `subdomain == ""` can be simplified to `subdomain` as an empty string is falsey
+ zerver/migrations/0375_invalid_characters_in_stream_names.py:48:37: PLC1901 `fixed_stream_name == ""` can be simplified to `fixed_stream_name` as an empty string is falsey
- zerver/migrations/0375_invalid_characters_in_stream_names.py:48:37: PLC1901 `fixed_stream_name == ""` can be simplified to `not fixed_stream_name` as an empty string is falsey
- zerver/models.py:932:30: PLC1901 `self.string_id == ""` can be simplified to `not self.string_id` as an empty string is falsey
+ zerver/models.py:932:30: PLC1901 `self.string_id == ""` can be simplified to `self.string_id` as an empty string is falsey
- zerver/signals.py:87:23: PLC1901 `user_tz == ""` can be simplified to `not user_tz` as an empty string is falsey
+ zerver/signals.py:87:23: PLC1901 `user_tz == ""` can be simplified to `user_tz` as an empty string is falsey
- zerver/tests/test_auth_backends.py:927:25: PLC1901 `subdomain == ""` can be simplified to `not subdomain` as an empty string is falsey
+ zerver/tests/test_auth_backends.py:927:25: PLC1901 `subdomain == ""` can be simplified to `subdomain` as an empty string is falsey
+ zerver/tests/test_email_notifications.py:469:46: PLC1901 `settings.EMAIL_GATEWAY_PATTERN != ""` can be simplified to `not settings.EMAIL_GATEWAY_PATTERN` as an empty string is falsey
- zerver/tests/test_email_notifications.py:469:46: PLC1901 `settings.EMAIL_GATEWAY_PATTERN != ""` can be simplified to `settings.EMAIL_GATEWAY_PATTERN` as an empty string is falsey
- zerver/tests/test_message_send.py:1372:58: PLC1901 `topic_name == ""` can be simplified to `not topic_name` as an empty string is falsey
+ zerver/tests/test_message_send.py:1372:58: PLC1901 `topic_name == ""` can be simplified to `topic_name` as an empty string is falsey
+ zerver/tests/test_outgoing_webhook_system.py:102:27: PLC1901 `content == ""` can be simplified to `content` as an empty string is falsey
- zerver/tests/test_outgoing_webhook_system.py:102:27: PLC1901 `content == ""` can be simplified to `not content` as an empty string is falsey
- zerver/tests/test_upload.py:941:39: PLC1901 `content_disposition != ""` can be simplified to `content_disposition` as an empty string is falsey
+ zerver/tests/test_upload.py:941:39: PLC1901 `content_disposition != ""` can be simplified to `not content_disposition` as an empty string is falsey
+ zerver/views/alert_words.py:19:44: PLC1901 `w != ""` can be simplified to `not w` as an empty string is falsey
- zerver/views/alert_words.py:19:44: PLC1901 `w != ""` can be simplified to `w` as an empty string is falsey
+ zerver/views/documentation.py:76:23: PLC1901 `article == ""` can be simplified to `article` as an empty string is falsey
- zerver/views/documentation.py:76:23: PLC1901 `article == ""` can be simplified to `not article` as an empty string is falsey
+ zerver/views/presence.py:87:22: PLC1901 `emoji_name == ""` can be simplified to `emoji_name` as an empty string is falsey
- zerver/views/presence.py:87:22: PLC1901 `emoji_name == ""` can be simplified to `not emoji_name` as an empty string is falsey
- zerver/views/push_notifications.py:18:21: PLC1901 `token_str == ""` can be simplified to `not token_str` as an empty string is falsey
+ zerver/views/push_notifications.py:18:21: PLC1901 `token_str == ""` can be simplified to `token_str` as an empty string is falsey
+ zerver/views/realm.py:329:43: PLC1901 `default_code_block_language == ""` can be simplified to `default_code_block_language` as an empty string is falsey
- zerver/views/realm.py:329:43: PLC1901 `default_code_block_language == ""` can be simplified to `not default_code_block_language` as an empty string is falsey
- zerver/views/streams.py:781:42: PLC1901 `stream.description == ""` can be simplified to `not stream.description` as an empty string is falsey
+ zerver/views/streams.py:781:42: PLC1901 `stream.description == ""` can be simplified to `stream.description` as an empty string is falsey
- zerver/views/users.py:243:89: PLC1901 `full_name.strip() != ""` can be simplified to `full_name.strip()` as an empty string is falsey
+ zerver/views/users.py:243:89: PLC1901 `full_name.strip() != ""` can be simplified to `not full_name.strip()` as an empty string is falsey
+ zerver/views/users.py:390:38: PLC1901 `default_sending_stream == ""` can be simplified to `default_sending_stream` as an empty string is falsey
- zerver/views/users.py:390:38: PLC1901 `default_sending_stream == ""` can be simplified to `not default_sending_stream` as an empty string is falsey
+ zerver/views/users.py:396:46: PLC1901 `default_events_register_stream == ""` can be simplified to `default_events_register_stream` as an empty string is falsey
- zerver/views/users.py:396:46: PLC1901 `default_events_register_stream == ""` can be simplified to `not default_events_register_stream` as an empty string is falsey
+ zerver/webhooks/clubhouse/view.py:470:26: PLC1901 `label_name == ""` can be simplified to `label_name` as an empty string is falsey
- zerver/webhooks/clubhouse/view.py:470:26: PLC1901 `label_name == ""` can be simplified to `not label_name` as an empty string is falsey
- zerver/webhooks/jira/view.py:130:26: PLC1901 `assignee_email != ""` can be simplified to `assignee_email` as an empty string is falsey
+ zerver/webhooks/jira/view.py:130:26: PLC1901 `assignee_email != ""` can be simplified to `not assignee_email` as an empty string is falsey
- zerver/webhooks/jira/view.py:175:21: PLC1901 `sub_event == ""` can be simplified to `not sub_event` as an empty string is falsey
+ zerver/webhooks/jira/view.py:175:21: PLC1901 `sub_event == ""` can be simplified to `sub_event` as an empty string is falsey
- zerver/webhooks/jira/view.py:213:28: PLC1901 `assignee_mention != ""` can be simplified to `assignee_mention` as an empty string is falsey
+ zerver/webhooks/jira/view.py:213:28: PLC1901 `assignee_mention != ""` can be simplified to `not assignee_mention` as an empty string is falsey
- zerver/webhooks/jira/view.py:249:64: PLC1901 `assignee_mention != ""` can be simplified to `assignee_mention` as an empty string is falsey
+ zerver/webhooks/jira/view.py:249:64: PLC1901 `assignee_mention != ""` can be simplified to `not assignee_mention` as an empty string is falsey
- zerver/webhooks/newrelic/view.py:137:29: PLC1901 `info["owner"] != ""` can be simplified to `info["owner"]` as an empty string is falsey
+ zerver/webhooks/newrelic/view.py:137:29: PLC1901 `info["owner"] != ""` can be simplified to `not info["owner"]` as an empty string is falsey
- zerver/webhooks/newrelic/view.py:86:29: PLC1901 `info["owner"] != ""` can be simplified to `info["owner"]` as an empty string is falsey
+ zerver/webhooks/newrelic/view.py:86:29: PLC1901 `info["owner"] != ""` can be simplified to `not info["owner"]` as an empty string is falsey
+ zerver/webhooks/opbeat/view.py:69:20: PLC1901 `title[0] != ""` can be simplified to `not title[0]` as an empty string is falsey
- zerver/webhooks/opbeat/view.py:69:20: PLC1901 `title[0] != ""` can be simplified to `title[0]` as an empty string is falsey
- zerver/webhooks/pivotal/view.py:59:24: PLC1901 `estimate != ""` can be simplified to `estimate` as an empty string is falsey
+ zerver/webhooks/pivotal/view.py:59:24: PLC1901 `estimate != ""` can be simplified to `not estimate` as an empty string is falsey
- zerver/webhooks/raygun/view.py:104:56: PLC1901 `message != ""` can be simplified to `message` as an empty string is falsey
+ zerver/webhooks/raygun/view.py:104:56: PLC1901 `message != ""` can be simplified to `not message` as an empty string is falsey
- zerver/webhooks/sentry/view.py:103:31: PLC1901 `syntax_highlight_as == ""` can be simplified to `not syntax_highlight_as` as an empty string is falsey
+ zerver/webhooks/sentry/view.py:103:31: PLC1901 `syntax_highlight_as == ""` can be simplified to `syntax_highlight_as` as an empty string is falsey
+ zerver/webhooks/sentry/view.py:87:24: PLC1901 `line == ""` can be simplified to `line` as an empty string is falsey
- zerver/webhooks/sentry/view.py:87:24: PLC1901 `line == ""` can be simplified to `not line` as an empty string is falsey
+ zerver/webhooks/slack_incoming/view.py:103:82: PLC1901 `piece.strip() != ""` can be simplified to `not piece.strip()` as an empty string is falsey
- zerver/webhooks/slack_incoming/view.py:103:82: PLC1901 `piece.strip() != ""` can be simplified to `piece.strip()` as an empty string is falsey
+ zerver/webhooks/slack_incoming/view.py:147:82: PLC1901 `piece.strip() != ""` can be simplified to `not piece.strip()` as an empty string is falsey
- zerver/webhooks/slack_incoming/view.py:147:82: PLC1901 `piece.strip() != ""` can be simplified to `piece.strip()` as an empty string is falsey
+ zerver/webhooks/slack_incoming/view.py:231:78: PLC1901 `piece.strip() != ""` can be simplified to `not piece.strip()` as an empty string is falsey
- zerver/webhooks/slack_incoming/view.py:231:78: PLC1901 `piece.strip() != ""` can be simplified to `piece.strip()` as an empty string is falsey
+ zerver/webhooks/slack_incoming/view.py:70:78: PLC1901 `piece.strip() != ""` can be simplified to `not piece.strip()` as an empty string is falsey
- zerver/webhooks/slack_incoming/view.py:70:78: PLC1901 `piece.strip() != ""` can be simplified to `piece.strip()` as an empty string is falsey
+ zerver/webhooks/slack_incoming/view.py:72:16: PLC1901 `body == ""` can be simplified to `body` as an empty string is falsey
- zerver/webhooks/slack_incoming/view.py:72:16: PLC1901 `body == ""` can be simplified to `not body` as an empty string is falsey
- zerver/webhooks/slack_incoming/view.py:78:16: PLC1901 `body != ""` can be simplified to `body` as an empty string is falsey
+ zerver/webhooks/slack_incoming/view.py:78:16: PLC1901 `body != ""` can be simplified to `not body` as an empty string is falsey
- zerver/webhooks/thinkst/view.py:52:80: PLC1901 `message["ReverseDNS"].tame(check_string) != ""` can be simplified to `message["ReverseDNS"].tame(check_string)` as an empty string is falsey
+ zerver/webhooks/thinkst/view.py:52:80: PLC1901 `message["ReverseDNS"].tame(check_string) != ""` can be simplified to `not message["ReverseDNS"].tame(check_string)` as an empty string is falsey
- zerver/webhooks/trello/view/card_actions.py:94:70: PLC1901 `old_data.get("desc").tame(check_none_or(check_string)) == ""` can be simplified to `not old_data.get("desc").tame(check_none_or(check_string))` as an empty string is falsey
+ zerver/webhooks/trello/view/card_actions.py:94:70: PLC1901 `old_data.get("desc").tame(check_none_or(check_string)) == ""` can be simplified to `old_data.get("desc").tame(check_none_or(check_string))` as an empty string is falsey
+ zerver/webhooks/trello/view/card_actions.py:97:75: PLC1901 `card_data.get("desc").tame(check_none_or(check_string)) == ""` can be simplified to `card_data.get("desc").tame(check_none_or(check_string))` as an empty string is falsey
- zerver/webhooks/trello/view/card_actions.py:97:75: PLC1901 `card_data.get("desc").tame(check_none_or(check_string)) == ""` can be simplified to `not card_data.get("desc").tame(check_none_or(check_string))` as an empty string is falsey
- zproject/backends.py:414:20: PLC1901 `password == ""` can be simplified to `not password` as an empty string is falsey
+ zproject/backends.py:414:20: PLC1901 `password == ""` can be simplified to `password` as an empty string is falsey
- zproject/backends.py:454:24: PLC1901 `password == ""` can be simplified to `not password` as an empty string is falsey
+ zproject/backends.py:454:24: PLC1901 `password == ""` can be simplified to `password` as an empty string is falsey
- zproject/computed_settings.py:296:30: PLC1901 `REMOTE_POSTGRES_HOST != ""` can be simplified to `REMOTE_POSTGRES_HOST` as an empty string is falsey
+ zproject/computed_settings.py:296:30: PLC1901 `REMOTE_POSTGRES_HOST != ""` can be simplified to `not REMOTE_POSTGRES_HOST` as an empty string is falsey
- zproject/computed_settings.py:305:35: PLC1901 `REMOTE_POSTGRES_SSLMODE != ""` can be simplified to `REMOTE_POSTGRES_SSLMODE` as an empty string is falsey
+ zproject/computed_settings.py:305:35: PLC1901 `REMOTE_POSTGRES_SSLMODE != ""` can be simplified to `not REMOTE_POSTGRES_SSLMODE` as an empty string is falsey
- zproject/computed_settings.py:525:19: PLC1901 `STATSD_HOST != ""` can be simplified to `STATSD_HOST` as an empty string is falsey
+ zproject/computed_settings.py:525:19: PLC1901 `STATSD_HOST != ""` can be simplified to `not STATSD_HOST` as an empty string is falsey
- zproject/computed_settings.py:535:50: PLC1901 `CAMO_URI != ""` can be simplified to `CAMO_URI` as an empty string is falsey
+ zproject/computed_settings.py:535:50: PLC1901 `CAMO_URI != ""` can be simplified to `not CAMO_URI` as an empty string is falsey
```

</p>
</details>
<details><summary>bokeh (+92, -92)</summary>
<p>

```diff
- examples/server/app/movies/main.py:82:25: PLC1901 `director_val != ''` can be simplified to `director_val` as an empty string is falsey
+ examples/server/app/movies/main.py:82:25: PLC1901 `director_val != ''` can be simplified to `not director_val` as an empty string is falsey
- examples/server/app/movies/main.py:84:21: PLC1901 `cast_val != ''` can be simplified to `cast_val` as an empty string is falsey
+ examples/server/app/movies/main.py:84:21: PLC1901 `cast_val != ''` can be simplified to `not cast_val` as an empty string is falsey
+ release/checks.py:102:45: PLC1901 `x != ""` can be simplified to `not x` as an empty string is falsey
- release/checks.py:102:45: PLC1901 `x != ""` can be simplified to `x` as an empty string is falsey
+ src/bokeh/command/subcommands/serve.py:967:69: PLC1901 `server.address != ''` can be simplified to `not server.address` as an empty string is falsey
- src/bokeh/command/subcommands/serve.py:967:69: PLC1901 `server.address != ''` can be simplified to `server.address` as an empty string is falsey
+ src/bokeh/command/subcommands/static.py:86:65: PLC1901 `server.address != ''` can be simplified to `not server.address` as an empty string is falsey
- src/bokeh/command/subcommands/static.py:86:65: PLC1901 `server.address != ''` can be simplified to `server.address` as an empty string is falsey
- src/bokeh/plotting/_tools.py:189:20: PLC1901 `tool == ''` can be simplified to `not tool` as an empty string is falsey
+ src/bokeh/plotting/_tools.py:189:20: PLC1901 `tool == ''` can be simplified to `tool` as an empty string is falsey
+ src/bokeh/server/server.py:275:57: PLC1901 `self.address != ''` can be simplified to `not self.address` as an empty string is falsey
- src/bokeh/server/server.py:275:57: PLC1901 `self.address != ''` can be simplified to `self.address` as an empty string is falsey
- src/bokeh/server/util.py:151:28: PLC1901 `parts[0] == ''` can be simplified to `not parts[0]` as an empty string is falsey
+ src/bokeh/server/util.py:151:28: PLC1901 `parts[0] == ''` can be simplified to `parts[0]` as an empty string is falsey
- src/bokeh/server/util.py:159:28: PLC1901 `parts[0] == ''` can be simplified to `not parts[0]` as an empty string is falsey
+ src/bokeh/server/util.py:159:28: PLC1901 `parts[0] == ''` can be simplified to `parts[0]` as an empty string is falsey
- tests/integration/widgets/test_autocomplete_input.py:266:36: PLC1901 `text_input.value == ""` can be simplified to `not text_input.value` as an empty string is falsey
+ tests/integration/widgets/test_autocomplete_input.py:266:36: PLC1901 `text_input.value == ""` can be simplified to `text_input.value` as an empty string is falsey
+ tests/integration/widgets/test_autocomplete_input.py:86:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `el.get_attribute("placeholder")` as an empty string is falsey
- tests/integration/widgets/test_autocomplete_input.py:86:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `not el.get_attribute("placeholder")` as an empty string is falsey
+ tests/integration/widgets/test_multi_choice.py:78:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `el.get_attribute("placeholder")` as an empty string is falsey
- tests/integration/widgets/test_multi_choice.py:78:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `not el.get_attribute("placeholder")` as an empty string is falsey
+ tests/integration/widgets/test_numeric_input.py:86:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_numeric_input.py:86:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_password_input.py:72:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `el.get_attribute("placeholder")` as an empty string is falsey
- tests/integration/widgets/test_password_input.py:72:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `not el.get_attribute("placeholder")` as an empty string is falsey
+ tests/integration/widgets/test_password_input.py:82:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_password_input.py:82:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:117:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:117:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:135:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:135:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:157:33: PLC1901 `label_el.text == ""` can be simplified to `label_el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:157:33: PLC1901 `label_el.text == ""` can be simplified to `not label_el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:181:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:181:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:203:33: PLC1901 `label_el.text == ""` can be simplified to `label_el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:203:33: PLC1901 `label_el.text == ""` can be simplified to `not label_el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:62:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:62:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:80:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:80:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_select.py:99:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_select.py:99:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/integration/widgets/test_text_input.py:72:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `el.get_attribute("placeholder")` as an empty string is falsey
- tests/integration/widgets/test_text_input.py:72:51: PLC1901 `el.get_attribute("placeholder") == ""` can be simplified to `not el.get_attribute("placeholder")` as an empty string is falsey
+ tests/integration/widgets/test_text_input.py:82:27: PLC1901 `el.text == ""` can be simplified to `el.text` as an empty string is falsey
- tests/integration/widgets/test_text_input.py:82:27: PLC1901 `el.text == ""` can be simplified to `not el.text` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_info.py:77:24: PLC1901 `lines[8] == ""` can be simplified to `lines[8]` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_info.py:77:24: PLC1901 `lines[8] == ""` can be simplified to `not lines[8]` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_info.py:78:19: PLC1901 `err == ""` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_info.py:78:19: PLC1901 `err == ""` can be simplified to `not err` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_info.py:83:19: PLC1901 `err == ""` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_info.py:83:19: PLC1901 `err == ""` can be simplified to `not err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:101:23: PLC1901 `out == ""` can be simplified to `not out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_json__subcommands.py:101:23: PLC1901 `out == ""` can be simplified to `out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_json__subcommands.py:108:23: PLC1901 `err == ""` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:108:23: PLC1901 `err == ""` can be simplified to `not err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:109:23: PLC1901 `out == ""` can be simplified to `not out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_json__subcommands.py:109:23: PLC1901 `out == ""` can be simplified to `out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_json__subcommands.py:120:23: PLC1901 `err == ""` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:120:23: PLC1901 `err == ""` can be simplified to `not err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:121:23: PLC1901 `out == ""` can be simplified to `not out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_json__subcommands.py:121:23: PLC1901 `out == ""` can be simplified to `out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_json__subcommands.py:132:23: PLC1901 `err == ""` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:132:23: PLC1901 `err == ""` can be simplified to `not err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:133:23: PLC1901 `out == ""` can be simplified to `not out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_json__subcommands.py:133:23: PLC1901 `out == ""` can be simplified to `out` as an empty string is falsey
+ tests/unit/bokeh/command/subcommands/test_secret.py:58:19: PLC1901 `err == ""` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/command/subcommands/test_secret.py:58:19: PLC1901 `err == ""` can be simplified to `not err` as an empty string is falsey
- tests/unit/bokeh/command/test_bootstrap.py:54:19: PLC1901 `out == ""` can be simplified to `not out` as an empty string is falsey
+ tests/unit/bokeh/command/test_bootstrap.py:54:19: PLC1901 `out == ""` can be simplified to `out` as an empty string is falsey
- tests/unit/bokeh/command/test_util__command.py:47:19: PLC1901 `out == ""` can be simplified to `not out` as an empty string is falsey
+ tests/unit/bokeh/command/test_util__command.py:47:19: PLC1901 `out == ""` can be simplified to `out` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_bases.py:125:23: PLC1901 `err == ''` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/core/property/test_bases.py:125:23: PLC1901 `err == ''` can be simplified to `not err` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_bases.py:137:23: PLC1901 `err == ''` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/core/property/test_bases.py:137:23: PLC1901 `err == ''` can be simplified to `not err` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_bases.py:163:23: PLC1901 `err == ''` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/core/property/test_bases.py:163:23: PLC1901 `err == ''` can be simplified to `not err` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_bases.py:178:23: PLC1901 `err == ''` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/core/property/test_bases.py:178:23: PLC1901 `err == ''` can be simplified to `not err` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_bases.py:195:23: PLC1901 `err == ''` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/core/property/test_bases.py:195:23: PLC1901 `err == ''` can be simplified to `not err` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_bases.py:212:23: PLC1901 `err == ''` can be simplified to `err` as an empty string is falsey
- tests/unit/bokeh/core/property/test_bases.py:212:23: PLC1901 `err == ''` can be simplified to `not err` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:270:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:270:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:275:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:275:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:280:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:280:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:285:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:285:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:290:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:290:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:295:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:295:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:300:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:300:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:305:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:305:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:312:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:312:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:317:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:317:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:322:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:322:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:327:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:327:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:334:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:334:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:339:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:339:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:344:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:344:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:349:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:349:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:354:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:354:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:359:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:359:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:364:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:364:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:369:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:369:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:374:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:374:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:379:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:379:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:384:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:384:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:389:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:389:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
- tests/unit/bokeh/core/property/test_validation__property.py:397:33: PLC1901 `str(e.value) == ''` can be simplified to `not str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/core/property/test_validation__property.py:397:33: PLC1901 `str(e.value) == ''` can be simplified to `str(e.value)` as an empty string is falsey
+ tests/unit/bokeh/document/test_callbacks__document.py:142:35: PLC1901 `caplog.text == ""` can be simplified to `caplog.text` as an empty string is falsey
- tests/unit/bokeh/document/test_callbacks__document.py:142:35: PLC1901 `caplog.text == ""` can be simplified to `not caplog.text` as an empty string is falsey
+ tests/unit/bokeh/document/test_callbacks__document.py:146:35: PLC1901 `caplog.text == ""` can be simplified to `caplog.text` as an empty string is falsey
- tests/unit/bokeh/document/test_callbacks__document.py:146:35: PLC1901 `caplog.text == ""` can be simplified to `not caplog.text` as an empty string is falsey
+ tests/unit/bokeh/embed/test_server__embed.py:209:48: PLC1901 `bes._process_arguments(None) == ""` can be simplified to `bes._process_arguments(None)` as an empty string is falsey
- tests/unit/bokeh/embed/test_server__embed.py:209:48: PLC1901 `bes._process_arguments(None) == ""` can be simplified to `not bes._process_arguments(None)` as an empty string is falsey
+ tests/unit/bokeh/embed/test_server__embed.py:227:46: PLC1901 `bes._process_app_path("/") == ""` can be simplified to `bes._process_app_path("/")` as an empty string is falsey
- tests/unit/bokeh/embed/test_server__embed.py:227:46: PLC1901 `bes._process_app_path("/") == ""` can be simplified to `not bes._process_app_path("/")` as an empty string is falsey
+ tests/unit/bokeh/embed/test_server__embed.py:235:56: PLC1901 `bes._process_relative_urls(True, "") == ""` can be simplified to `bes._process_relative_urls(True, "")` as an empty string is falsey
- tests/unit/bokeh/embed/test_server__embed.py:235:56: PLC1901 `bes._process_relative_urls(True, "") == ""` can be simplified to `not bes._process_relative_urls(True, "")` as an empty string is falsey
+ tests/unit/bokeh/embed/test_server__embed.py:236:62: PLC1901 `bes._process_relative_urls(True, "/stuff") == ""` can be simplified to `bes._process_relative_urls(True, "/stuff")` as an empty string is falsey
- tests/unit/bokeh/embed/test_server__embed.py:236:62: PLC1901 `bes._process_relative_urls(True, "/stuff") == ""` can be simplified to `not bes._process_relative_urls(True, "/stuff")` as an empty string is falsey
+ tests/unit/bokeh/embed/test_server__embed.py:251:53: PLC1901 `bes._process_resources("default") == ""` can be simplified to `bes._process_resources("default")` as an empty string is falsey
- tests/unit/bokeh/embed/test_server__embed.py:251:53: PLC1901 `bes._process_resources("default") == ""` can be simplified to `not bes._process_resources("default")` as an empty string is falsey
- tests/unit/bokeh/embed/test_standalone.py:119:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `not script.get_attribute("integrity")` as an empty string is falsey
+ tests/unit/bokeh/embed/test_standalone.py:119:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `script.get_attribute("integrity")` as an empty string is falsey
- tests/unit/bokeh/embed/test_standalone.py:145:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `not script.get_attribute("integrity")` as an empty string is falsey
+ tests/unit/bokeh/embed/test_standalone.py:145:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `script.get_attribute("integrity")` as an empty string is falsey
- tests/unit/bokeh/embed/test_standalone.py:168:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `not script.get_attribute("integrity")` as an empty string is falsey
+ tests/unit/bokeh/embed/test_standalone.py:168:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `script.get_attribute("integrity")` as an empty string is falsey
- tests/unit/bokeh/embed/test_standalone.py:189:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `not script.get_attribute("integrity")` as an empty string is falsey
+ tests/unit/bokeh/embed/test_standalone.py:189:57: PLC1901 `script.get_attribute("integrity") == ""` can be simplified to `script.get_attribute("integrity")` as an empty string is falsey
- tests/unit/bokeh/embed/test_standalone.py:398:39: PLC1901 `out["doc"]["title"] == ""` can be simplified to `not out["doc"]["title"]` as an empty string is falsey
+ tests/unit/bokeh/embed/test_standalone.py:398:39: PLC1901 `out["doc"]["title"] == ""` can be simplified to `out["doc"]["title"]` as an empty string is falsey
- tests/unit/bokeh/embed/test_util__embed.py:549:35: PLC1901 `caplog.text != ''` can be simplified to `caplog.text` as an empty string is falsey
+ tests/unit/bokeh/embed/test_util__embed.py:549:35: PLC1901 `caplog.text != ''` can be simplified to `not caplog.text` as an empty string is falsey
- tests/unit/bokeh/embed/test_util__embed.py:563:35: PLC1901 `caplog.text != ''` can be simplified to `caplog.text` as an empty string is falsey
+ tests/unit/bokeh/embed/test_util__embed.py:563:35: PLC1901 `caplog.text != ''` can be simplified to `not caplog.text` as an empty string is falsey
+ tests/unit/bokeh/embed/test_util__embed.py:581:35: PLC1901 `caplog.text == ''` can be simplified to `caplog.text` as an empty string is falsey
- tests/unit/bokeh/embed/test_util__embed.py:581:35: PLC1901 `caplog.text == ''` can be simplified to `not caplog.text` as an empty string is falsey
+ tests/unit/bokeh/models/test_annotations.py:320:26: PLC1901 `label.text == ""` can be simplified to `label.text` as an empty string is falsey
- tests/unit/bokeh/models/test_annotations.py:320:26: PLC1901 `label.text == ""` can be simplified to `not label.text` as an empty string is falsey
- tests/unit/bokeh/models/test_annotations.py:470:26: PLC1901 `title.text == ""` can be simplified to `not title.text` as an empty string is falsey
+ tests/unit/bokeh/models/test_annotations.py:470:26: PLC1901 `title.text == ""` can be simplified to `title.text` as an empty string is falsey
- tests/unit/bokeh/models/test_plots.py:270:31: PLC1901 `plot.title.text == ''` can be simplified to `not plot.title.text` as an empty string is falsey
+ tests/unit/bokeh/models/test_plots.py:270:31: PLC1901 `plot.title.text == ''` can be simplified to `plot.title.text` as an empty string is falsey
+ tests/unit/bokeh/models/test_transforms.py:38:40: PLC1901 `custom_js_transform.func == ""` can be simplified to `custom_js_transform.func` as an empty string is falsey
- tests/unit/bokeh/models/test_transforms.py:38:40: PLC1901 `custom_js_transform.func == ""` can be simplified to `not custom_js_transform.func` as an empty string is falsey
+ tests/unit/bokeh/models/test_transforms.py:39:42: PLC1901 `custom_js_transform.v_func == ""` can be simplified to `custom_js_transform.v_func` as an empty string is falsey
- tests/unit/bokeh/models/test_transforms.py:39:42: PLC1901 `custom_js_transform.v_func == ""` can be simplified to `not custom_js_transform.v_func` as an empty string is falsey
- tests/unit/bokeh/server/test_server__server.py:152:33: PLC1901 `server.prefix == ""` can be simplified to `not server.prefix` as an empty string is falsey
+ tests/unit/bokeh/server/test_server__server.py:152:33: PLC1901 `server.prefix == ""` can be simplified to `server.prefix` as an empty string is falsey
- tests/unit/bokeh/server/test_tornado__server.py:114:42: PLC1901 `server._tornado.prefix == ""` can be simplified to `not server._tornado.prefix` as an empty string is falsey
+ tests/unit/bokeh/server/test_tornado__server.py:114:42: PLC1901 `server._tornado.prefix == ""` can be simplified to `server._tornado.prefix` as an empty string is falsey
- tests/unit/bokeh/server/test_tornado__server.py:55:30: PLC1901 `r.root_url == ""` can be simplified to `not r.root_url` as an empty string is falsey
+ tests/unit/bokeh/server/test_tornado__server.py:55:30: PLC1901 `r.root_url == ""` can be simplified to `r.root_url` as an empty string is falsey
```

</p>
</details>
<details><summary>scikit-build (+7, -7)</summary>
<p>

```diff
+ skbuild/command/egg_info.py:33:32: PLC1901 `egg_base == ""` can be simplified to `egg_base` as an empty string is falsey
- skbuild/command/egg_info.py:33:32: PLC1901 `egg_base == ""` can be simplified to `not egg_base` as an empty string is falsey
- skbuild/platform_specifics/abstract.py:217:38: PLC1901 `suffix == ""` can be simplified to `not suffix` as an empty string is falsey
+ skbuild/platform_specifics/abstract.py:217:38: PLC1901 `suffix == ""` can be simplified to `suffix` as an empty string is falsey
+ skbuild/platform_specifics/abstract.py:219:38: PLC1901 `suffix != ""` can be simplified to `not suffix` as an empty string is falsey
- skbuild/platform_specifics/abstract.py:219:38: PLC1901 `suffix != ""` can be simplified to `suffix` as an empty string is falsey
+ skbuild/setuptools_wrap.py:300:58: PLC1901 `package != ""` can be simplified to `not package` as an empty string is falsey
- skbuild/setuptools_wrap.py:300:58: PLC1901 `package != ""` can be simplified to `package` as an empty string is falsey
- skbuild/setuptools_wrap.py:914:20: PLC1901 `dest_dir != ""` can be simplified to `dest_dir` as an empty string is falsey
+ skbuild/setuptools_wrap.py:914:20: PLC1901 `dest_dir != ""` can be simplified to `not dest_dir` as an empty string is falsey
+ skbuild/utils/__init__.py:145:27: PLC1901 `package_dir != ""` can be simplified to `not package_dir` as an empty string is falsey
- skbuild/utils/__init__.py:145:27: PLC1901 `package_dir != ""` can be simplified to `package_dir` as an empty string is falsey
- tests/test_skbuild.py:150:27: PLC1901 `vs_path == ""` can be simplified to `not vs_path` as an empty string is falsey
+ tests/test_skbuild.py:150:27: PLC1901 `vs_path == ""` can be simplified to `vs_path` as an empty string is falsey
```

</p>
</details>
<details><summary>airflow (+119, -119)</summary>
<p>

```diff
+ airflow/cli/commands/connection_command.py:136:36: PLC1901 `urlsplit(uri).scheme != ""` can be simplified to `not urlsplit(uri).scheme` as an empty string is falsey
- airflow/cli/commands/connection_command.py:136:36: PLC1901 `urlsplit(uri).scheme != ""` can be simplified to `urlsplit(uri).scheme` as an empty string is falsey
+ airflow/models/connection.py:236:30: PLC1901 `host_block == ""` can be simplified to `host_block` as an empty string is falsey
- airflow/models/connection.py:236:30: PLC1901 `host_block == ""` can be simplified to `not host_block` as an empty string is falsey
+ airflow/models/connection.py:236:56: PLC1901 `authority_block == ""` can be simplified to `authority_block` as an empty string is falsey
- airflow/models/connection.py:236:56: PLC1901 `authority_block == ""` can be simplified to `not authority_block` as an empty string is falsey
- airflow/providers/alibaba/cloud/sensors/oss_key.py:73:37: PLC1901 `parsed_url.netloc == ""` can be simplified to `not parsed_url.netloc` as an empty string is falsey
+ airflow/providers/alibaba/cloud/sensors/oss_key.py:73:37: PLC1901 `parsed_url.netloc == ""` can be simplified to `parsed_url.netloc` as an empty string is falsey
+ airflow/providers/alibaba/cloud/sensors/oss_key.py:79:37: PLC1901 `parsed_url.scheme != ""` can be simplified to `not parsed_url.scheme` as an empty string is falsey
- airflow/providers/alibaba/cloud/sensors/oss_key.py:79:37: PLC1901 `parsed_url.scheme != ""` can be simplified to `parsed_url.scheme` as an empty string is falsey
+ airflow/providers/alibaba/cloud/sensors/oss_key.py:79:64: PLC1901 `parsed_url.netloc != ""` can be simplified to `not parsed_url.netloc` as an empty string is falsey
- airflow/providers/alibaba/cloud/sensors/oss_key.py:79:64: PLC1901 `parsed_url.netloc != ""` can be simplified to `parsed_url.netloc` as an empty string is falsey
+ airflow/providers/amazon/aws/hooks/s3.py:219:33: PLC1901 `parsed_url.scheme != ""` can be simplified to `not parsed_url.scheme` as an empty string is falsey
- airflow/providers/amazon/aws/hooks/s3.py:219:33: PLC1901 `parsed_url.scheme != ""` can be simplified to `parsed_url.scheme` as an empty string is falsey
+ airflow/providers/amazon/aws/hooks/s3.py:219:60: PLC1901 `parsed_url.netloc != ""` can be simplified to `not parsed_url.netloc` as an empty string is falsey
- airflow/providers/amazon/aws/hooks/s3.py:219:60: PLC1901 `parsed_url.netloc != ""` can be simplified to `parsed_url.netloc` as an empty string is falsey
+ airflow/providers/amazon/aws/operators/eks.py:302:42: PLC1901 `self.nodegroup_subnets != ""` can be simplified to `not self.nodegroup_subnets` as an empty string is falsey
- airflow/providers/amazon/aws/operators/eks.py:302:42: PLC1901 `self.nodegroup_subnets != ""` can be simplified to `self.nodegroup_subnets` as an empty string is falsey
+ airflow/providers/apache/hive/hooks/hive.py:132:32: PLC1901 `proxy_user_value != ""` can be simplified to `not proxy_user_value` as an empty string is falsey
- airflow/providers/apache/hive/hooks/hive.py:132:32: PLC1901 `proxy_user_value != ""` can be simplified to `proxy_user_value` as an empty string is falsey
+ airflow/providers/apache/hive/transfers/mssql_to_hive.py:118:64: PLC1901 `field[0] == ""` can be simplified to `field[0]` as an empty string is falsey
- airflow/providers/apache/hive/transfers/mssql_to_hive.py:118:64: PLC1901 `field[0] == ""` can be simplified to `not field[0]` as an empty string is falsey
+ airflow/providers/apache/hive/transfers/vertica_to_hive.py:122:56: PLC1901 `field[0] == ""` can be simplified to `field[0]` as an empty string is falsey
- airflow/providers/apache/hive/transfers/vertica_to_hive.py:122:56: PLC1901 `field[0] == ""` can be simplified to `not field[0]` as an empty string is falsey
- airflow/providers/databricks/hooks/databricks_base.py:432:46: PLC1901 `self.databricks_conn.login == ""` can be simplified to `not self.databricks_conn.login` as an empty string is falsey
+ airflow/providers/databricks/hooks/databricks_base.py:432:46: PLC1901 `self.databricks_conn.login == ""` can be simplified to `self.databricks_conn.login` as an empty string is falsey
- airflow/providers/databricks/hooks/databricks_base.py:432:85: PLC1901 `self.databricks_conn.password == ""` can be simplified to `not self.databricks_conn.password` as an empty string is falsey
+ airflow/providers/databricks/hooks/databricks_base.py:432:85: PLC1901 `self.databricks_conn.password == ""` can be simplified to `self.databricks_conn.password` as an empty string is falsey
- airflow/providers/databricks/hooks/databricks_base.py:455:46: PLC1901 `self.databricks_conn.login == ""` can be simplified to `not self.databricks_conn.login` as an empty string is falsey
+ airflow/providers/databricks/hooks/databricks_base.py:455:46: PLC1901 `self.databricks_conn.login == ""` can be simplified to `self.databricks_conn.login` as an empty string is falsey
- airflow/providers/databricks/hooks/databricks_base.py:455:85: PLC1901 `self.databricks_conn.password == ""` can be simplified to `not self.databricks_conn.password` as an empty string is falsey
+ airflow/providers/databricks/hooks/databricks_base.py:455:85: PLC1901 `self.databricks_conn.password == ""` can be simplified to `self.databricks_conn.password` as an empty string is falsey
- airflow/providers/databricks/operators/databricks_sql.py:243:26: PLC1901 `table_name == ""` can be simplified to `not table_name` as an empty string is falsey
+ airflow/providers/databricks/operators/databricks_sql.py:243:26: PLC1901 `table_name == ""` can be simplified to `table_name` as an empty string is falsey
+ airflow/providers/databricks/operators/databricks_sql.py:245:29: PLC1901 `file_location == ""` can be simplified to `file_location` as an empty string is falsey
- airflow/providers/databricks/operators/databricks_sql.py:245:29: PLC1901 `file_location == ""` can be simplified to `not file_location` as an empty string is falsey
+ airflow/providers/google/cloud/hooks/cloud_sql.py:584:28: PLC1901 `line == ""` can be simplified to `line` as an empty string is falsey
- airflow/providers/google/cloud/hooks/cloud_sql.py:584:28: PLC1901 `line == ""` can be simplified to `not line` as an empty string is falsey
- airflow/providers/google/cloud/hooks/cloud_sql.py:589:28: PLC1901 `line != ""` can be simplified to `line` as an empty string is falsey
+ airflow/providers/google/cloud/hooks/cloud_sql.py:589:28: PLC1901 `line != ""` can be simplified to `not line` as an empty string is falsey
- airflow/providers/google/cloud/hooks/cloud_sql.py:785:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/hooks/cloud_sql.py:785:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/hooks/compute_ssh.py:155:60: PLC1901 `value.strip() == ""` can be simplified to `not value.strip()` as an empty string is falsey
+ airflow/providers/google/cloud/hooks/compute_ssh.py:155:60: PLC1901 `value.strip() == ""` can be simplified to `value.strip()` as an empty string is falsey
- airflow/providers/google/cloud/operators/cloud_sql.py:255:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/cloud_sql.py:255:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/compute.py:68:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/compute.py:68:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/spanner.py:178:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/spanner.py:178:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/spanner.py:259:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/spanner.py:259:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/spanner.py:306:39: PLC1901 `queries[-1] == ""` can be simplified to `not queries[-1]` as an empty string is falsey
+ airflow/providers/google/cloud/operators/spanner.py:306:39: PLC1901 `queries[-1] == ""` can be simplified to `queries[-1]` as an empty string is falsey
- airflow/providers/google/cloud/operators/spanner.py:370:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/spanner.py:370:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/spanner.py:478:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/spanner.py:478:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/spanner.py:569:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/spanner.py:569:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/spanner.py:98:31: PLC1901 `self.project_id == ""` can be simplified to `not self.project_id` as an empty string is falsey
+ airflow/providers/google/cloud/operators/spanner.py:98:31: PLC1901 `self.project_id == ""` can be simplified to `self.project_id` as an empty string is falsey
- airflow/providers/google/cloud/operators/speech_to_text.py:103:26: PLC1901 `self.audio == ""` can be simplified to `not self.audio` as an empty string is falsey
+ airflow/providers/google/cloud/operators/speech_to_text.py:103:26: PLC1901 `self.audio == ""` can be simplified to `self.audio` as an empty string is falsey
- airflow/providers/google/cloud/operators/speech_to_text.py:105:27: PLC1901 `self.config == ""` can be simplified to `not self.config` as an empty string is falsey
+ airflow/providers/google/cloud/operators/speech_to_text.py:105:27: PLC1901 `self.config == ""` can be simplified to `self.config` as an empty string is falsey
+ airflow/providers/google/cloud/operators/text_to_speech.py:123:44: PLC1901 `getattr(self, parameter) == ""` can be simplified to `getattr(self, parameter)` as an empty string is falsey
- airflow/providers/google/cloud/operators/text_to_speech.py:123:44: PLC1901 `getattr(self, parameter) == ""` can be simplified to `not getattr(self, parameter)` as an empty string is falsey
+ airflow/providers/google/common/hooks/base_google.py:361:68: PLC1901 `field_value.strip() == ""` can be simplified to `field_value.strip()` as an empty string is falsey
- airflow/providers/google/common/hooks/base_google.py:361:68: PLC1901 `field_value.strip() == ""` can be simplified to `not field_value.strip()` as an empty string is falsey
- airflow/providers/hashicorp/secrets/vault.py:178:37: PLC1901 `self.connections_path == ""` can be simplified to `not self.connections_path` as an empty string is falsey
+ airflow/providers/hashicorp/secrets/vault.py:178:37: PLC1901 `self.connections_path == ""` can be simplified to `self.connections_path` as an empty string is falsey
- airflow/providers/hashicorp/secrets/vault.py:240:35: PLC1901 `self.variables_path == ""` can be simplified to `not self.variables_path` as an empty string is falsey
+ airflow/providers/hashicorp/secrets/vault.py:240:35: PLC1901 `self.variables_path == ""` can be simplified to `self.variables_path` as an empty string is falsey
- airflow/providers/hashicorp/secrets/vault.py:259:32: PLC1901 `self.config_path == ""` can be simplified to `not self.config_path` as an empty string is falsey
+ airflow/providers/hashicorp/secrets/vault.py:259:32: PLC1901 `self.config_path == ""` can be simplified to `self.config_path` as an empty string is falsey
- airflow/providers/microsoft/azure/hooks/fileshare.py:132:25: PLC1901 `value == ""` can be simplified to `not value` as an empty string is falsey
+ airflow/providers/microsoft/azure/hooks/fileshare.py:132:25: PLC1901 `value == ""` can be simplified to `value` as an empty string is falsey
- airflow/providers/microsoft/azure/secrets/key_vault.py:172:27: PLC1901 `path_prefix == ""` can be simplified to `not path_prefix` as an empty string is falsey
+ airflow/providers/microsoft/azure/secrets/key_vault.py:172:27: PLC1901 `path_prefix == ""` can be simplified to `path_prefix` as an empty string is falsey
- airflow/providers/microsoft/azure/utils.py:72:15: PLC1901 `ret == ""` can be simplified to `not ret` as an empty string is falsey
+ airflow/providers/microsoft/azure/utils.py:72:15: PLC1901 `ret == ""` can be simplified to `ret` as an empty string is falsey
- airflow/providers/slack/transfers/sql_to_slack.py:179:52: PLC1901 `self.sql.strip() == ""` can be simplified to `not self.sql.strip()` as an empty string is falsey
+ airflow/providers/slack/transfers/sql_to_slack.py:179:52: PLC1901 `self.sql.strip() == ""` can be simplified to `self.sql.strip()` as an empty string is falsey
- airflow/providers/slack/transfers/sql_to_slack.py:181:72: PLC1901 `self.slack_message.strip() == ""` can be simplified to `not self.slack_message.strip()` as an empty string is falsey
+ airflow/providers/slack/transfers/sql_to_slack.py:181:72: PLC1901 `self.slack_message.strip() == ""` can be simplified to `self.slack_message.strip()` as an empty string is falsey
- airflow/stats.py:629:56: PLC1901 `tags_in_string == ""` can be simplified to `not tags_in_string` as an empty string is falsey
+ airflow/stats.py:629:56: PLC1901 `tags_in_string == ""` can be simplified to `tags_in_string` as an empty string is falsey
+ airflow/utils/helpers.py:94:22: PLC1901 `choice == ""` can be simplified to `choice` as an empty string is falsey
- airflow/utils/helpers.py:94:22: PLC1901 `choice == ""` can be simplified to `not choice` as an empty string is falsey
+ airflow/www/app.py:101:34: PLC1901 `cookie_samesite_config == ""` can be simplified to `cookie_samesite_config` as an empty string is falsey
- airflow/www/app.py:101:34: PLC1901 `cookie_samesite_config == ""` can be simplified to `not cookie_samesite_config` as an empty string is falsey
- airflow/www/fab_security/manager.py:1065:46: PLC1901 `username == ""` can be simplified to `not username` as an empty string is falsey
+ airflow/www/fab_security/manager.py:1065:46: PLC1901 `username == ""` can be simplified to `username` as an empty string is falsey
- airflow/www/fab_security/manager.py:1329:46: PLC1901 `username == ""` can be simplified to `not username` as an empty string is falsey
+ airflow/www/fab_security/manager.py:1329:46: PLC1901 `username == ""` can be simplified to `username` as an empty string is falsey
- airflow/www/fab_security/manager.py:910:44: PLC1901 `username == ""` can be simplified to `not username` as an empty string is falsey
+ airflow/www/fab_security/manager.py:910:44: PLC1901 `username == ""` can be simplified to `username` as an empty string is falsey
+ airflow/www/views.py:4538:33: PLC1901 `value != ""` can be simplified to `not value` as an empty string is falsey
- airflow/www/views.py:4538:33: PLC1901 `value != ""` can be simplified to `value` as an empty string is falsey
- dev/breeze/src/airflow_breeze/commands/setup_commands.py:367:30: PLC1901 `strip_line == ""` can be simplified to `not strip_line` as an empty string is falsey
+ dev/breeze/src/airflow_breeze/commands/setup_commands.py:367:30: PLC1901 `strip_line == ""` can be simplified to `strip_line` as an empty string is falsey
- dev/breeze/src/airflow_breeze/commands/setup_commands.py:385:34: PLC1901 `strip_line.strip() == ""` can be simplified to `not strip_line.strip()` as an empty string is falsey
+ dev/breeze/src/airflow_breeze/commands/setup_commands.py:385:34: PLC1901 `strip_line.strip() == ""` can be simplified to `strip_line.strip()` as an empty string is falsey
- dev/breeze/src/airflow_breeze/utils/confirm.py:79:35: PLC1901 `user_status == ""` can be simplified to `not user_status` as an empty string is falsey
+ dev/breeze/src/airflow_breeze/utils/confirm.py:79:35: PLC1901 `user_status == ""` can be simplified to `user_status` as an empty string is falsey
+ dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:244:30: PLC1901 `docker_version == ""` can be simplified to `docker_version` as an empty string is falsey
- dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:244:30: PLC1901 `docker_version == ""` can be simplified to `not docker_version` as an empty string is falsey
- dev/breeze/tests/test_find_airflow_directory.py:34:22: PLC1901 `output == ""` can be simplified to `not output` as an empty string is falsey
+ dev/breeze/tests/test_find_airflow_directory.py:34:22: PLC1901 `output == ""` can be simplified to `output` as an empty string is falsey
- dev/breeze/tests/test_find_airflow_directory.py:42:22: PLC1901 `output == ""` can be simplified to `not output` as an empty string is falsey
+ dev/breeze/tests/test_find_airflow_directory.py:42:22: PLC1901 `output == ""` can be simplified to `output` as an empty string is falsey
+ dev/provider_packages/prepare_provider_packages.py:1364:28: PLC1901 `old_text != ""` can be simplified to `not old_text` as an empty string is falsey
- dev/provider_packages/prepare_provider_packages.py:1364:28: PLC1901 `old_text != ""` can be simplified to `old_text` as an empty string is falsey
- dev/provider_packages/prepare_provider_packages.py:1690:93: PLC1901 `version_suffix == ""` can be simplified to `not version_suffix` as an empty string is falsey
+ dev/provider_packages/prepare_provider_packages.py:1690:93: PLC1901 `version_suffix == ""` can be simplified to `version_suffix` as an empty string is falsey
+ dev/provider_packages/prepare_provider_packages.py:332:105: PLC1901 `version_suffix != ""` can be simplified to `not version_suffix` as an empty string is falsey
- dev/provider_packages/prepare_provider_packages.py:332:105: PLC1901 `version_suffix != ""` can be simplified to `version_suffix` as an empty string is falsey
+ dev/provider_packages/prepare_provider_packages.py:480:20: PLC1901 `line == ""` can be simplified to `line` as an empty string is falsey
- dev/provider_packages/prepare_provider_packages.py:480:20: PLC1901 `line == ""` can be simplified to `not line` as an empty string is falsey
+ dev/provider_packages/prepare_provider_packages.py:522:36: PLC1901 `version_required != ""` can be simplified to `not version_required` as an empty string is falsey
- dev/provider_packages/prepare_provider_packages.py:522:36: PLC1901 `version_required != ""` can be simplified to `version_required` as an empty string is falsey
+ dev/provider_packages/prepare_provider_packages.py:649:35: PLC1901 `current_release_version == ""` can be simplified to `current_release_version` as an empty string is falsey
- dev/provider_packages/prepare_provider_packages.py:649:35: PLC1901 `current_release_version == ""` can be simplified to `not current_release_version` as an empty string is falsey
- docs/conf.py:466:21: PLC1901 `value == ""` can be simplified to `not value` as an empty string is falsey
+ docs/conf.py:466:21: PLC1901 `value == ""` can be simplified to `value` as an empty string is falsey
- scripts/ci/pre_commit/pre_commit_newsfragments.py:62:24: PLC1901 `lines[1] != ""` can be simplified to `lines[1]` as an empty string is falsey
+ scripts/ci/pre_commit/pre_commit_newsfragments.py:62:24: PLC1901 `lines[1] != ""` can be simplified to `not lines[1]` as an empty string is falsey
+ scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py:51:32: PLC1901 `line.strip() == ""` can be simplified to `line.strip()` as an empty string is falsey
- scripts/ci/pre_commit/pre_commit_sort_in_the_wild.py:51:32: PLC1901 `line.strip() == ""` can be simplified to `not line.strip()` as an empty string is falsey
- scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py:136:31: PLC1901 `stripped_line == ""` can be simplified to `not stripped_line` as an empty string is falsey
+ scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py:136:31: PLC1901 `stripped_line == ""` can be simplified to `stripped_line` as an empty string is falsey
- scripts/ci/pre_commit/pre_commit_yaml_to_cfg.py:115:36: PLC1901 `single_line_desc == ""` can be simplified to `not single_line_desc` as an empty string is falsey
+ scripts/ci/pre_commit/pre_commit_yaml_to_cfg.py:115:36: PLC1901 `single_line_desc == ""` can be simplified to `single_line_desc` as an empty string is falsey
+ scripts/ci/pre_commit/pre_commit_yaml_to_cfg.py:93:54: PLC1901 `x != ""` can be simplified to `not x` as an empty string is falsey
- scripts/ci/pre_commit/pre_commit_yaml_to_cfg.py:93:54: PLC1901 `x != ""` can be simplified to `x` as an empty string is falsey
- scripts/ci/pre_commit/pre_commit_yaml_to_cfg.py:98:36: PLC1901 `single_line_desc == ""` can be simplified to `not single_line_desc` as an empty string is falsey
+ scripts/ci/pre_commit/pre_commit_yaml_to_cfg.py:98:36: PLC1901 `single_line_desc == ""` can be simplified to `single_line_desc` as an empty string is falsey
+ scripts/in_container/verify_providers.py:677:36: PLC1901 `os.environ.get("CI") != ""` can be simplified to `not os.environ.get("CI")` as an empty string is falsey
- scripts/in_container/verify_providers.py:677:36: PLC1901 `os.environ.get("CI") != ""` can be simplified to `os.environ.get("CI")` as an empty string is falsey
+ scripts/in_container/verify_providers.py:685:36: PLC1901 `os.environ.get("CI") != ""` can be simplified to `not os.environ.get("CI")` as an empty string is falsey
- scripts/in_container/verify_providers.py:685:36: PLC1901 `os.environ.get("CI") != ""` can be simplified to `os.environ.get("CI")` as an empty string is falsey
- setup.py:535:67: PLC1901 `extra != ""` can be simplified to `extra` as an empty string is falsey
+ setup.py:535:67: PLC1901 `extra != ""` can be simplified to `not extra` as an empty string is falsey
+ tests/always/test_secrets_backends.py:102:16: PLC1901 `"" == metastore_backend.get_variable(key="empty_str")` can be simplified to `metastore_backend.get_variable(key="empty_str")` as an empty string is falsey
- tests/always/test_secrets_backends.py:102:16: PLC1901 `"" == metastore_backend.get_variable(key="empty_str")` can be simplified to `not metastore_backend.get_variable(key="empty_str")` as an empty string is falsey
+ tests/always/test_secrets_backends.py:93:16: PLC1901 `"" == env_secrets_backend.get_variable(key="empty_str")` can be simplified to `env_secrets_backend.get_variable(key="empty_str")` as an empty string is falsey
- tests/always/test_secrets_backends.py:93:16: PLC1901 `"" == env_secrets_backend.get_variable(key="empty_str")` can be simplified to `not env_secrets_backend.get_variable(key="empty_str")` as an empty string is falsey
- tests/api/common/experimental/test_pool.py:75:36: PLC1901 `pool.description == ""` can be simplified to `not pool.description` as an empty string is falsey
+ tests/api/common/experimental/test_pool.py:75:36: PLC1901 `pool.description == ""` can be simplified to `pool.description` as an empty string is falsey
- tests/api/common/experimental/test_pool.py:83:36: PLC1901 `pool.description == ""` can be simplified to `not pool.description` as an empty string is falsey
+ tests/api/common/experimental/test_pool.py:83:36: PLC1901 `pool.description == ""` can be simplified to `pool.description` as an empty string is falsey
+ tests/api_connexion/endpoints/test_user_endpoint.py:543:37: PLC1901 `data["last_name"] == ""` can be simplified to `data["last_name"]` as an empty string is falsey
- tests/api_connexion/endpoints/test_user_endpoint.py:543:37: PLC1901 `data["last_name"] == ""` can be simplified to `not data["last_name"]` as an empty string is falsey
+ tests/charts/test_configmap.py:74:20: PLC1901 `"" == jmespath.search('data."airflow_local_settings.py"', docs[0]).strip()` can be simplified to `jmespath.search('data."airflow_local_settings.py"', docs[0]).strip()` as an empty string is falsey
- tests/charts/test_configmap.py:74:20: PLC1901 `"" == jmespath.search('data."airflow_local_settings.py"', docs[0]).strip()` can be simplified to `not jmespath.search('data."airflow_local_settings.py"', docs[0]).strip()` as an empty string is falsey
- tests/models/test_dag.py:1414:16: PLC1901 `"" != dag_run.run_id` can be simplified to `dag_run.run_id` as an empty string is falsey
+ tests/models/test_dag.py:1414:16: PLC1901 `"" != dag_run.run_id` can be simplified to `not dag_run.run_id` as an empty string is falsey
- tests/models/test_pool.py:200:36: PLC1901 `pool.description == ""` can be simplified to `not pool.description` as an empty string is falsey
+ tests/models/test_pool.py:200:36: PLC1901 `pool.description == ""` can be simplified to `pool.description` as an empty string is falsey
- tests/models/test_pool.py:208:36: PLC1901 `pool.description == ""` can be simplified to `not pool.description` as an empty string is falsey
+ tests/models/test_pool.py:208:36: PLC1901 `pool.description == ""` can be simplified to `pool.description` as an empty string is falsey
+ tests/models/test_variable.py:171:16: PLC1901 `"" == Variable.get("test_key")` can be simplified to `Variable.get("test_key")` as an empty string is falsey
- tests/models/test_variable.py:171:16: PLC1901 `"" == Variable.get("test_key")` can be simplified to `not Variable.get("test_key")` as an empty string is falsey
+ tests/models/test_xcom_arg.py:79:30: PLC1901 `actual.key == ""` can be simplified to `actual.key` as an empty string is falsey
- tests/models/test_xcom_arg.py:79:30: PLC1901 `actual.key == ""` can be simplified to `not actual.key` as an empty string is falsey
- tests/providers/amazon/aws/links/test_links.py:115:67: PLC1901 `ti.task.get_extra_links(ti, extra_link_class.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, extra_link_class.name)` as an empty string is falsey
+ tests/providers/amazon/aws/links/test_links.py:115:67: PLC1901 `ti.task.get_extra_links(ti, extra_link_class.name) == ""` can be simplified to `ti.task.get_extra_links(ti, extra_link_class.name)` as an empty string is falsey
+ tests/providers/amazon/aws/links/test_links.py:119:77: PLC1901 `deserialized_task.get_extra_links(ti, extra_link_class.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, extra_link_class.name)` as an empty string is falsey
- tests/providers/amazon/aws/links/test_links.py:119:77: PLC1901 `deserialized_task.get_extra_links(ti, extra_link_class.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, extra_link_class.name)` as an empty string is falsey
- tests/providers/apache/beam/operators/test_beam.py:319:49: PLC1901 `self.operator.launcher_binary == ""` can be simplified to `not self.operator.launcher_binary` as an empty string is falsey
+ tests/providers/apache/beam/operators/test_beam.py:319:49: PLC1901 `self.operator.launcher_binary == ""` can be simplified to `self.operator.launcher_binary` as an empty string is falsey
- tests/providers/apache/beam/operators/test_beam.py:320:47: PLC1901 `self.operator.worker_binary == ""` can be simplified to `not self.operator.worker_binary` as an empty string is falsey
+ tests/providers/apache/beam/operators/test_beam.py:320:47: PLC1901 `self.operator.worker_binary == ""` can be simplified to `self.operator.worker_binary` as an empty string is falsey
- tests/providers/apache/beam/operators/test_beam.py:335:36: PLC1901 `operator.go_file == ""` can be simplified to `not operator.go_file` as an empty string is falsey
+ tests/providers/apache/beam/operators/test_beam.py:335:36: PLC1901 `operator.go_file == ""` can be simplified to `operator.go_file` as an empty string is falsey
- tests/providers/apache/beam/operators/test_beam.py:356:36: PLC1901 `operator.go_file == ""` can be simplified to `not operator.go_file` as an empty string is falsey
+ tests/providers/apache/beam/operators/test_beam.py:356:36: PLC1901 `operator.go_file == ""` can be simplified to `operator.go_file` as an empty string is falsey
- tests/providers/apache/pig/hooks/test_pig.py:54:26: PLC1901 `stdout == ""` can be simplified to `not stdout` as an empty string is falsey
+ tests/providers/apache/pig/hooks/test_pig.py:54:26: PLC1901 `stdout == ""` can be simplified to `stdout` as an empty string is falsey
- tests/providers/apache/pig/hooks/test_pig.py:82:26: PLC1901 `stdout == ""` can be simplified to `not stdout` as an empty string is falsey
+ tests/providers/apache/pig/hooks/test_pig.py:82:26: PLC1901 `stdout == ""` can be simplified to `stdout` as an empty string is falsey
+ tests/providers/cncf/kubernetes/operators/test_pod.py:1076:35: PLC1901 `caplog.text == ""` can be simplified to `caplog.text` as an empty string is falsey
- tests/providers/cncf/kubernetes/operators/test_pod.py:1076:35: PLC1901 `caplog.text == ""` can be simplified to `not caplog.text` as an empty string is falsey
+ tests/providers/cncf/kubernetes/operators/test_pod.py:1086:31: PLC1901 `caplog.text == ""` can be simplified to `caplog.text` as an empty string is falsey
- tests/providers/cncf/kubernetes/operators/test_pod.py:1086:31: PLC1901 `caplog.text == ""` can be simplified to `not caplog.text` as an empty string is falsey
+ tests/providers/cncf/kubernetes/operators/test_pod.py:1096:31: PLC1901 `caplog.text == ""` can be simplified to `caplog.text` as an empty string is falsey
- tests/providers/cncf/kubernetes/operators/test_pod.py:1096:31: PLC1901 `caplog.text == ""` can be simplified to `not caplog.text` as an empty string is falsey
+ tests/providers/cncf/kubernetes/operators/test_pod.py:1111:31: PLC1901 `caplog.text == ""` can be simplified to `caplog.text` as an empty string is falsey
- tests/providers/cncf/kubernetes/operators/test_pod.py:1111:31: PLC1901 `caplog.text == ""` can be simplified to `not caplog.text` as an empty string is falsey
- tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:372:46: PLC1901 `target == ""` can be simplified to `not target` as an empty string is falsey
+ tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:372:46: PLC1901 `target == ""` can be simplified to `target` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_bigquery.py:745:16: PLC1901 `"" == bigquery_task.get_extra_links(ti, BigQueryConsoleLink.name)` can be simplified to `bigquery_task.get_extra_links(ti, BigQueryConsoleLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_bigquery.py:745:16: PLC1901 `"" == bigquery_task.get_extra_links(ti, BigQueryConsoleLink.name)` can be simplified to `not bigquery_task.get_extra_links(ti, BigQueryConsoleLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1161:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1161:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1164:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1164:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1318:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1318:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1321:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1321:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1424:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1424:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1427:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1427:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1497:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1497:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1500:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1500:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1810:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1810:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:1813:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:1813:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:744:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:744:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:747:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:747:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:837:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:837:62: PLC1901 `ti.task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `ti.task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/google/cloud/operators/test_dataproc.py:840:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
- tests/providers/google/cloud/operators/test_dataproc.py:840:72: PLC1901 `deserialized_task.get_extra_links(ti, DataprocLink.name) == ""` can be simplified to `not deserialized_task.get_extra_links(ti, DataprocLink.name)` as an empty string is falsey
+ tests/providers/microsoft/azure/transfers/test_sftp_to_wasb.py:88:29: PLC1901 `delimiter == ""` can be simplified to `delimiter` as an empty string is falsey
- tests/providers/microsoft/azure/transfers/test_sftp_to_wasb.py:88:29: PLC1901 `delimiter == ""` can be simplified to `not delimiter` as an empty string is falsey
- tests/providers/sftp/hooks/test_sftp.py:428:26: PLC1901 `output == ""` can be simplified to `not output` as an empty string is falsey
+ tests/providers/sftp/hooks/test_sftp.py:428:26: PLC1901 `output == ""` can be simplified to `output` as an empty string is falsey
- tests/system/providers/docker/example_docker_copy_data.py:64:64: PLC1901 `task_output == ""` can be simplified to `not task_output` as an empty string is falsey
+ tests/system/providers/docker/example_docker_copy_data.py:64:64: PLC1901 `task_output == ""` can be simplified to `task_output` as an empty string is falsey
+ tests/utils/test_logging_mixin.py:105:31: PLC1901 `log._buffer == ""` can be simplified to `log._buffer` as an empty string is falsey
- tests/utils/test_logging_mixin.py:105:31: PLC1901 `log._buffer == ""` can be simplified to `not log._buffer` as an empty string is falsey
+ tests/utils/test_logging_mixin.py:121:31: PLC1901 `log._buffer == ""` can be simplified to `log._buffer` as an empty string is falsey
- tests/utils/test_logging_mixin.py:121:31: PLC1901 `log._buffer == ""` can be simplified to `not log._buffer` as an empty string is falsey
- tests/www/api/experimental/test_endpoints.py:436:39: PLC1901 `pool["description"] == ""` can be simplified to `not pool["description"]` as an empty string is falsey
+ tests/www/api/experimental/test_endpoints.py:436:39: PLC1901 `pool["description"] == ""` can be simplified to `pool["description"]` as an empty string is falsey
- tests/www/test_utils.py:122:16: PLC1901 `"" == utils.get_params()` can be simplified to `not utils.get_params()` as an empty string is falsey
+ tests/www/test_utils.py:122:16: PLC1901 `"" == utils.get_params()` can be simplified to `utils.get_params()` as an empty string is falsey
- tests/www/test_utils.py:217:16: PLC1901 `"" == rendered` can be simplified to `not rendered` as an empty string is falsey
+ tests/www/test_utils.py:217:16: PLC1901 `"" == rendered` can be simplified to `rendered` as an empty string is falsey
```

</p>
</details>

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@MichaReiser approved on 2023-03-14 08:31_

Nice! 

> It's hard to remove the clap dependency entirely due to orphan rules. Open to ideas -- we could create wrapper structs in ruff-cli? But this at least removes clap for isolated testing, etc.

Can you extend on this. What are the types affected by it? What do we use it for? 

I don't mind wrappers in general because the CLI is it's on API and the data structures may use different layouts than what Ruff uses internally.

---

_Comment by @charliermarsh on 2023-03-14 15:01_

> Can you extend on this. What are the types affected by it? What do we use it for?

There are two enums: `PythonVersion` (like `py37`) and `SerializationFormat` (like `github` or `json`). They're both "simple" enums with no associated data, which ultimately end up on `Settings`. We want them to derive `ValueEnum`.

Beyond that, there's `RuleSelector` which is a little more complicated because it implements its own `possible_values` implementation to enable autocompletion of all rules in the CLI.


---

_Merged by @charliermarsh on 2023-03-14 15:02_

---

_Closed by @charliermarsh on 2023-03-14 15:02_

---

_Branch deleted on 2023-03-14 15:02_

---
