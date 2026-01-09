---
number: 19685
title: Allow for certain global names in F821
type: issue
state: open
author: williambdean
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-08-01T12:41:09Z
updated_at: 2025-08-01T19:49:47Z
url: https://github.com/astral-sh/ruff/issues/19685
synced_at: 2026-01-07T13:12:16-06:00
---

# Allow for certain global names in F821

---

_Issue opened by @williambdean on 2025-08-01 12:41_

### Summary

Interested in F821 but with certain names allowed. For example, `spark` or `dbutils` are auto injected into Databrick notebooks

Reference: 
- https://docs.astral.sh/ruff/rules/undefined-name/
- https://docs.databricks.com/aws/en/developers/

---

_Comment by @ntBre on 2025-08-01 17:30_

I didn't find anything on that page about the automatic injection, but I did see in one of the [tutorials](https://docs.databricks.com/aws/en/getting-started/dataframes#step-3-load-data-into-a-dataframe-from-csv-file) that they use `spark` without importing it.

I think this could make sense as a configuration option. I don't think there's any other workaround besides ignoring the rule for the whole notebook file. Unless you can import them anyway, but I'm guessing that could fail depending on how they inject them.

---

_Label `configuration` added by @ntBre on 2025-08-01 17:30_

---

_Label `needs-decision` added by @ntBre on 2025-08-01 17:30_

---

_Comment by @williambdean on 2025-08-01 19:49_

Thanks for the reply.

Sorry, may be a bad Databricks reference. I personally find their docs a bit confusing. I think they auto inject a bunch of stuff into locals. Most users (and their notebooks) just reflect that convention so getting users to change that behavior is tough.

Yes, the work around of ignoring on the whole file is fine but removes the functionality

---
