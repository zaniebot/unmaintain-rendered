---
number: 9876
title: "The `show-source` deprecation warning should have a clearer call to action"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
assignees: []
created_at: 2024-02-07T16:14:31Z
updated_at: 2024-07-12T03:32:27Z
url: https://github.com/astral-sh/ruff/issues/9876
synced_at: 2026-01-07T13:12:15-06:00
---

# The `show-source` deprecation warning should have a clearer call to action

---

_Issue opened by @zanieb on 2024-02-07 16:14_

"Please update your configuration" should be more specific. 

Raised in https://github.com/astral-sh/ruff/pull/9874

---

_Label `good first issue` added by @zanieb on 2024-02-07 16:14_

---

_Comment by @AbhinavMir on 2024-02-11 04:26_

Hi @zanieb - love Ruff, would love to contribute.

What's a good replacement? 

Current message: `show-source` is deprecated and is now part of `output-format` in the form of `full` or `concise` options. Please update your configuration.

Modified message: `show-source` is deprecated and is now part of `output-format` in the form of `full` or `concise` options. Please update your configuration by replacing `show-source` with either `output-format = full` for viewing source code or `output-format = concise` (set as the default) for not showing source. This can be done in your configuration file. 

---

_Comment by @zanieb on 2024-02-11 05:15_

Hey @AbhinavMir! Love to hear that :)

I don't think we need to be quite so verbose. I haven't given the exact message a lot of thought but here are some ideas. This is a bit more work, but we should actually be able to _tell_ which value they're using so if they use `show-source=true` we can say 

> The `show-source` option has been deprecated. Use `output-format=full` instead.

and for `show-source=false` we can say

> The `show-source` option has been deprecated. Use `output-format=text` instead.

We may also be able to tell if this was provided via the CLI or a configuration file. If so, we can make even better suggestions e.g.

> The `show-source` option has been deprecated. Update your configuration file to use `output-format = text` instead.

and

> The `--show-source` option has been deprecated. Use `--output-format=full` instead.





---

_Comment by @AbhinavMir on 2024-02-11 06:31_

Gotcha, I can probably prod into the code a bit more tomorrow, but it'd be great if you could point out where the options are being read from. The idea, I assume is, read the flags, show the correct output - but just wondering how this is architectured in the code base.

---

_Comment by @dylwil3 on 2024-07-12 03:10_

Should this issue be closed in light of #9814, which removes `show-source` as an option (along with its deprecation warning)?

---

_Closed by @zanieb on 2024-07-12 03:32_

---

_Comment by @zanieb on 2024-07-12 03:32_

Thanks!

---
