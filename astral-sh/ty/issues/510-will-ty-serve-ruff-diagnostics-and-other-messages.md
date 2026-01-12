```yaml
number: 510
title: "Will `ty` serve `ruff` diagnostics and other messages via a single language server?"
type: issue
state: closed
author: jdtsmith
labels:
  - question
  - server
assignees: []
created_at: 2025-05-25T15:32:41Z
updated_at: 2025-10-05T20:26:01Z
url: https://github.com/astral-sh/ty/issues/510
synced_at: 2026-01-12T15:54:23Z
```

# Will `ty` serve `ruff` diagnostics and other messages via a single language server?

---

_@jdtsmith_

### Question

Some editor tools cannot multiplex language servers, e.g. Emacs' native `eglot`.  Will it be possible for `ty` to serve `ruff` diagnostics and other LSP capabilities over the same server connection?  Speculating, but this might also allow for more refined LSP protocol support without any duplication of effort between the two tools.

### Version

N/A

---

_Label `question` added by @jdtsmith on 2025-05-25 15:32_

---

_Label `server` added by @AlexWaygood on 2025-05-25 15:35_

---

_Comment by @vikigenius on 2025-05-25 16:25_

Not really related but even with eglot, there has been talks to be able to do this via a dedicated multiplexer: See https://github.com/thefrontside/lspx

But I do agree with the duplication part. When you run multiple servers that don't really know about each other there are definitely some duplicate diagnostics and some conflicts. Less likely here since it's the same team, but there is always a chance

---

_Comment by @MichaReiser on 2025-05-26 06:41_

Hi @jdtsmith 

Initially, no. ty and Ruff are separate tools, and both have their own language server implementation. Our long-term goal is to unify the two tools, but there are some open questions about how we'll exactly do this.

> But I do agree with the duplication part. When you run multiple servers that don't really know about each other there are definitely some duplicate diagnostics and some conflicts. Less likely here since it's the same team, but there is always a chance

This is also a problem with ty and ruff. There are some overlapping rules, and both tools show syntax errors by default. However, this can be worked around by disabling the overlapping rules or configuring the server to not show syntax errors.

In the meantime, you can use a generic LSP multiplexer, which should give you the same experience to what users get when using the ty and ruff VS Code extension.

---

_Comment by @jdtsmith on 2025-05-26 14:53_

Glad to hear it's on the roadmap, thanks.  In the meantime, do you have a recommended multiplexer that works with `ty` and `ruff`?  The only ones I've seen are quite incomplete.

---

_Comment by @MichaReiser on 2025-05-26 15:09_

> Glad to hear it's on the roadmap, thanks. In the meantime, do you have a recommended multiplexer that works with ty and ruff? The only ones I've seen are quite incomplete.

No, I don't have any experience with multiplexers (I only use zed/VS code myself)

---

_Comment by @mrzzmrzz on 2025-06-02 05:20_

> > Glad to hear it's on the roadmap, thanks. In the meantime, do you have a recommended multiplexer that works with ty and ruff? The only ones I've seen are quite incomplete.
> 
> No, I don't have any experience with multiplexers (I only use zed/VS code myself)

hey, MichaReiser. I noticed that when using the [ty plugin](https://github.com/zed-extensions/ty) with the Zed editor, it seems that the editor does not display warning/error information like the ty plugin in VSCode. I'm wondering if this is normal? Or is there a new way to show such diagnostic information in the editor? (By the way, Ruff displays diagnostic information normally in Zed)

---

_Comment by @MichaReiser on 2025-06-02 07:04_

> hey, MichaReiser. I noticed that when using the [ty plugin](https://github.com/zed-extensions/ty?rgh-link-date=2025-06-02T05%3A20%3A48.000Z) with the Zed editor, it seems that the editor does not display warning/error information like the ty plugin in VSCode. I'm wondering if this is normal? Or is there a new way to show such diagnostic information in the editor? (By the way, Ruff displays diagnostic information normally in Zed)

This should be fixed with the next release which adds support for push diagnostics (which is how Zed retrieves diagnostics) in addition to pull diagnostics (which is what vs code uses)

---

_Comment by @mrzzmrzz on 2025-06-02 07:12_

Thank you!

---

_Closed by @MichaReiser on 2025-08-15 12:09_

---

_Comment by @annamontare on 2025-10-05 17:49_

> This is also a problem with ty and ruff. There are some overlapping rules, and both tools show syntax errors by default. However, this can be worked around by disabling the overlapping rules or configuring the server to not show syntax errors.

@MichaReiser you mentioned configuring the server to not show syntax errors, how do we go about doing that? I couldn't find a configuration item or rule in the docs.

---

_Comment by @MichaReiser on 2025-10-05 18:06_

You can disable [showSyntaxErrors](https://docs.astral.sh/ruff/editors/settings/#showsyntaxerrors) in Ruff. ty doesn't support this option yet

---

_Comment by @annamontare on 2025-10-05 20:26_

Thanks!

---
