```yaml
number: 12176
title: Show syntax error notification on manual invocation
type: issue
state: open
author: dhruvmanila
labels:
  - wish
  - server
assignees: []
created_at: 2024-07-04T04:34:09Z
updated_at: 2025-02-04T11:03:08Z
url: https://github.com/astral-sh/ruff/issues/12176
synced_at: 2026-01-12T15:54:51Z
```

# Show syntax error notification on manual invocation

---

_@dhruvmanila_

To quote Micha:

> Let's say you have a very long document and the syntax error is outside the visible area. It might now be unclear to users why the manually triggered action does _nothing_.
> 
> RustRover does show an error when running `rustfmt` failed because of a syntax error and I find this useful information.

I also think this is a useful indicator but might be difficult to achieve for a language server. This can be achieved at least for code actions using the [`CodeActionTriggerKind`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#codeActionTriggerKind) but for other requests like formatting and workspace command it's difficult without having full control over both the client and the server.

---

_Label `server` added by @dhruvmanila on 2024-07-04 04:34_

---

_Comment by @InSyncWithFoo on 2025-02-04 00:17_

For what it's worth, some [might prefer](https://github.com/InSyncWithFoo/ryecharm/issues/7) <em>not</em> to be notified.

---

_Comment by @dhruvmanila on 2025-02-04 11:02_

> For what it's worth, some [might prefer](https://github.com/InSyncWithFoo/ryecharm/issues/7) _not_ to be notified.

Yeah, good point. There's also the fact that most editors have a way to signal the users that there are errors in this document in a toolbar like UI even though the visible part of the document doesn't contain it.

This kinda of changes my opinion about this which is I'm not sure if it's possible or worth adding this. (marking it as "wish" for now)

---

_Label `wish` added by @dhruvmanila on 2025-02-04 11:02_

---
