```yaml
number: 1468
title: Support for pip-compile-multi workflows?
type: issue
state: closed
author: PeterJCLaw
labels:
  - question
assignees: []
created_at: 2024-02-16T10:04:47Z
updated_at: 2024-07-16T23:23:09Z
url: https://github.com/astral-sh/uv/issues/1468
synced_at: 2026-01-12T15:58:28Z
```

# Support for pip-compile-multi workflows?

---

_@PeterJCLaw_

I see from https://github.com/astral-sh/uv/issues/815 that you're aware of `pip-compile-multi`, just wondering if you have plans to add support for that too?

---

_Label `question` added by @MichaReiser on 2024-02-16 12:15_

---

_Comment by @zanieb on 2024-02-17 05:36_

I don't think we plan to support it. The usage is small and there are simple workarounds e.g. a requirements file that includes the others. Sorry I know that's not a great answer to hear but I'd rather focus on building new workflows that solve this problem rather than adding another compatibility interface.


---

_Comment by @Mogost on 2024-06-13 12:25_

@zanieb Could you provide more details on the specific plans and features you’re considering? Additionally, how do you see uv evolving in terms of unique workflows and integration with Dependabot? Enhancing these areas and good dependabot integration could significantly popularize uv and streamline dependency management.

For reference, Dependabot is open to contributing new ecosystems, which you can find more about [here](https://github.com/dependabot/dependabot-core/blob/main/CONTRIBUTING.md#contributing-new-ecosystems).

I was too embarrassed to open a new issue, so I asked here. Moreover this ticket can probably be closed.


---

_Comment by @zanieb on 2024-06-13 13:48_

Hi!

Dependabot support is being tracked in https://github.com/astral-sh/uv/issues/2512 — of course we'd love an integration once what we're stable.

You can see all the work in progress with the [preview label](https://github.com/astral-sh/uv/issues?q=label%3Apreview+). We don't have a full roadmap written out at this time, the [readme](https://github.com/astral-sh/uv?tab=readme-ov-file#roadmap) has a blurb though.


---

_Closed by @zanieb on 2024-06-13 13:48_

---

_Comment by @Mogost on 2024-06-13 14:14_

Thanks for the clarification! 
> We don't have a full roadmap

It would be great to have one. Anyway thank you for you work on py-cargo 😃 



---

_Comment by @mermerico on 2024-07-16 23:23_

If I tried to work on a PR to implement this, would you be interested or do you feel it's not worth supporting? We're finding the sequential import workflow to be a pain.

---
