```yaml
number: 2987
title: "When searching for fixed strings (-F), dash (-) in pattern should be ignored, but it returns  `unrecognized flag ->`"
type: issue
state: closed
author: tivrfoa
labels: []
assignees: []
created_at: 2025-02-13T22:27:46Z
updated_at: 2025-02-13T22:36:32Z
url: https://github.com/BurntSushi/ripgrep/issues/2987
synced_at: 2026-01-12T16:13:25Z
```

# When searching for fixed strings (-F), dash (-) in pattern should be ignored, but it returns  `unrecognized flag ->`

---

_@tivrfoa_

#### Describe your feature request

When searching for fixed strings (-F), dash (-) in pattern should be ignored, but it returns rg: unrecognized flag ->

I looked at `man` page, and saw that I can use `-e`, but it's strange, because I'm not looking for a regex pattern.

In my case, I was searching: `rg -F '-> Result'`

I also tested, and realized that I can combine both: `rg -Fe '-> Result'`, which already solves my problem.
So this is a small issue.

I'll still open the issue, because I think it's unintuitive when using `-F`
but I understand that handling starting with `-` is edge case, and would probably complicate things.

It's great that `-Fe` works, though!! 😄 

---

_Comment by @BurntSushi on 2025-02-13 22:36_

This is intended behavior. `-F` is a boolean switch that doesn't accept a parameter. So it isn't possible to disambiguate a pattern starting with a `-` and some other flag. Hence why you need `-e`, which always knows the next argument is a pattern.

A "pattern" is common terminology for not just a regex but also a plain literal.

---

_Closed by @BurntSushi on 2025-02-13 22:36_

---
