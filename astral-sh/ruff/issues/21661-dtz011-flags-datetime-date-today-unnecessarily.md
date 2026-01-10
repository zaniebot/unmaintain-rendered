```yaml
number: 21661
title: "DTZ011 flags `datetime.date.today()` unnecessarily for pure date usage"
type: issue
state: closed
author: Jason-Francis-github
labels:
  - question
assignees: []
created_at: 2025-11-27T12:39:49Z
updated_at: 2025-12-10T17:23:08Z
url: https://github.com/astral-sh/ruff/issues/21661
synced_at: 2026-01-10T11:10:00Z
```

# DTZ011 flags `datetime.date.today()` unnecessarily for pure date usage

---

_Issue opened by @Jason-Francis-github on 2025-11-27 12:39_

### Summary


#### **Description**
The `DTZ011` rule currently flags `datetime.date.today()` as a violation, suggesting the use of `datetime.datetime.now(tz=...).date()` instead. While this makes sense for timezone-aware **datetime** operations, it feels overly strict when the requirement is for a **date only**, without any time component.

For example:

```python
from datetime import date

registration_date = date.today().strftime("%d/%m/%Y")
```

This is a common and reasonable pattern when working with dates in isolation (e.g., formatting for display, storing a date-only field). Time zones are irrelevant in this context because there is no ambiguity about hours or minutes.

---

#### **Why this matters**
- Enforcing timezone awareness for pure dates adds unnecessary complexity.
- The suggested alternative (`datetime.now(tz=...).date()`) introduces overhead without practical benefit for date-only use cases.
- Many projects (especially tests or business logic dealing with dates) will need to add `# noqa: DTZ011` or disable the rule entirely, reducing the value of linting.

---

#### **Proposed Solutions**
1. **Allow `date.today()` by default**  
   Exempt `datetime.date.today()` from `DTZ011` since it does not suffer from the same ambiguity as naive `datetime`.

2. **Introduce a separate rule or configuration**  
   - Keep `DTZ011` for `datetime.now()` and similar datetime operations.
   - Add an option to ignore date-only functions like `date.today()`.

3. **Document rationale clearly**  
   If the rule is intended to apply universally, clarify why `date.today()` should be considered problematic.

---

#### **Example of current behavior**
```python
# Current flagged usage:
from datetime import date
registration_date = date.today().strftime("%d/%m/%Y")  # noqa: DTZ011

# Suggested alternative (adds unnecessary complexity for date-only):
from datetime import datetime
from zoneinfo import ZoneInfo
registration_date = datetime.now(ZoneInfo("Europe/London")).date().strftime("%d/%m/%Y")
```

---

#### **Expected behavior**
No lint error for `date.today()` when used for date-only formatting or logic.


---

_Label `question` added by @MichaReiser on 2025-11-28 08:42_

---

_Comment by @MichaReiser on 2025-11-28 08:44_

Hi. `DTZ011` only flags `date.today()` usages. I think you can just disable the rule if, for your use case, the time zone part isn't important.

> Introduce a separate rule or configuration

This is already the case today. `datetime.now` is [`DTZ005`](https://docs.astral.sh/ruff/rules/call-datetime-now-without-tzinfo/)

---

_Closed by @MichaReiser on 2025-12-10 17:23_

---
