```yaml
number: 889
title: "\"F841 local variable unused\" triggering erroniously"
type: issue
state: closed
author: jhallard
labels:
  - bug
assignees: []
created_at: 2022-11-23T07:34:13Z
updated_at: 2022-11-23T23:30:13Z
url: https://github.com/astral-sh/ruff/issues/889
synced_at: 2026-01-10T12:09:58Z
```

# "F841 local variable unused" triggering erroniously

---

_Issue opened by @jhallard on 2022-11-23 07:34_

Example code below:

```
    # Verify the annotation is coming from two different sources
    annotations = get_annotations(tdm_client, node_uid)

    assert len({annotations["source_uid"] for annotations in annotations}) == 2
```

This is triggering for me

```
.../.../test_annotation_filter.py:352:5: F841 Local variable `annotations` is assigned to but never used
```

---

_Comment by @charliermarsh on 2022-11-23 14:47_

Thanks for this! Will fix today.

---

_Label `bug` added by @charliermarsh on 2022-11-23 14:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-23 14:47_

---

_Closed by @charliermarsh on 2022-11-23 15:13_

---

_Comment by @jhallard on 2022-11-23 16:17_

Thank you!!

---

_Comment by @charliermarsh on 2022-11-23 22:41_

Going out in [v0.0.136](https://github.com/charliermarsh/ruff/releases/tag/v0.0.136) -- sorry for the delay :)

---

_Comment by @jhallard on 2022-11-23 22:44_

Incredible turn around.. we just adopted `ruff` internally at my company and are seeing 50x speedups over our flake8, black, and isort lint stack!

---

_Comment by @charliermarsh on 2022-11-23 23:09_

@jhallard - Amazing! Don't hesitate to file issues or feature requests as they come up, would love to hear them and definitely aim to be very attentive to early adopters :)

(Separately, I've never used the enterprise product but was always a big fan of the ideas around Snorkel! I built a lot of infra and tooling for data labeling + active learning loops when I was at [Spring](https://www.springdiscovery.com/).)


---
