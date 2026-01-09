---
number: 20449
title: Path to not forking lsp-types?
type: issue
state: closed
author: musicinmybrain
labels:
  - question
assignees: []
created_at: 2025-09-17T05:51:57Z
updated_at: 2025-09-17T07:10:55Z
url: https://github.com/astral-sh/ruff/issues/20449
synced_at: 2026-01-07T13:12:16-06:00
---

# Path to not forking lsp-types?

---

_Issue opened by @musicinmybrain on 2025-09-17 05:51_

### Question

Looking at the git dependency on a forked `lsp-types`,

https://github.com/astral-sh/ruff/blob/d121a76aeff8388c482410a0f65723c60821d3c2/Cargo.toml#L121-L123

and considering the text in https://github.com/astral-sh/lsp-types/commit/ddc7dc8b374dedd947abf14bdb4a37f257a53e92,

> This fork is a temporary solution for supporting Jupyter Notebooks for our new LSP server, `ruff server`.
> This fork is not actively maintained by Astral.

I wonder what the path to using an unmodified copy of `lsp-types` as a regular dependency would look like. Is this approach still considered temporary?

### Version

_No response_

---

_Label `question` added by @musicinmybrain on 2025-09-17 05:51_

---

_Comment by @MichaReiser on 2025-09-17 07:10_

See https://github.com/astral-sh/ruff/issues/11595 for some previous discussion. 

You're right, that this work is probably less temporary than we hoped when we created it. However, there's no real rush to unfork because there have only been very few and minor upstream changes and our fork is working fine. #11595 mentions a few alternatives that we might consider once we need new LSP features that aren't part of our fork, but until then, it's hard for us to prioritize the unfork without any real gain. 

---

_Closed by @MichaReiser on 2025-09-17 07:10_

---
