```yaml
number: 1055
title: Not selecting a valid return type with overloaded methods
type: issue
state: closed
author: brentonmcs
labels: []
assignees: []
created_at: 2025-08-19T23:32:18Z
updated_at: 2025-08-20T00:46:11Z
url: https://github.com/astral-sh/ty/issues/1055
synced_at: 2026-01-12T15:54:24Z
```

# Not selecting a valid return type with overloaded methods

---

_@brentonmcs_

### Summary

This might be a pandas issue but I thought I'd see if it could be resolved.

I'm triggering a unresolved-attribute when working with the pandas pd.to_datetime call as it's picking the type of one of the overloaded methods (Timestamp) but in the context it should be picking up the Series type. 

code is this

`df.assign(_event_date=lambda df: pd.to_datetime(df.event_date).dt.date)`



### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Comment by @carljm on 2025-08-20 00:46_

Thanks for the report! This is due to the use of type aliases in the overload definitions, should be fixed along with #544 

---

_Closed by @carljm on 2025-08-20 00:46_

---
