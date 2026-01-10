```yaml
number: 12718
title: "[docs] Convert single line list to multiline in configuration examples"
type: pull_request
state: closed
author: Cjkjvfnby
labels:
  - documentation
assignees: []
base: main
head: sa/update-config-example-in-docs
created_at: 2024-08-06T17:28:31Z
updated_at: 2024-08-08T12:29:39Z
url: https://github.com/astral-sh/ruff/pull/12718
synced_at: 2026-01-10T21:47:02Z
```

# [docs] Convert single line list to multiline in configuration examples

---

_Pull request opened by @Cjkjvfnby on 2024-08-06 17:28_

## Summary

It's a good practice to write configuration as a multiline list with trailing commas. Such a format allows you to have a simple and clear diff when you maintain configuration.

Having comments per rule is much more easy to support than one comment for all of them.

My changes break a pattern for numbers per config section, not only do some of them have numbers, but maybe remove other numbers as well.

Documentation page that will be updated: https://docs.astral.sh/ruff/configuration/



---

_Comment by @MichaReiser on 2024-08-07 05:52_

Thanks for working on the documentation. 

I don't think there's a strong agreement that one is preferred. Especially in documentation where a more dense form can improve readability. I prefer to keep the documentation as is to save some space. But interested to hear what others think

---

_Label `documentation` added by @MichaReiser on 2024-08-07 05:52_

---

_Comment by @charliermarsh on 2024-08-08 12:26_

I generally prefer multiline, but I agree with Micha that we should leave as-is to keep the docs more dense.

---

_Closed by @MichaReiser on 2024-08-08 12:29_

---
