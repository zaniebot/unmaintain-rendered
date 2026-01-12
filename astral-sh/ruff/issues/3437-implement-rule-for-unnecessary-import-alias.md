```yaml
number: 3437
title: Implement rule for unnecessary import alias
type: issue
state: closed
author: stinodego
labels:
  - question
assignees: []
created_at: 2023-03-10T12:32:03Z
updated_at: 2023-03-12T17:45:54Z
url: https://github.com/astral-sh/ruff/issues/3437
synced_at: 2026-01-12T15:54:43Z
```

# Implement rule for unnecessary import alias

---

_@stinodego_

This is a feature request to implement the following lint from the `flake8-tidy-imports` package:
https://github.com/adamchainz/flake8-tidy-imports#i250-unnecessary-import-alias

Apparently, it is also an `isort` rule:
https://pycqa.github.io/isort/docs/configuration/options.html#remove-redundant-aliases

---

_Renamed from "Implement `TID250`: Unnecessary import alias" to "Implement rule for unnecessary import alias" by @stinodego on 2023-03-10 12:34_

---

_Comment by @charliermarsh on 2023-03-10 20:59_

I think we support this as `PLC0414`, Pylint's "useless-import-alias" rule.

---

_Comment by @charliermarsh on 2023-03-10 21:00_

In combination with `PLR0402`, Pylint's "consider-using-from-import" rule.

---

_Label `question` added by @charliermarsh on 2023-03-11 20:53_

---

_Comment by @stinodego on 2023-03-12 15:29_

Oh great, that was indeed what I was looking for! Thanks for the help.

Still, I think this raises another question, which is: what to do about rules that are implemented by multiple sources? I'd argue that by enabling the `TID` lints, I should expect these rules to be enabled.

In any case, I have what I need for now. I'll close this issue - the point I raised above could get its own separate issue if you feel it's merited.

---

_Closed by @stinodego on 2023-03-12 15:29_

---

_Comment by @charliermarsh on 2023-03-12 17:45_

Attaching this to #2186, where we're tracking these kinds of known aliases. I agree that enabling `TID` should give you this rule, and internally we actually _do_ support rule aliasing, we just haven't figured out all of the details -- there are some tricky cases which make the "right" behavior unclear.

---
