```yaml
number: 17970
title: Ruff as a rust crate / API?
type: issue
state: open
author: twitchyliquid64
labels:
  - release
assignees: []
created_at: 2025-05-08T21:48:10Z
updated_at: 2025-05-09T06:01:09Z
url: https://github.com/astral-sh/ruff/issues/17970
synced_at: 2026-01-12T15:54:56Z
```

# Ruff as a rust crate / API?

---

_@twitchyliquid64_

Hi there!

Theres a lot of awesome work in Ruff that I would love to reuse, notably Ruffs parser & AST. I was wondering if there was interest in making some of the ruff internals available as `crates.io` crates?

Alternatively, I would be open to making a copy of some of those core crates into their own crate that I'll publish (with attribution + same license + your blessing), similar to what I did with https://github.com/twitchyliquid64/golang-asm.

Cheers
Tom

---

_Comment by @MichaReiser on 2025-05-09 06:01_

See https://github.com/astral-sh/ruff/issues/43 for previous discussions around publishing to crates.io. 

I'm somewhat open to publishing the parser and AST crates to crates.io if someone puts the effort into setting it up as part of our release pipeline (with big disclaimers that the crates could change at any time)

> Alternatively, I would be open to making a copy of some of those core crates into their own crate that I'll publish (with attribution + same license + your blessing), similar to what I did with [t](https://github.com/twitchyliquid64/golang-asm?rgh-link-date=2025-05-08T21%3A48%3A10.000Z)

I don't see any problems with this for as long as you use different names. CC: @zanieb @charliermarsh 

---

_Label `release` added by @MichaReiser on 2025-05-09 06:01_

---
