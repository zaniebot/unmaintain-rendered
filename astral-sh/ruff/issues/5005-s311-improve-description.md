```yaml
number: 5005
title: "S311: improve description"
type: issue
state: closed
author: spaceone
labels:
  - documentation
assignees: []
created_at: 2023-06-10T13:22:18Z
updated_at: 2025-03-26T05:26:32Z
url: https://github.com/astral-sh/ruff/issues/5005
synced_at: 2026-01-10T11:09:47Z
```

# S311: improve description

---

_Issue opened by @spaceone on 2023-06-10 13:22_

```
import random
random.randrange(1, 255)

 $ ruff foo.py
warning: One or more modules are part of multiple import sections.
foo.py:2:1: S311 Standard pseudo-random generators are not suitable for cryptographic purposes
Found 1 error.
$ ruff rule S311
# suspicious-non-cryptographic-random-usage (S311)

Derived from the **flake8-bandit** linter.

Message formats:
* Standard pseudo-random generators are not suitable for cryptographic purposes
```

The description speaks about cryptographic purposes but I don't have any cryptographic context and the rule is also not checking for any. So the rule help could describe this.

---

_Comment by @dhruvmanila on 2023-06-10 14:39_

FYI, `bandit` produces the same result:
```
Test results:
>> Issue: [B311:blacklist] Standard pseudo-random generators are not suitable for security/cryptographic purposes.
   Severity: Low   Confidence: High
   CWE: CWE-330 (https://cwe.mitre.org/data/definitions/330.html)
   More Info: https://bandit.readthedocs.io/en/1.7.5/blacklists/blacklist_calls.html#b311-random
   Location: src/S311.py:3:0
2	
3	random.randrange(1, 255)
```

---

_Label `documentation` added by @dhruvmanila on 2023-06-10 14:39_

---

_Comment by @charliermarsh on 2023-06-10 14:40_

I think this can be considered part of #2646. We've greatly expanded the number of rules that have custom documentation (the above is just auto-generated from the rule message), but we haven't gotten to the Bandit rules yet.

---

_Closed by @charliermarsh on 2023-06-10 14:40_

---

_Comment by @andrei-korshikov on 2025-03-26 05:26_

#2646 is completed, but this rule still does not have "custom documentation":)

I do agree with @spaceone , my first thought was exactly the same: "Hey, I do not have any crypto purposes!" So, while the phrase `Standard pseudo-random generators are not suitable for cryptographic purposes` is totally correct, it is a bit confusing indeed.

My suggestion: replace it with `Use of standard pseudo-random generators`.

---
