```yaml
number: 2233
title: "type-add: Support for regexpes"
type: issue
state: closed
author: warpspin
labels:
  - wontfix
assignees: []
created_at: 2022-06-11T09:02:57Z
updated_at: 2022-06-11T10:50:33Z
url: https://github.com/BurntSushi/ripgrep/issues/2233
synced_at: 2026-01-12T16:13:24Z
```

# type-add: Support for regexpes

---

_@warpspin_

I wished --type-add would also support regexpes in addition to globs.

In my current use case, I'd usually like to filter all SQL dumps (*.sql) but still include SQL migration files (*.sql file but starting with a number, so basically [^0-9].*\.sql as a regexp, so I'd love to redefine the sql type to something excluding migrations. But I'm pretty sure more use cases come up once regexpes would be supported.


---

_Comment by @BurntSushi on 2022-06-11 10:50_

Sorry, but the file filtering and type matching logic is staying with globs for the foreseeable future.

It sounds like you can achieve what you want by adding multiple file types and filtering that way.

---

_Closed by @BurntSushi on 2022-06-11 10:50_

---

_Label `wontfix` added by @BurntSushi on 2022-06-11 10:50_

---
