```yaml
number: 13445
title: Test cases failing with regex changes for S608
type: pull_request
state: closed
author: DataEnggNerd
labels: []
assignees: []
base: main
head: main
created_at: 2024-09-22T03:17:58Z
updated_at: 2024-09-30T17:59:35Z
url: https://github.com/astral-sh/ruff/pull/13445
synced_at: 2026-01-12T15:55:44Z
```

# Test cases failing with regex changes for S608

---

_@DataEnggNerd_

@MichaReiser 

I have made the changes as suggested in https://github.com/astral-sh/ruff/issues/12044#issuecomment-2357678761. 

But this regex is failing for 7 test cases out of 58 in https://github.com/astral-sh/ruff/blob/9259cf91d8dd042f45147fa1be97662833c91919/crates/ruff_linter/resources/test/fixtures/flake8_bandit/S608.py 
(For additional test cases, refer - https://github.com/astral-sh/ruff/blob/257359165f845adc1a2bd41aeabfe6d56fbee420/crates/ruff_linter/resources/test/fixtures/flake8_bandit/S608.py#L117 )

I have implemented the regex from bandit as well. Commit - https://github.com/astral-sh/ruff/commit/9259cf91d8dd042f45147fa1be97662833c91919, which is failing for 3 test cases. 

---

_Comment by @MichaReiser on 2024-09-23 08:26_

I looked into this more and the reason why the multiline f-strings aren't picked up are because of `concatenated_f_string` that escapes special characters like `\n`. 

https://github.com/astral-sh/ruff/blob/257359165f845adc1a2bd41aeabfe6d56fbee420/crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs#L124-L129

It's not entirely clear to me why @dhruvmanila added the `escape_default` call but I'm  sure there's a reason. I want to wait for his reply before changing the behavior. However, he's off this week, so his reply might be a bit delayed.

---

_Comment by @DataEnggNerd on 2024-09-23 17:29_

Meanwhile I will try playing with these configs. Thanks @MichaReiser 

---

_Comment by @dhruvmanila on 2024-09-30 10:59_

> It's not entirely clear to me why @dhruvmanila added the `escape_default` call but I'm sure there's a reason.

Looking back I'm not exactly sure why I added this but I suspect that it was to make sure the regex matched multiline strings. I should've updated the regex instead back then to avoid this bug from ever occurring. I would remove it and update the regex and run the tests. Sorry for the churn @DataEnggNerd.

---

_Comment by @DataEnggNerd on 2024-09-30 17:07_

@dhruvmanila @MichaReiser All checks passed while removing `concatenated_f_string`.


---

_Marked ready for review by @DataEnggNerd on 2024-09-30 17:14_

---

_Closed by @DataEnggNerd on 2024-09-30 17:33_

---

_Comment by @MichaReiser on 2024-09-30 17:50_

What's the reason for closing this pr? It sounded like everything working 

---

_Comment by @DataEnggNerd on 2024-09-30 17:59_

CI was failing as I have not updated the snapshot. working on it. 

---
