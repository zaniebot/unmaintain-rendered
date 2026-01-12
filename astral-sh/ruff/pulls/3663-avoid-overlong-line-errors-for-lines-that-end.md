```yaml
number: 3663
title: Avoid overlong-line errors for lines that end with URLs
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/e501
created_at: 2023-03-22T02:53:44Z
updated_at: 2023-03-26T18:48:37Z
url: https://github.com/astral-sh/ruff/pull/3663
synced_at: 2026-01-12T15:55:13Z
```

# Avoid overlong-line errors for lines that end with URLs

---

_@charliermarsh_

## Summary

This is (maybe?) a controversial change. Prior to this PR, we ignored overlong lines in a few cases:

1. The line contains a single "word" that can't be split up.

```py
"""Lorem ipsum dolor sit amet.

    https://github.com/PyCQA/pycodestyle/pull/258/files#diff-841c622497a8033d10152bfdfb15b20b92437ecdea21a260944ea86b77b51533

Lorem ipsum dolor sit amet.
"""
```

2. The line is an own-line comment, and the comment consists solely of a URL.

```py
# OK
# https://loooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooong.url.com
```

3. (Not relevant) The line is a `TODO` comment, and the user has `ignore_overlong_task_comments` enabled.

There are a few cases that _aren't_ covered, but arguably should be. The motivating issue used this example:

```py
def func():
    """
    [this-is-ok](https://loooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooog.url.com)
    [this is not](https://loooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooog.url.com)
    """
    ...
```

The first line (`[this-is-ok]`) is _not_ flagged as overlong, because there's no whitespace, so the line can't be split up. The second line _is_ flagged as overlong. You can fix it by changing to:

```py
def func():
    """
    [this-is-ok](https://loooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooog.url.com)
    [this is not](
        https://loooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooog.url.com
    )
    """
    ...
```

But, is that really better?

This also came up in another issue:

```py
class Foo:
    """
    @see https://looooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooong.url.com
    """
```

The change enacted here is that we avoid overlong line errors if the last word contains a URL (even if it does not _start_ with a URL, and even if the line could be split up further).

Closes #3625.

Closes #3051.


---

_Review requested from @konstin by @charliermarsh on 2023-03-22 02:53_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-22 02:53_

---

_Comment by @charliermarsh on 2023-03-22 02:54_

Open to input on this, it's a little bit of a subjective change. Not sure exactly how other linters handle it.

---

_Comment by @github-actions[bot] on 2023-03-22 03:21_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -273, 0 error(s))

<details><summary>airflow (+2, -0)</summary>
<p>

```diff
+ airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:90:10: RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ tests/system/providers/google/cloud/bigtable/example_bigtable.py:43:6: RUF100 [*] Unused `noqa` directive (unused: `E501`)
```

</p>
</details>
<details><summary>zulip (+1, -273)</summary>
<p>

