```yaml
number: 4795
title: PyCharm plugin potential trademark issues
type: issue
state: closed
author: InSyncWithFoo
labels:
  - question
assignees: []
created_at: 2024-07-04T00:09:16Z
updated_at: 2024-07-05T23:47:13Z
url: https://github.com/astral-sh/uv/issues/4795
synced_at: 2026-01-12T15:58:52Z
```

# PyCharm plugin potential trademark issues

---

_@InSyncWithFoo_

I'm writing [a PyCharm plugin for uv](https://github.com/InSyncWithFoo/uv-for-pycharm) (barely functional as of yet), which I would like to eventually publish to JetBrains Marketplace under the same name (i.e. "<i>uv</i>"). Are there any trademark or relevant issues I need to know about?

By the way, the placeholders in this line of `LICENSE-APACHE` should have been substituted:

https://github.com/astral-sh/uv/blob/de40f798b936024f3e3c41f5a110683b245c952f/LICENSE-APACHE#L189

---

_Comment by @charliermarsh on 2024-07-05 22:06_

Thanks for asking. We'll likely publish an official uv IntelliJ plugin at some point so I'd personally prefer if you could use a name that marked the plugin as third-party or community-built, to avoid publishing something that would be confusingly similar.

Separately:

> By the way, the placeholders in this line of LICENSE-APACHE should have been substituted.

I believe what we have there is correct. You can see that (for example) [Cargo](https://github.com/rust-lang/cargo/blob/be1db6ca60b136a33849bb8905d00e1a7e7040e2/LICENSE-APACHE#L189) has the same thing. That section of the license is the appendix, and it exists to communicate to others how they can apply to license to their own work, IIUC. (Could be wrong!)


---

_Label `question` added by @charliermarsh on 2024-07-05 22:07_

---

_Closed by @InSyncWithFoo on 2024-07-05 23:47_

---
