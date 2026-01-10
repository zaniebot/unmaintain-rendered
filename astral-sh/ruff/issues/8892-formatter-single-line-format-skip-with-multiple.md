```yaml
number: 8892
title: "Formatter: `single_line_format_skip_with_multiple_comments` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-11-29T01:36:48Z
updated_at: 2024-01-08T08:38:08Z
url: https://github.com/astral-sh/ruff/issues/8892
synced_at: 2026-01-10T11:09:51Z
```

# Formatter: `single_line_format_skip_with_multiple_comments` preview style

---

_Issue opened by @MichaReiser on 2023-11-29 01:36_

Implement Black's [`single_line_format_skip_with_multiple_comments`](https://github.com/psf/black/pull/3959) style in Ruff. 

I don't think it's necessary to gate this change as a preview style. We should also allow the same for `fmt:off` and `fmt:on`. 


```
# fmt:skip                           <-- single comment
# noqa:XXX # fmt:skip # a nice line  <-- multiple comments (Preview)
# pylint:XXX; fmt:skip               <-- list of comments (; separated, Preview)
``` 





---

_Label `formatter` added by @MichaReiser on 2023-11-29 01:37_

---

_Label `preview` added by @MichaReiser on 2023-11-29 01:37_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 01:38_

---

_Renamed from "`single_line_format_skip_with_multiple_comments ` Support using `noqa: E501` and `fmt:skip` on the same statement." to "Formatter: `single_line_format_skip_with_multiple_comments` preview style" by @MichaReiser on 2023-11-29 01:39_

---

_Comment by @charliermarsh on 2023-11-29 03:54_

I don't mind requiring a `#` to delimit each pragma -- we already have this requirement in the linter IIRC.

---

_Comment by @zanieb on 2023-11-29 04:12_

I like the `;` delimiter for pragmas, I think `#` is pretty awkward.

---

_Comment by @charliermarsh on 2023-11-29 04:13_

I find `#` more intuitive.

---

_Comment by @zanieb on 2023-11-29 04:14_

:D Are you suggesting that we should not support using `;`?

---

_Comment by @charliermarsh on 2023-11-29 04:20_

I don't feel strongly about supporting it but I probably wouldn't recommend it. We have to use something here that _all_ tools adhere to, and `#` is likely the most universal and portable.

---

_Comment by @georgettica on 2023-12-13 09:55_

for me, the big addition is I can explain in the same comment I am skipping the formatting what led me to this

also, I am against the `;` as its a new delimiter, and `#` is that we have and use now

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-05 00:13_

---

_Closed by @charliermarsh on 2024-01-05 00:39_

---

_Comment by @MichaReiser on 2024-01-08 08:38_

Thanks @charliermarsh for implementing this. 

Allowing `#` is a good first step. Supporting `;` or `:` is something that we may want to support in the future. For example, Biome has a standardized format for specifying the reason using `biome-ignore rule: <reason>.` Standardizing the format has the advantage that downstream tools (e.g., a dashboard) can use the explanation. 

I recommend deferring this until we look into suppression comments holistically. 

---
