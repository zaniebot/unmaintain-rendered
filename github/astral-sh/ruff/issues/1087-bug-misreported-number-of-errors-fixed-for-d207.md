---
number: 1087
title: "[bug] Misreported number of errors fixed for `D207`/`D208`"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2022-12-05T23:28:02Z
updated_at: 2022-12-09T22:31:05Z
url: https://github.com/astral-sh/ruff/issues/1087
synced_at: 2026-01-07T13:12:14-06:00
---

# [bug] Misreported number of errors fixed for `D207`/`D208`

---

_Issue opened by @smackesey on 2022-12-05 23:28_

From my shell (on `0.0.160`):

```
$ruff --select=D207,D208 `git ls-files '*.py'`
Found 7 error(s).
python_modules/dagster/dagster/_loggers/__init__.py:112:1: D208 Docstring is over-indented
python_modules/dagster/dagster_tests/core_tests/test_pipeline_errors.py:103:1: D208 Docstring is over-indented
python_modules/dagster/dagster_tests/core_tests/test_pipeline_errors.py:104:1: D208 Docstring is over-indented
python_modules/dagster/dagster_tests/core_tests/test_pipeline_errors.py:106:1: D208 Docstring is over-indented
python_modules/dagster/dagster_tests/core_tests/test_pipeline_errors.py:107:1: D208 Docstring is over-indented
python_modules/libraries/dagster-aws/dagster_aws/utils/mrjob/log4j.py:54:1: D207 Docstring is under-indented
python_modules/libraries/dagster-aws/dagster_aws/utils/mrjob/log4j.py:90:1: D207 Docstring is under-indented
7 potentially fixable with the --fix option.


$ ruff --fix --select=D207,D208 `git ls-files '*.py'`
Found 2 error(s) (205 fixed).
python_modules/libraries/dagster-aws/dagster_aws/utils/mrjob/log4j.py:54:1: D207 Docstring is under-indented
python_modules/libraries/dagster-aws/dagster_aws/utils/mrjob/log4j.py:90:1: D207 Docstring is under-indented
2 potentially fixable with the --fix option.
```

Notice it says 205 errors were fixed after initially detecting 7! Also the errors being fixed are weird and maybe false positives? I'm not entirely sure what's happening. I'm guessing it has something to do with encoding. Here are the files that are changed when I run the above `--fix` on `dagster` `master`:

```
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   python_modules/dagster/dagster/_grpc/__generated__/__init__.py
	modified:   python_modules/dagster/dagster/_loggers/__init__.py
	modified:   python_modules/dagster/dagster_tests/core_tests/test_pipeline_errors.py
	modified:   python_modules/libraries/dagster-aws/dagster_aws/utils/mrjob/log4j.py

no changes added to commit (use "git add" and/or "git commit -a")
```

---

_Comment by @charliermarsh on 2022-12-05 23:33_

Lol. I'm guessing that there's an unstable fix here, whereby one fix is causing another error, and it's iteratively fixing and re-inserting an error and fixing until it hits the iteration limit. Will take a look.

---

_Comment by @charliermarsh on 2022-12-05 23:43_

@smackesey - If it's helpful for now, I think the issue with those specific docstrings is that they contain unescaped newline characters (like `\n`).


---

_Comment by @charliermarsh on 2022-12-05 23:45_

As in, I think that should be a raw string, like:

```py
class Log4jRecord(
    namedtuple(
        "_Log4jRecord", "caller_location level logger message num_lines start_line thread timestamp"
    )
):
    r"""Represents a Log4J log record.

    caller_location -- e.g. 'YarnClientImpl.java:submitApplication(251)'
    level -- e.g. 'INFO'
    logger -- e.g. 'amazon.emr.metrics.MetricsSaver'
    message -- the actual message. If this is a multi-line message (e.g.
        for counters), the lines will be joined by '\n'
    num_lines -- how many lines made up the message
    start_line -- which line the message started on (0-indexed)
    thread -- e.g. 'main'. Defaults to ''
    timestamp -- unparsed timestamp, e.g. '15/12/07 20:49:28'
    """
    pass
```

---

_Comment by @charliermarsh on 2022-12-05 23:53_

(But, we still shouldn't be inserting an errant fix, so there's still a bug here.)

---

_Label `bug` added by @charliermarsh on 2022-12-06 00:23_

---

_Comment by @charliermarsh on 2022-12-09 21:45_

Ah, ok, the issue is that we're using the inner docstring contents, which are created during the parsing pass and treat `\n` as an actual newline.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-09 21:45_

---

_Referenced in [astral-sh/ruff#1167](../../astral-sh/ruff/pulls/1167.md) on 2022-12-09 22:31_

---

_Closed by @charliermarsh on 2022-12-09 22:31_

---
