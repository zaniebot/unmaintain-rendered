---
number: 10139
title: Can RSE102 conflict with the celery.app.task.Task.retry() method?
type: issue
state: closed
author: cclauss
labels:
  - bug
  - rule
assignees: []
created_at: 2024-02-26T16:23:18Z
updated_at: 2024-03-11T17:53:28Z
url: https://github.com/astral-sh/ruff/issues/10139
synced_at: 2026-01-10T01:22:49Z
---

# Can RSE102 conflict with the celery.app.task.Task.retry() method?

---

_Issue opened by @cclauss on 2024-02-26 16:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
[`celery.app.task.Task.retry()`](https://docs.celeryq.dev/en/stable/reference/celery.app.task.html#celery.app.task.Task.retry) is a strange construction that is a method call instead of the normally expected instance of an exception. This seems to confuse [`ruff rule RSE102`](https://docs.astral.sh/ruff/rules/unnecessary-paren-on-raise-exception) unnecessary-paren-on-raise-exception if `Task.retry()` is called with no parameters.
```python
@app.task(bind=True)
def tweet(self, auth, message):
    twitter = Twitter(oauth=auth)
    try:
        twitter.post_status_update(message)
    except twitter.FailWhale:
        raise self.retry()  # noqa: RSE102
```
If we leave off the `noqa` then `ruff --fix` may remove the parens which would break Celery's `Task.retry()` logic.
```diff
-        raise self.retry()
+        raise self.retry
```
@Nusnus


---

_Comment by @AlexWaygood on 2024-02-27 11:34_

Maybe method calls (anything that starts with `self.`?) should just be exempted from this check? I feel like it's pretty rare that `method` is going to be an exception class if you're doing `raise self.method()` 

---

_Label `rule` added by @charliermarsh on 2024-02-28 00:52_

---

_Comment by @zanieb on 2024-03-11 17:42_

Maybe we should make this fix unsafe unless it's like visibly one of the standard exception classes?

---

_Comment by @cclauss on 2024-03-11 17:52_

I sense that `RSE102` should ignore all instances of `raise self.method()`.

---

_Comment by @charliermarsh on 2024-03-11 17:53_

Oh sorry, I think this got fixed because we now avoid all calls that don't "look like" exceptions.

---

_Closed by @charliermarsh on 2024-03-11 17:53_

---

_Label `bug` added by @charliermarsh on 2024-03-11 17:53_

---
