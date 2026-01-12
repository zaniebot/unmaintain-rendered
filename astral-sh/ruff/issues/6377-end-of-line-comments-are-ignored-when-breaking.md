```yaml
number: 6377
title: End-of-line comments are ignored when breaking long lines in the formatter
type: issue
state: closed
author: charliermarsh
labels:
  - formatter
assignees: []
created_at: 2023-08-06T15:47:10Z
updated_at: 2023-08-16T07:11:11Z
url: https://github.com/astral-sh/ruff/issues/6377
synced_at: 2026-01-12T15:54:46Z
```

# End-of-line comments are ignored when breaking long lines in the formatter

---

_@charliermarsh_

For example, we treat this as stable formatting:

```python
def get_recent_deployments(threshold_days: int) -> Set[str]:
    # Returns a list of deployments not older than threshold days
    # including `/root/zulip` directory if it exists.
    recent = set()
    threshold_date = (
        datetime.datetime.now() - datetime.timedelta(days=threshold_days)  # noqa: DTZ005
    )
```

Whereas we _should_ be formatting it as:

```python
def get_recent_deployments(threshold_days: int) -> Set[str]:
    # Returns a list of deployments not older than threshold days
    # including `/root/zulip` directory if it exists.
    recent = set()
    threshold_date = datetime.datetime.now() - datetime.timedelta(
        days=threshold_days
    )  # noqa: DTZ005
```

Since the `# noqa: DTZ005` causes the accompanying line to exceed the line length.

---

_Label `formatter` added by @charliermarsh on 2023-08-06 15:47_

---

_Comment by @charliermarsh on 2023-08-06 15:47_

I haven't looked into this at all, but @MichaReiser has mentioned that this is hard (for line suffixes to be included in the break length).

---

_Comment by @MichaReiser on 2023-08-06 16:23_

Thanks for the detailed report. I don't know if we want to change this. I think you can argue both ways and it adds a fair amount of complexity. See https://github.com/astral-sh/ruff/issues/5630 for the details 

Closing to track this in a single issue 

---

_Closed by @MichaReiser on 2023-08-06 16:23_

---

_Comment by @charliermarsh on 2023-08-06 16:28_

Thanks, missed that this was a dupe.

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:11_

---
