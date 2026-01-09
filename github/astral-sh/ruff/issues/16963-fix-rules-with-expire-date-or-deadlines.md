---
number: 16963
title: "`FIX` rules with expire date or deadlines"
type: issue
state: open
author: edpyt
labels:
  - rule
  - wish
assignees: []
created_at: 2025-03-25T05:49:37Z
updated_at: 2025-03-28T16:02:13Z
url: https://github.com/astral-sh/ruff/issues/16963
synced_at: 2026-01-07T13:12:16-06:00
---

# `FIX` rules with expire date or deadlines

---

_Issue opened by @edpyt on 2025-03-25 05:49_

### Summary

I want to have a feature to write `# TODO:` comments with deadlines.  
Something like:  
```python
s = "1"  # TODO(<=30.03.2025): rewrite to int
```

And if today date is 01.04.2025, then in next `ruff check` i want to see an error like:  
```bash
Today is 01.04.2025 you need to fix this!
```


- https://docs.astral.sh/ruff/rules/#flake8-fixme-fix
- https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/expiring-todo-comments.md

---

_Label `wish` added by @dylwil3 on 2025-03-25 12:03_

---

_Comment by @MichaReiser on 2025-03-27 16:15_

I see the use case for this but I feel hesitant about adding this as a feature. It would be the first rule where ruff would suddenly fails even when you didn't make any changes to your project files (content or metadata). It's also not clear what a user would do in this situation because fixing a TODO often takes a fair bit of time. My most likely action would be to bump the TODO date. 



---

_Comment by @edpyt on 2025-03-28 13:28_

thanks for the reply!

> It's also not clear what a user would do 

if it will be released as a separate rule (e.g. `FIX005`), then probably:
* ignore this rule.  
  (do you think this will be inconvenient?)  
* update dates/fix todos.  

> because fixing a TODO often takes a fair bit of time

I think this is a matter of habit. user should set a correct deadline for completing code changes

also, I found that ESLint has a plugin with similar functionality: https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/expiring-todo-comments.md

##### offtop:
> My most likely action would be to bump the TODO date.

normally me, always postpone the deadlines dates :D

---

_Label `rule` added by @MichaReiser on 2025-03-28 15:15_

---

_Comment by @InSyncWithFoo on 2025-03-28 15:54_

> It would be the first rule where ruff would suddenly fails even when you didn't make any changes to your project files [...]

It's also important to mention that if one didn't make any changes since the last run, Ruff may also <em>not</em> report anything later due to the cache.

---

_Comment by @MichaReiser on 2025-03-28 16:02_

That's an excellent point. It breaks many assumptions of how ruff works

---
