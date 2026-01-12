```yaml
number: 2009
title: moving .ts from typoscript to TypeScript
type: pull_request
state: closed
author: Mellbourn
labels:
  - rollup
assignees: []
base: master
head: move-ts-extension-to-typescript
created_at: 2021-09-29T21:01:09Z
updated_at: 2023-07-08T22:53:07Z
url: https://github.com/BurntSushi/ripgrep/pull/2009
synced_at: 2026-01-12T18:23:14Z
```

# moving .ts from typoscript to TypeScript

---

_@Mellbourn_

The TypeScript language is in the top ten of the [most popular programming languages](https://pypl.github.io/PYPL.html).

Typoscript is a custom language for the TYPO3 CMS, that is far less used.

I think that the `.ts` extension should belong to the `typescript` type in ripgrep, not typoscript.


---

_Comment by @BurntSushi on 2021-09-29 22:04_

The `ts` file type already exists. That's for typescript.

File types don't own extensions. Multiple file types may have the same globs.

---

_Comment by @Mellbourn on 2021-09-29 22:21_

Right you are. It was a bit confusing that I could start to type `typ` in the shell and the completion system suggested `typoscript`, which I initially thought was a typo :D

---

_Comment by @matkoniecz on 2021-11-22 14:56_

So 

```
    ("typescript", &["*.ts"]),
    ("typoscript", &["*.ts", "*.typoscript"]),
```

would be better, right?

---

_Comment by @BurntSushi on 2023-07-08 17:35_

This is wrong. Firstly, if `ts` and `typescript` are referring to the same thing, then they should have the same globs. This PR does not have that. Secondly, this PR removes a glob from `typoscript` without justifying it.

---

_Label `rollup` added by @BurntSushi on 2023-07-08 17:37_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