```diff
- scripts/lib/setup_venv.py:65:101: E501 Line too long (103 > 100 characters)
- tools/lib/provision.py:72:101: E501 Line too long (102 > 100 characters)
- tools/linter_lib/custom_check.py:604:101: E501 Line too long (102 > 100 characters)
- tools/linter_lib/custom_check.py:833:101: E501 Line too long (105 > 100 characters)
- zerver/actions/realm_settings.py:434:101: E501 Line too long (105 > 100 characters)
- zerver/lib/markdown/__init__.py:243:101: E501 Line too long (160 > 100 characters)
+ zerver/lib/markdown/__init__.py:854:101: E501 Line too long (113 > 100 characters)
- zerver/lib/markdown/testing_mocks.py:151:101: E501 Line too long (126 > 100 characters)
- zerver/lib/markdown/testing_mocks.py:195:101: E501 Line too long (126 > 100 characters)
- zerver/lib/markdown/testing_mocks.py:38:101: E501 Line too long (126 > 100 characters)
- zerver/lib/markdown/testing_mocks.py:8:101: E501 Line too long (106 > 100 characters)
- zerver/lib/markdown/testing_mocks.py:98:101: E501 Line too long (126 > 100 characters)
- zerver/tests/test_email_notifications.py:1314:101: E501 Line too long (111 > 100 characters)
- zerver/tests/test_email_notifications.py:1559:101: E501 Line too long (116 > 100 characters)
- zerver/tests/test_email_notifications.py:1577:101: E501 Line too long (108 > 100 characters)
- zerver/tests/test_link_embed.py:1029:101: E501 Line too long (126 > 100 characters)
- zerver/tests/test_link_embed.py:991:101: E501 Line too long (110 > 100 characters)
- zerver/tests/test_markdown.py:1167:101: E501 Line too long (114 > 100 characters)
- zerver/tests/test_markdown.py:1188:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_markdown.py:1523:101: E501 Line too long (115 > 100 characters)
- zerver/tests/test_markdown.py:1524:101: E501 Line too long (114 > 100 characters)
- zerver/tests/test_markdown.py:1585:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_markdown.py:1645:101: E501 Line too long (115 > 100 characters)
- zerver/tests/test_markdown.py:2914:101: E501 Line too long (153 > 100 characters)
- zerver/tests/test_markdown.py:2971:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_markdown.py:2977:101: E501 Line too long (132 > 100 characters)
- zerver/tests/test_markdown.py:2983:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_markdown.py:487:101: E501 Line too long (143 > 100 characters)
- zerver/tests/test_markdown.py:537:101: E501 Line too long (181 > 100 characters)
- zerver/tests/test_markdown.py:563:101: E501 Line too long (113 > 100 characters)
- zerver/tests/test_markdown.py:617:101: E501 Line too long (144 > 100 characters)
- zerver/tests/test_markdown.py:669:101: E501 Line too long (105 > 100 characters)
- zerver/tests/test_markdown.py:706:101: E501 Line too long (183 > 100 characters)
- zerver/tests/test_markdown.py:713:101: E501 Line too long (198 > 100 characters)
- zerver/tests/test_markdown.py:722:101: E501 Line too long (207 > 100 characters)
- zerver/tests/test_markdown.py:817:101: E501 Line too long (112 > 100 characters)
- zerver/tests/test_markdown.py:819:101: E501 Line too long (220 > 100 characters)
- zerver/tests/test_markdown.py:831:101: E501 Line too long (125 > 100 characters)
- zerver/tests/test_markdown.py:849:101: E501 Line too long (219 > 100 characters)
- zerver/tests/test_markdown.py:879:101: E501 Line too long (149 > 100 characters)
- zerver/tests/test_markdown.py:884:101: E501 Line too long (104 > 100 characters)
- zerver/tests/test_markdown.py:902:101: E501 Line too long (109 > 100 characters)
- zerver/tests/test_markdown.py:919:101: E501 Line too long (121 > 100 characters)
- zerver/tests/test_markdown.py:985:101: E501 Line too long (156 > 100 characters)
- zerver/tests/test_push_notifications.py:1641:101: E501 Line too long (102 > 100 characters)
- zerver/tests/test_signup.py:195:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_signup.py:205:101: E501 Line too long (119 > 100 characters)
- zerver/tests/test_signup.py:230:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_signup.py:238:101: E501 Line too long (103 > 100 characters)
- zerver/tests/test_thumbnail.py:36:101: E501 Line too long (203 > 100 characters)
- zerver/tests/test_thumbnail.py:42:101: E501 Line too long (201 > 100 characters)
- zerver/tests/test_thumbnail.py:48:101: E501 Line too long (191 > 100 characters)
- zerver/tests/test_upload.py:1258:101: E501 Line too long (120 > 100 characters)
- zerver/tests/test_upload.py:451:101: E501 Line too long (128 > 100 characters)
- zerver/tests/test_upload_local.py:260:101: E501 Line too long (123 > 100 characters)
- zerver/tests/test_upload_s3.py:516:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/alertmanager/tests.py:13:101: E501 Line too long (174 > 100 characters)
- zerver/webhooks/alertmanager/tests.py:14:101: E501 Line too long (175 > 100 characters)
- zerver/webhooks/alertmanager/tests.py:27:101: E501 Line too long (198 > 100 characters)
- zerver/webhooks/appveyor/tests.py:15:101: E501 Line too long (119 > 100 characters)
- zerver/webhooks/appveyor/tests.py:29:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/azuredevops/tests.py:47:101: E501 Line too long (166 > 100 characters)
- zerver/webhooks/azuredevops/tests.py:55:101: E501 Line too long (164 > 100 characters)
- zerver/webhooks/basecamp/tests.py:100:101: E501 Line too long (146 > 100 characters)
- zerver/webhooks/basecamp/tests.py:104:101: E501 Line too long (143 > 100 characters)
- zerver/webhooks/basecamp/tests.py:108:101: E501 Line too long (158 > 100 characters)
- zerver/webhooks/basecamp/tests.py:112:101: E501 Line too long (156 > 100 characters)
- zerver/webhooks/basecamp/tests.py:116:101: E501 Line too long (147 > 100 characters)
- zerver/webhooks/basecamp/tests.py:120:101: E501 Line too long (135 > 100 characters)
- zerver/webhooks/basecamp/tests.py:124:101: E501 Line too long (137 > 100 characters)
- zerver/webhooks/basecamp/tests.py:128:101: E501 Line too long (133 > 100 characters)
- zerver/webhooks/basecamp/tests.py:12:101: E501 Line too long (137 > 100 characters)
- zerver/webhooks/basecamp/tests.py:132:101: E501 Line too long (143 > 100 characters)
- zerver/webhooks/basecamp/tests.py:16:101: E501 Line too long (136 > 100 characters)
- zerver/webhooks/basecamp/tests.py:20:101: E501 Line too long (151 > 100 characters)
- zerver/webhooks/basecamp/tests.py:24:101: E501 Line too long (149 > 100 characters)
- zerver/webhooks/basecamp/tests.py:28:101: E501 Line too long (138 > 100 characters)
- zerver/webhooks/basecamp/tests.py:32:101: E501 Line too long (135 > 100 characters)
- zerver/webhooks/basecamp/tests.py:36:101: E501 Line too long (135 > 100 characters)
- zerver/webhooks/basecamp/tests.py:40:101: E501 Line too long (138 > 100 characters)
- zerver/webhooks/basecamp/tests.py:64:101: E501 Line too long (137 > 100 characters)
- zerver/webhooks/basecamp/tests.py:68:101: E501 Line too long (136 > 100 characters)
- zerver/webhooks/basecamp/tests.py:72:101: E501 Line too long (136 > 100 characters)
- zerver/webhooks/basecamp/tests.py:76:101: E501 Line too long (139 > 100 characters)
- zerver/webhooks/basecamp/tests.py:80:101: E501 Line too long (144 > 100 characters)
- zerver/webhooks/basecamp/tests.py:84:101: E501 Line too long (154 > 100 characters)
- zerver/webhooks/basecamp/tests.py:88:101: E501 Line too long (139 > 100 characters)
- zerver/webhooks/basecamp/tests.py:92:101: E501 Line too long (154 > 100 characters)
- zerver/webhooks/basecamp/tests.py:96:101: E501 Line too long (143 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:101:101: E501 Line too long (127 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:156:101: E501 Line too long (123 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:165:101: E501 Line too long (123 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:16:101: E501 Line too long (103 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:26:101: E501 Line too long (103 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:36:101: E501 Line too long (120 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:37:101: E501 Line too long (115 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:52:101: E501 Line too long (120 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:53:101: E501 Line too long (115 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:67:101: E501 Line too long (120 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:68:101: E501 Line too long (115 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:79:101: E501 Line too long (120 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:80:101: E501 Line too long (115 > 100 characters)
- zerver/webhooks/beanstalk/tests.py:86:101: E501 Line too long (127 > 100 characters)
- zerver/webhooks/bitbucket/tests.py:17:101: E501 Line too long (140 > 100 characters)
- zerver/webhooks/bitbucket/tests.py:24:101: E501 Line too long (140 > 100 characters)
- zerver/webhooks/bitbucket/tests.py:35:101: E501 Line too long (140 > 100 characters)
- zerver/webhooks/bitbucket/tests.py:42:101: E501 Line too long (142 > 100 characters)
- zerver/webhooks/bitbucket/tests.py:51:101: E501 Line too long (142 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:105:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:157:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:168:101: E501 Line too long (128 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:177:101: E501 Line too long (119 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:186:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:19:101: E501 Line too long (151 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:24:101: E501 Line too long (153 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:278:101: E501 Line too long (123 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:29:101: E501 Line too long (153 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:332:101: E501 Line too long (107 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:362:101: E501 Line too long (107 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:37:101: E501 Line too long (153 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:45:101: E501 Line too long (153 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:53:101: E501 Line too long (151 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:58:101: E501 Line too long (142 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:64:101: E501 Line too long (142 > 100 characters)
- zerver/webhooks/bitbucket2/tests.py:83:101: E501 Line too long (140 > 100 characters)
- zerver/webhooks/bitbucket3/tests.py:172:101: E501 Line too long (173 > 100 characters)
- zerver/webhooks/bitbucket3/tests.py:183:101: E501 Line too long (166 > 100 characters)
- zerver/webhooks/bitbucket3/tests.py:188:101: E501 Line too long (164 > 100 characters)
- zerver/webhooks/bitbucket3/tests.py:194:101: E501 Line too long (166 > 100 characters)
- zerver/webhooks/bitbucket3/tests.py:199:101: E501 Line too long (168 > 100 characters)
- zerver/webhooks/circleci/tests.py:23:101: E501 Line too long (157 > 100 characters)
- zerver/webhooks/circleci/tests.py:50:101: E501 Line too long (142 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:107:101: E501 Line too long (129 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:113:101: E501 Line too long (139 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:117:101: E501 Line too long (143 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:159:101: E501 Line too long (141 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:172:101: E501 Line too long (131 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:176:101: E501 Line too long (144 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:180:101: E501 Line too long (133 > 100 characters)
- zerver/webhooks/clubhouse/tests.py:395:101: E501 Line too long (118 > 100 characters)
- zerver/webhooks/freshdesk/tests.py:18:101: E501 Line too long (119 > 100 characters)
- zerver/webhooks/freshdesk/tests.py:44:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/freshdesk/tests.py:64:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/gci/tests.py:11:101: E501 Line too long (180 > 100 characters)
- zerver/webhooks/gci/tests.py:16:101: E501 Line too long (183 > 100 characters)
- zerver/webhooks/gci/tests.py:21:101: E501 Line too long (180 > 100 characters)
- zerver/webhooks/gci/tests.py:26:101: E501 Line too long (178 > 100 characters)
- zerver/webhooks/gci/tests.py:31:101: E501 Line too long (175 > 100 characters)
- zerver/webhooks/gitea/tests.py:15:101: E501 Line too long (108 > 100 characters)
- zerver/webhooks/gitea/tests.py:16:101: E501 Line too long (108 > 100 characters)
- zerver/webhooks/gitea/tests.py:17:101: E501 Line too long (108 > 100 characters)
- zerver/webhooks/gitea/tests.py:18:101: E501 Line too long (108 > 100 characters)
- zerver/webhooks/gitea/tests.py:19:101: E501 Line too long (111 > 100 characters)
- zerver/webhooks/github/tests.py:117:101: E501 Line too long (122 > 100 characters)
- zerver/webhooks/github/tests.py:156:101: E501 Line too long (154 > 100 characters)
- zerver/webhooks/github/tests.py:177:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/github/tests.py:181:101: E501 Line too long (129 > 100 characters)
- zerver/webhooks/github/tests.py:187:101: E501 Line too long (168 > 100 characters)
- zerver/webhooks/github/tests.py:205:101: E501 Line too long (146 > 100 characters)
- zerver/webhooks/github/tests.py:209:101: E501 Line too long (154 > 100 characters)
- zerver/webhooks/github/tests.py:243:101: E501 Line too long (148 > 100 characters)
- zerver/webhooks/github/tests.py:249:101: E501 Line too long (194 > 100 characters)
- zerver/webhooks/github/tests.py:273:101: E501 Line too long (123 > 100 characters)
- zerver/webhooks/github/tests.py:277:101: E501 Line too long (124 > 100 characters)
- zerver/webhooks/github/tests.py:28:101: E501 Line too long (131 > 100 characters)
- zerver/webhooks/github/tests.py:295:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/github/tests.py:351:101: E501 Line too long (185 > 100 characters)
- zerver/webhooks/github/tests.py:60:101: E501 Line too long (157 > 100 characters)
- zerver/webhooks/github/tests.py:66:101: E501 Line too long (157 > 100 characters)
- zerver/webhooks/github/tests.py:73:101: E501 Line too long (157 > 100 characters)
- zerver/webhooks/github/tests.py:80:101: E501 Line too long (157 > 100 characters)
- zerver/webhooks/github/tests.py:86:101: E501 Line too long (156 > 100 characters)
- zerver/webhooks/github/tests.py:92:101: E501 Line too long (156 > 100 characters)
- zerver/webhooks/gitlab/tests.py:172:101: E501 Line too long (119 > 100 characters)
- zerver/webhooks/gitlab/tests.py:179:101: E501 Line too long (122 > 100 characters)
- zerver/webhooks/gitlab/tests.py:185:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/gitlab/tests.py:200:101: E501 Line too long (121 > 100 characters)
- zerver/webhooks/gitlab/tests.py:206:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/gitlab/tests.py:214:101: E501 Line too long (123 > 100 characters)
- zerver/webhooks/gitlab/tests.py:220:101: E501 Line too long (118 > 100 characters)
- zerver/webhooks/gitlab/tests.py:331:101: E501 Line too long (126 > 100 characters)
- zerver/webhooks/gitlab/tests.py:339:101: E501 Line too long (126 > 100 characters)
- zerver/webhooks/gitlab/tests.py:356:101: E501 Line too long (125 > 100 characters)
- zerver/webhooks/gitlab/tests.py:364:101: E501 Line too long (125 > 100 characters)
- zerver/webhooks/gitlab/tests.py:379:101: E501 Line too long (138 > 100 characters)
- zerver/webhooks/gitlab/tests.py:394:101: E501 Line too long (136 > 100 characters)
- zerver/webhooks/gitlab/tests.py:400:101: E501 Line too long (136 > 100 characters)
- zerver/webhooks/gitlab/tests.py:43:101: E501 Line too long (145 > 100 characters)
- zerver/webhooks/gitlab/tests.py:51:101: E501 Line too long (146 > 100 characters)
- zerver/webhooks/gitlab/tests.py:586:101: E501 Line too long (126 > 100 characters)
- zerver/webhooks/gitlab/tests.py:605:101: E501 Line too long (127 > 100 characters)
- zerver/webhooks/gitlab/tests.py:60:101: E501 Line too long (146 > 100 characters)
- zerver/webhooks/gogs/tests.py:16:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/gogs/tests.py:20:101: E501 Line too long (140 > 100 characters)
- zerver/webhooks/gogs/tests.py:27:101: E501 Line too long (140 > 100 characters)
- zerver/webhooks/gogs/tests.py:37:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/gogs/tests.py:42:101: E501 Line too long (141 > 100 characters)
- zerver/webhooks/gogs/tests.py:49:101: E501 Line too long (141 > 100 characters)
- zerver/webhooks/grafana/tests.py:38:101: E501 Line too long (104 > 100 characters)
- zerver/webhooks/grafana/tests.py:57:101: E501 Line too long (107 > 100 characters)
- zerver/webhooks/groove/tests.py:116:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/groove/tests.py:13:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/groove/tests.py:80:101: E501 Line too long (105 > 100 characters)
- zerver/webhooks/groove/tests.py:98:101: E501 Line too long (105 > 100 characters)
- zerver/webhooks/jira/tests.py:134:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/jira/tests.py:190:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/jira/tests.py:212:101: E501 Line too long (134 > 100 characters)
- zerver/webhooks/mention/tests.py:12:101: E501 Line too long (168 > 100 characters)
- zerver/webhooks/opbeat/tests.py:14:101: E501 Line too long (117 > 100 characters)
- zerver/webhooks/opbeat/tests.py:59:101: E501 Line too long (125 > 100 characters)
- zerver/webhooks/opbeat/tests.py:74:101: E501 Line too long (118 > 100 characters)
- zerver/webhooks/opbeat/tests.py:89:101: E501 Line too long (115 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:113:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:128:101: E501 Line too long (107 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:12:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:144:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:160:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:175:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:190:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:207:101: E501 Line too long (107 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:223:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:28:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:45:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:62:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:79:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/opsgenie/tests.py:96:101: E501 Line too long (112 > 100 characters)
- zerver/webhooks/pivotal/tests.py:75:101: E501 Line too long (102 > 100 characters)
- zerver/webhooks/rhodecode/tests.py:45:101: E501 Line too long (164 > 100 characters)
- zerver/webhooks/rhodecode/tests.py:53:101: E501 Line too long (163 > 100 characters)
- zerver/webhooks/semaphore/tests.py:22:101: E501 Line too long (152 > 100 characters)
- zerver/webhooks/semaphore/tests.py:36:101: E501 Line too long (152 > 100 characters)
- zerver/webhooks/sentry/tests.py:111:101: E501 Line too long (183 > 100 characters)
- zerver/webhooks/sentry/tests.py:124:101: E501 Line too long (176 > 100 characters)
- zerver/webhooks/sentry/tests.py:12:101: E501 Line too long (201 > 100 characters)
- zerver/webhooks/sentry/tests.py:135:101: E501 Line too long (181 > 100 characters)
- zerver/webhooks/sentry/tests.py:145:101: E501 Line too long (169 > 100 characters)
- zerver/webhooks/sentry/tests.py:155:101: E501 Line too long (177 > 100 characters)
- zerver/webhooks/sentry/tests.py:38:101: E501 Line too long (174 > 100 characters)
- zerver/webhooks/sentry/tests.py:64:101: E501 Line too long (172 > 100 characters)
- zerver/webhooks/sentry/tests.py:88:101: E501 Line too long (157 > 100 characters)
- zerver/webhooks/slack_incoming/tests.py:110:101: E501 Line too long (132 > 100 characters)
- zerver/webhooks/slack_incoming/tests.py:112:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/slack_incoming/tests.py:76:101: E501 Line too long (141 > 100 characters)
- zerver/webhooks/slack_incoming/tests.py:95:101: E501 Line too long (141 > 100 characters)
- zerver/webhooks/splunk/tests.py:110:101: E501 Line too long (186 > 100 characters)
- zerver/webhooks/splunk/tests.py:129:101: E501 Line too long (186 > 100 characters)
- zerver/webhooks/splunk/tests.py:148:101: E501 Line too long (186 > 100 characters)
- zerver/webhooks/splunk/tests.py:16:101: E501 Line too long (186 > 100 characters)
- zerver/webhooks/splunk/tests.py:35:101: E501 Line too long (216 > 100 characters)
- zerver/webhooks/splunk/tests.py:53:101: E501 Line too long (445 > 100 characters)
- zerver/webhooks/splunk/tests.py:91:101: E501 Line too long (201 > 100 characters)
- zerver/webhooks/stripe/tests.py:172:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/stripe/view.py:199:101: E501 Line too long (109 > 100 characters)
- zerver/webhooks/taiga/tests.py:111:101: E501 Line too long (151 > 100 characters)
- zerver/webhooks/taiga/tests.py:185:101: E501 Line too long (150 > 100 characters)
- zerver/webhooks/taiga/tests.py:231:101: E501 Line too long (150 > 100 characters)
- zerver/webhooks/taiga/tests.py:235:101: E501 Line too long (152 > 100 characters)
- zerver/webhooks/trello/tests.py:111:101: E501 Line too long (123 > 100 characters)
- zerver/webhooks/trello/tests.py:23:101: E501 Line too long (119 > 100 characters)
- zerver/webhooks/trello/tests.py:27:101: E501 Line too long (125 > 100 characters)
- zerver/webhooks/trello/tests.py:31:101: E501 Line too long (128 > 100 characters)
- zerver/webhooks/trello/tests.py:73:101: E501 Line too long (126 > 100 characters)
- zerver/webhooks/trello/tests.py:77:101: E501 Line too long (116 > 100 characters)
- zerver/webhooks/trello/tests.py:81:101: E501 Line too long (134 > 100 characters)
- zerver/webhooks/trello/tests.py:85:101: E501 Line too long (136 > 100 characters)
- zerver/webhooks/wekan/tests.py:118:101: E501 Line too long (155 > 100 characters)
- zerver/webhooks/wekan/tests.py:154:101: E501 Line too long (121 > 100 characters)
- zerver/webhooks/wekan/tests.py:163:101: E501 Line too long (125 > 100 characters)
- zerver/webhooks/wekan/tests.py:172:101: E501 Line too long (149 > 100 characters)
- zerver/webhooks/wekan/tests.py:199:101: E501 Line too long (143 > 100 characters)
- zproject/prod_settings_template.py:248:101: E501 Line too long (143 > 100 characters)
- zproject/prod_settings_template.py:258:101: E501 Line too long (140 > 100 characters)
- zproject/prod_settings_template.py:502:101: E501 Line too long (103 > 100 characters)
- zproject/prod_settings_template.py:661:101: E501 Line too long (108 > 100 characters)
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.6±0.41ms     2.3 MB/sec    1.00     17.4±0.50ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.12ms     3.7 MB/sec    1.00      4.5±0.21ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   625.4±30.88µs     4.7 MB/sec    1.00   613.8±27.41µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.7±0.29ms     3.3 MB/sec    1.00      7.6±0.20ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.03      9.3±0.25ms     4.4 MB/sec    1.00      9.1±0.25ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.07ms     8.0 MB/sec    1.00      2.0±0.06ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.7±11.61µs    12.0 MB/sec    1.00   245.9±10.26µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.13ms     5.9 MB/sec    1.00      4.4±0.23ms     5.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.03ms     2.7 MB/sec    1.07     16.0±0.04ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.04      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.0±4.79µs     6.5 MB/sec    1.02    465.9±4.23µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.03ms     3.8 MB/sec    1.06      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.02ms     5.0 MB/sec    1.13      9.2±0.03ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1746.7±7.77µs     9.5 MB/sec    1.10   1929.1±8.66µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.7±1.06µs    16.1 MB/sec    1.08    197.1±3.38µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.11      4.2±0.04ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/pycodestyle/E501.py`:64 on 2023-03-22 08:14_

