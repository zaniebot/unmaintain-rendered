```yaml
number: 5465
title: DJ012 triggers one violation per missplaced database field. Could be grouped as one single one?
type: issue
state: closed
author: EricDepagne
labels:
  - bug
assignees: []
created_at: 2023-07-02T19:33:44Z
updated_at: 2023-07-03T06:08:42Z
url: https://github.com/astral-sh/ruff/issues/5465
synced_at: 2026-01-10T11:09:47Z
```

# DJ012 triggers one violation per missplaced database field. Could be grouped as one single one?

---

_Issue opened by @EricDepagne on 2023-07-02 19:33_

Hi all.
I' m using Ruff with a django project.
Whenever DJ012 is triggered (which in my case  is when Meta Class is before the database fields fields), each violation of the same rule in the same model is counted as one.
Would it be possible to group them all tgether, as they are technically the same, so to report only one?

Here' s a short example:
```
class Activity(models.Model):
    """
    Define an activity
    """

    class Meta:
        verbose_name_plural = "Activities"
    name = models.CharField(max_length=50, blank=True)
    temp = ArrayField(
        models.PositiveSmallIntegerField(blank=True, default=0, null=True),
        default=list,
    )

```
will trigger two violations, as they are two fields after the class Meta that will trigger the rule.



---

_Label `bug` added by @charliermarsh on 2023-07-02 20:01_

---

_Comment by @charliermarsh on 2023-07-02 20:01_

Yeah, I think this would be better behavior. Nice idea.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-03 00:18_

---

_Closed by @charliermarsh on 2023-07-03 01:09_

---

_Comment by @EricDepagne on 2023-07-03 06:08_

Wow! that' s an amazing time to fix a very minor issue that I only reported because I thought it might be a good idea!
Thank you so much!


---
