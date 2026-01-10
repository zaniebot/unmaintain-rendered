```yaml
number: 1660
title: invalid-method-override - subclassing with kwargs
type: issue
state: closed
author: brentonmcs
labels: []
assignees: []
created_at: 2025-11-27T21:44:26Z
updated_at: 2025-11-27T22:24:13Z
url: https://github.com/astral-sh/ty/issues/1660
synced_at: 2026-01-10T01:58:59Z
```

# invalid-method-override - subclassing with kwargs

---

_Issue opened by @brentonmcs on 2025-11-27 21:44_

### Summary

Hi,

We have a base class that we subclass through our solution which has a method:

`def transform(self, **child_data: pd.DataFrame) -> pd.DataFrame:`

Between alpha 27 and 28 this now reports this as invalid-method-override when we subclass as

`def transform( self, query1 : pd.DataFrame, query2 : pd.DataFrame) -> pd.DataFrame:`

Is this expected?

### Version

ty 0.0.1-alpha.28

---

_Comment by @AlexWaygood on 2025-11-27 22:23_

Hi! Yes, this is expected. The signature of `transform()` on your base class means that the method will accept arbitrary keyword arguments without erroring, but the signature of `transform()` on your subclass only accepts two possible keyword arguments (`query1` and `query2`). That means that there are many possible combinations of keyword arguments that would be permitted by the method on the base class but are not permitted by the method on the subclass, which is a violation of the Liskov Substitution Principle 

---

_Closed by @AlexWaygood on 2025-11-27 22:24_

---