Can you add the `@see` example, or is this not yet supported? 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:48 on 2023-03-22 08:16_

What happens if we have

```
This is a long sentence that ends with a shortened URL and, therefore, could easily be broken across multiple lines ([source](https://ruff.rs))
```

---

_@MichaReiser reviewed on 2023-03-22 08:16_

---

_@konstin approved on 2023-03-22 09:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/helpers.rs`:48 on 2023-03-22 15:59_

Yeah this would be "allowed", which feels off.

I guess we could allow: (1) one word before URL (like `@see https://...`), (2) entire line is a Markdown link?

---

_@charliermarsh reviewed on 2023-03-22 15:59_

---

_@MichaReiser reviewed on 2023-03-22 16:55_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:48 on 2023-03-22 16:55_

I like that!

---

_@charliermarsh reviewed on 2023-03-22 18:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/helpers.rs`:48 on 2023-03-22 18:19_

Hmm... becoming less sure of that approach. Here are some examples from Zulip that seem valid to me and are no longer flagged, but _would_ be flagged based on what I just suggested:

```py
## What portion of events are sampled (https://docs.sentry.io/platforms/javascript/configuration/sampling/):
# SENTRY_FRONTEND_SAMPLE_RATE = 1.0
# SENTRY_FRONTEND_TRACE_RATE = 0.1
```

```py
        expected_message = """
## [[FIRING:2] InstanceDown for api-server (env="prod", severity="critical")](https://alertmanager.local//#/alerts?receiver=default)

:chart_with_upwards_trend: **[Graph](http://generator.local/1)**   :notebook: **[Runbook](https://runbook.local/1)**
"""
```

---

_@MichaReiser reviewed on 2023-03-23 08:50_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:48 on 2023-03-23 08:50_

ESLint's [max-len rule](https://eslint.org/docs/latest/rules/max-len) has many different configuration options, one to ignore comments, and another to ignore URLs. I'm not sure what the right solution is but is probably why I prefer formatters where the line-width is [a guidance](https://prettier.io/docs/en/options.html#print-width) rather than an absolute limit over line-length lint rules. 



---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/helpers.rs`:48 on 2023-03-25 19:26_

I think we ship this for now, but... we should revise the check such that it errors if the URL _starts_ after the line-length limit.

---

_@charliermarsh reviewed on 2023-03-25 19:26_

---

_Merged by @charliermarsh on 2023-03-26 18:17_

---

_Closed by @charliermarsh on 2023-03-26 18:17_

---

_Branch deleted on 2023-03-26 18:17_

---
