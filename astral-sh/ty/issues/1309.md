```yaml
number: 1309
title: Multi-version checking
type: issue
state: open
author: DetachHead
labels:
  - wish
assignees: []
created_at: 2025-10-05T10:51:53Z
updated_at: 2025-11-18T16:10:39Z
url: https://github.com/astral-sh/ty/issues/1309
synced_at: 2026-01-10T01:58:59Z
```

# Multi-version checking

---

_Issue opened by @DetachHead on 2025-10-05 10:51_

many python projects support a range of python versions, eg. `>=3.9,<3.14`, which means having to write checks like this:

```py
if sys.version_info < (3, 13):
    ...
else:
    ...
```

currently, all the type checkers only support specifying one python version, which means one of those branches will incorrectly be treated as unreachable, when it's obvious that the developer intends for their code to run on multiple different python versions.

related: #784

---

_Comment by @AlexWaygood on 2025-10-05 10:56_

Thanks! Yes, this is a feature we'd love to have. We initially wanted to include it in ty's first alpha, but postponed it due to the complexity of the feature and the number of other things we have to do.

We already have an issue tracking multi-platform checking (#160) but it looks like we didn't have one open for multi-version checking — so thank you!

---

_Label `wish` added by @AlexWaygood on 2025-10-05 10:56_

---

_Renamed from "support multiple values / ranges for `python-version`" to "Multi-version checking" by @AlexWaygood on 2025-10-05 10:57_

---

_Comment by @DetachHead on 2025-10-05 11:12_

oh i thought multiple platforms were already supported since there's an `"all"` option for `python-platform` https://docs.astral.sh/ty/reference/configuration/#python-platform

---

_Comment by @AlexWaygood on 2025-10-05 11:19_

We support it as a configuration option but it doesn't necessarily lead to a great experience right now, which is why it isn't the default (see #160 for more details)

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
