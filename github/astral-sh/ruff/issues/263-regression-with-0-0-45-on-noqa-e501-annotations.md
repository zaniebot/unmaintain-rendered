---
number: 263
title: "Regression with 0.0.45 on `noqa:E501` annotations"
type: issue
state: closed
author: amotl
labels: []
assignees: []
created_at: 2022-09-23T22:01:56Z
updated_at: 2022-09-23T23:00:18Z
url: https://github.com/astral-sh/ruff/issues/263
synced_at: 2026-01-07T13:12:14-06:00
---

# Regression with 0.0.45 on `noqa:E501` annotations

---

_Issue opened by @amotl on 2022-09-23 22:01_

Dear Charlie,

while it succeeded before, `ruff==0.0.45` just failed on [^1] at [^2] with a few admonitions. You can easily inspect an example spot at [secpi/model/service.py#L29-L38](https://github.com/isarengineering/SecPi/blob/3838620735b4a117a3e1c39742c69dadfae8b7b0/secpi/model/service.py#L29-L38), where we are annotating long comment lines within the docstring with `noqa:E501`.
```
echo '{"action": "shutdown"}' | amqp-publish --url="amqp://guest:guest@localhost:5672" --routing-key=secpi-op-1  # noqa:E501
```

With kind regards,
Andreas.

[^1]: https://github.com/isarengineering/SecPi/pull/27
[^2]: https://github.com/isarengineering/SecPi/actions/runs/3115805304/jobs/5053061630

---

_Comment by @charliermarsh on 2022-09-23 22:09_

Thank you! I shipped a big refactor to that logic. Will review this case. Apologies for the inconvenience.

---

_Comment by @charliermarsh on 2022-09-23 22:13_

I'm a little torn. The desired way to express this now is:

```py
def got_operational(self, ch, method, properties, body):
    """
    AMQP: Receive and process operational messages.

    Currently, this implements the handler for the shutdown signal, which is mostly
    needed in testing scenarios.

    Usage::
        echo '{"action": "shutdown"}' | amqp-publish --url="amqp://guest:guest@localhost:5672" --routing-key=secpi-op-1
    """  # noqa:E501
    ...
```

That is, the noqa goes at the end of the multi-line string, which is similar to Flake8. We could support _both_ (at the end of the multi-line string _or_ within it), but it adds some complexity and I didn't think it was warranted.

---

_Referenced in [isarengineering/SecPi#28](../../isarengineering/SecPi/pulls/28.md) on 2022-09-23 22:57_

---

_Comment by @amotl on 2022-09-23 23:00_

Hi again,

I don't know where I picked up that habit to place `noqa:E501` annotations, but I will be very happy to unlearn it. I always found it a bit silly because it would pollute the docstring content. -- https://github.com/isarengineering/SecPi/pull/28

Thanks again for your quick help!

Cheers,
Andreas.


---

_Closed by @amotl on 2022-09-23 23:00_

---
