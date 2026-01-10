```yaml
number: 3226
title: "[`tryceratops`]: Too Many Try Statements"
type: pull_request
state: closed
author: colin99d
labels: []
assignees: []
base: main
head: too_many_try
created_at: 2023-02-25T19:22:15Z
updated_at: 2024-06-10T18:13:00Z
url: https://github.com/astral-sh/ruff/pull/3226
synced_at: 2026-01-10T21:55:59Z
```

# [`tryceratops`]: Too Many Try Statements

---

_Pull request opened by @colin99d on 2023-02-25 19:22_

Merge after #3109 because this closes: #2056 

---

_Renamed from "['tryceratops']: Too Many Try Statements" to "[`tryceratops`]: Too Many Try Statements" by @colin99d on 2023-02-25 23:44_

---

_Comment by @charliermarsh on 2023-02-27 00:14_

I'm conflicted on this one. I see the intent of what the rule is trying to do, but even in the [example](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC101.md), I'm not certain the fixed version is better:

```py
def main_function():
    try:
        receipt_note = receipt_service.create(order_id)
        broker.emit_receipt_note(receipt_note)
    except ReceiptNoteCreationFailed:
        logger.exception("log")
        raise
    except NoteEmissionFailed:
        logger.exception("another log")
        raise
```

Since it's now unclear which statement is expected to throw which exception. I ran this over Zulip too and it hit some cases where it wouldn't make sense to refactor in this way (e.g., a function with a `for` loop that handles an exception in body, followed by another exception handler way down after the loop). In general, I actually think handling exceptions in the narrowest scope possible is _good_.

What do you think?


---

_Comment by @colin99d on 2023-02-27 00:34_

I think the bigger issue is if we have two separate blocks catching a ValueError.

---

_Comment by @colin99d on 2023-02-27 17:44_

Im gonna go ahead and close this.

---

_Closed by @colin99d on 2023-02-27 17:44_

---

_Comment by @ericbuehl on 2024-06-10 16:08_

I have a strong preference for the opposite of what [TRY101](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TRY101.md) promotes -- specifically addressed by [Pylint W0717](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/too-many-try-statements.html) (unfortunately using the same name as this proposed rule 🙃).  Is there any ongoing work to implement Pylint W0717 in ruff?  If not I may take a stab at it

---

_Comment by @Pierre-Sassoulas on 2024-06-10 18:12_

Seems like we could rename the pylint check to ``too-many-statements-inside-try`` to differentiate the two.

---
