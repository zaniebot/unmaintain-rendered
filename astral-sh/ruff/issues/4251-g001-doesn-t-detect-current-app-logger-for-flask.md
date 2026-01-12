```yaml
number: 4251
title: "G001 doesn't detect current_app.logger for Flask"
type: issue
state: closed
author: samuelhwilliams
labels: []
assignees: []
created_at: 2023-05-06T06:12:09Z
updated_at: 2023-05-06T15:31:11Z
url: https://github.com/astral-sh/ruff/issues/4251
synced_at: 2026-01-12T15:54:44Z
```

# G001 doesn't detect current_app.logger for Flask

---

_@samuelhwilliams_

Ruff version: 0.0.264

I've started using Ruff on a few Flask projects and after turning on the G linter (flake8-logging-format) have noticed that Ruff doesn't report on formatting violations inside `current_app.logger` calls.

Having done a quick dig around Ruff I suspect this is the thing that would need updating: https://github.com/charliermarsh/ruff/blob/11e1380df412cd18eb8f6001e6164195bf90436b/crates/ruff_python_semantic/src/analyze/logging.rs#L19

Example `app.py` ([git repo here](https://github.com/samuelhwilliams/test_ruff_flask_logger)):
<img width="544" alt="image" src="https://user-images.githubusercontent.com/2920760/236605760-9edc70cc-45a4-4479-a67c-e82ef2ed18fb.png">


Validate:
```bash
$ sam:~/work/test_ruff_flask_logger [main]$ ruff check .
app.py:11:17: G001 Logging statement uses `string.format()`
Found 1 error.
```

---

_Comment by @dhruvmanila on 2023-05-06 07:03_

This can be supported easily. I think even `app.logger.info` works, right? The latter would be hard to detect as there's no type inference yet.

Let me create a quick PR for the main issue :)


---

_Comment by @samuelhwilliams on 2023-05-06 08:52_

Amazing - thank you 🙇‍♂️

---

_Closed by @charliermarsh on 2023-05-06 15:31_

---
