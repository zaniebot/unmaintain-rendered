```yaml
number: 6861
title: Ruff and Black disagree on spacing between slice indices
type: issue
state: closed
author: danparizher
labels:
  - question
assignees: []
created_at: 2023-08-25T02:35:28Z
updated_at: 2023-09-17T17:56:22Z
url: https://github.com/astral-sh/ruff/issues/6861
synced_at: 2026-01-12T15:54:46Z
```

# Ruff and Black disagree on spacing between slice indices

---

_@danparizher_

When running `ruff . --fix` on this line of code, it deletes a space between `1` and `:`, which is inconsistent with the Black formatter.
```diff
- self.reduce_section_group(merged_sections[i - 1 : i + 2])
+ self.reduce_section_group(merged_sections[i - 1: i + 2])
```
Running `black .` reverts this change

Another snippet example:
```diff
- for row in self.data[self.cur_page * 10 - 10 : self.cur_page * 10]
+ for row in self.data[self.cur_page * 10 - 10: self.cur_page * 10]
```

---

_Comment by @charliermarsh on 2023-08-25 02:43_

Can you share your Ruff configuration?

---

_Comment by @danparizher on 2023-08-25 02:50_

@charliermarsh I have found that it originates from Nursery Rule `E203`. So it is a disagreement between Pycodestyle and Black?

---

_Comment by @charliermarsh on 2023-08-25 03:44_

@Arborym - Yeah, that's right. We don't enable any rules by default that conflict with Black, but Pycodestyle (and, by extension, Flake8) contain some, a few of which would be enabeld via `NURSERY`. See: https://black.readthedocs.io/en/stable/guides/using_black_with_other_tools.html#flake8. I'd recommend turning off that rule explicitly if you're using the nursery set alongside Black.

---

_Label `question` added by @charliermarsh on 2023-08-25 03:44_

---

_Closed by @charliermarsh on 2023-08-25 21:24_

---

_Comment by @Avasam on 2023-09-17 17:56_

This issue now extends to Ruff's own formatter. But I assume the answer would be the same: Just disable that rule. So I won't open a new issue for it. 

Also https://github.com/astral-sh/ruff/discussions/7305#discussioncomment-6980767
> [...] there are some unanswered questions about how the linter/formatter configuration should co-exist.
> 
> There's no definite timeline but we plan to add configuration support as part of our [beta release](https://github.com/astral-sh/ruff/milestone/5). Expect it to take a couple of weeks, maybe even more, depending on the user feedback.

---
