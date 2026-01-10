```yaml
number: 15818
title: "`native-auth` store is not read for domain during `uv -v sync`"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2025-09-12T16:59:21Z
updated_at: 2025-09-19T12:40:56Z
url: https://github.com/astral-sh/uv/issues/15818
synced_at: 2026-01-10T03:23:54Z
```

# `native-auth` store is not read for domain during `uv -v sync`

---

_Issue opened by @zanieb on 2025-09-12 16:59_

> The native store doesn't support "prefix" matches yet. I think we need to fix that before you example will work. Can you login to `www.reportlab.com` instead? (We do support domain-level matches)
> 
> p.s. Thank you for trying the feature and taking the time to provide feedback!
> 
> I can confirm the issue and tried the domain login you suggested, means using
> `UV_PREVIEW_FEATURES=native-auth uv auth login www.my_company.com`
> 
> Then I try:
> `UV_PREVIEW_FEATURES=native-auth uv -v sync`
> and see in the debug output, that uv is looking for plain text credentials:
> `DEBUG Loaded credential file /home/...`
> ...
> `DEBUG Checking text store for credentials for https://www.my_company.com/...`
> ...
> `  ╰─▶ Missing credentials for https://www.my_company.com/...`
> 
> If I login with plain text storage, `UV_PREVIEW_FEATURES=native-auth uv -v sync` also works. 

 _Originally posted by @RStreitfeld in [#8810](https://github.com/astral-sh/uv/issues/8810#issuecomment-3285980607)_

---

_Closed by @zanieb on 2025-09-15 18:47_

---

_Comment by @RStreitfeld on 2025-09-19 11:14_

Thanks for the fix and the great work!

---

_Comment by @zanieb on 2025-09-19 12:40_

Thank you!

---
