---
number: 10470
title: Tests fails on 32 bits archs
type: issue
state: closed
author: raspbeguy
labels:
  - bug
  - testing
assignees: []
created_at: 2024-03-19T08:02:22Z
updated_at: 2024-03-20T13:44:10Z
url: https://github.com/astral-sh/ruff/issues/10470
synced_at: 2026-01-07T13:12:15-06:00
---

# Tests fails on 32 bits archs

---

_Issue opened by @raspbeguy on 2024-03-19 08:02_

Hello, this issue is following #10359.

Now that `display_default_settings` is fixed, some other tests are failing specifically on 32 bits archs:

```
failures:
    rules::flake8_logging::tests::rules::rule_undocumentedwarn_path_new_log009_py_expects
    rules::pyupgrade::tests::datetime_utc_alias_py311
    rules::refurb::tests::rules::rule_redundantlogbase_path_new_furb163_py_expects
    rules::ruff::tests::implicit_optional_py39::path_new_ruf013_0_py_expects
    rules::tryceratops::tests::rules::rule_errorinsteadofexception_path_new_try400_py_expects
```

I attach the complete error log.
[ruff_build.log](https://github.com/astral-sh/ruff/files/14647324/ruff_build.log)

Thanks.

---

_Label `bug` added by @dhruvmanila on 2024-03-19 08:45_

---

_Label `testing` added by @dhruvmanila on 2024-03-19 08:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-19 15:53_

---

_Comment by @charliermarsh on 2024-03-19 17:22_

I'll take a look... my guess is there's some non-determinism?

---

_Referenced in [astral-sh/ruff#10478](../../astral-sh/ruff/pulls/10478.md) on 2024-03-19 17:54_

---

_Closed by @charliermarsh on 2024-03-19 18:01_

---

_Comment by @charliermarsh on 2024-03-19 18:02_

Should be fixed now.

---

_Comment by @raspbeguy on 2024-03-20 13:44_

I confirm that it's working now. Thanks.

---
