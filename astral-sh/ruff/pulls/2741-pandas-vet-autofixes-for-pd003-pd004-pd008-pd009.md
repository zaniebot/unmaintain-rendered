```yaml
number: 2741
title: "[`pandas-vet`] autofixes for PD003, PD004, PD008, PD009 and PD011"
type: pull_request
state: closed
author: sbrugman
labels: []
assignees: []
base: main
head: pd-vet-autofixes
created_at: 2023-02-10T21:01:17Z
updated_at: 2023-07-12T08:49:31Z
url: https://github.com/astral-sh/ruff/pull/2741
synced_at: 2026-01-12T15:55:11Z
```

# [`pandas-vet`] autofixes for PD003, PD004, PD008, PD009 and PD011

---

_@sbrugman_

Autofixes for PD003, PD004, PD008, PD009 and PD011

---

_Review comment by @spaceone on `src/rules/pandas_vet/rules/check_attr.rs`:117 on 2023-02-10 21:07_

I consider this fix dangerous as there is no check if `.values` is actually a pandas object.

---

_@spaceone reviewed on 2023-02-10 21:07_

---

_Review comment by @sbrugman on `src/rules/pandas_vet/rules/check_attr.rs`:117 on 2023-02-10 21:12_

The fix is only applied when the use-of-dot-values is detected. (If it's not, then the rule logic should be updated, not that of the fix)

---

_@sbrugman reviewed on 2023-02-10 21:12_

---

_Marked ready for review by @sbrugman on 2023-02-10 21:41_

---

_@spaceone reviewed on 2023-02-10 22:07_

---

_Review comment by @spaceone on `src/rules/pandas_vet/rules/check_attr.rs`:117 on 2023-02-10 22:07_

yes, and `use-of-dot-values` detects every `.values` access instead of only pandas related ones: #2229

---

_@sbrugman reviewed on 2023-02-10 22:09_

---

_Review comment by @sbrugman on `src/rules/pandas_vet/rules/check_attr.rs`:117 on 2023-02-10 22:09_

Yeah, thats partially limitation of not having type information. We could try to make the detection more conservative, but if the user enables it consciously, and the detection is correct, then the fix is in principle correct.. I think.

---

_Review comment by @spaceone on `src/rules/pandas_vet/rules/check_attr.rs`:117 on 2023-02-10 22:12_

yes

---

_@spaceone reviewed on 2023-02-10 22:12_

---

_Renamed from "autofixes for pandas-vet" to "[`pandas-vet`] autofixes for PD003, PD004, PD008, PD009 and PD011" by @sbrugman on 2023-02-11 15:46_

---

_@charliermarsh reviewed on 2023-02-11 17:51_

---

_Review comment by @charliermarsh on `src/rules/pandas_vet/rules/check_attr.rs`:117 on 2023-02-11 17:51_

My concern here is just that we still have known false-positives for this rule, and so we'll be knowingly breaking code in certain places by applying these autofixes. I'm tempted to say that these should be off-by-default, but enableable by users, which we haven't done before but I'd like to start doing in some cases.

---

_@sbrugman reviewed on 2023-02-12 11:37_

---

_Review comment by @sbrugman on `src/rules/pandas_vet/rules/check_attr.rs`:117 on 2023-02-12 11:37_

This one should benefit from the autofix safety levels (https://github.com/charliermarsh/ruff/issues/1992). Is there anything we could do in the meantime to merge this, like make the tests more conservative? Implemented this one for autofixing https://github.com/ing-bank/popmon

---

_Comment by @sbrugman on 2023-02-16 08:21_

This PR probably needs autofix aggressiveness levels (https://github.com/charliermarsh/ruff/issues/1997)

---

_Closed by @sbrugman on 2023-07-11 11:11_

---

_Comment by @zanieb on 2023-07-12 00:58_

@sbrugman fwiw those levels are implemented now https://github.com/astral-sh/ruff/issues/4181 although they are not yet respected when running Ruff :)

---

_Comment by @sbrugman on 2023-07-12 08:49_

Ok, sounds good! Anyone is free to pick up this where it was left off

---
