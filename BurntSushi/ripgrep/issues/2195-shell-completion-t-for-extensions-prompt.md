```yaml
number: 2195
title: "shell completion '-t' for extensions prompt"
type: issue
state: closed
author: Freed-Wu
labels:
  - enhancement
  - help wanted
  - rollup
assignees: []
created_at: 2022-04-26T12:20:55Z
updated_at: 2023-07-08T22:53:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2195
synced_at: 2026-01-12T16:13:24Z
```

# shell completion '-t' for extensions prompt

---

_@Freed-Wu_

Now, the shell completion `-t`(e.g. zsh) is as following:

```
❯ rg -t<Tab>
file type
agda          cabal         csv           fidl          hbs           license       minified      po            robot         sv            typoscript    z
...
```

In the code, it truncate `:*` of the output of `rg --type-list`

```
❯ rg --type-list|head -n1
agda: *.agda, *.lagda
```

How about adding the extension information? Like this:

```
❯ rg -t<Tab>
file type
agda     *.agda,*.lagda
aidl     *.aidl
...
```

It should can be realized, because zsh use `(agda aidl)` to provide the completion without prompt information, and use `(agda\:*.agda,*.lagda aidl\:*.aidl)` to provide the completion with prompt information.

---

_Comment by @Freed-Wu on 2022-04-26 12:39_

Just like `fd -t<Tab>`

---

_Comment by @BurntSushi on 2022-04-26 12:47_

I think the current behavior is ideal. Otherwise it becomes way too noisy. If you want to see the extensions for each type, then I think needing to run `rg --type-list` separate is fine.

---

_Closed by @BurntSushi on 2022-04-26 12:47_

---

_Label `wontfix` added by @BurntSushi on 2022-04-26 12:47_

---

_Comment by @okdana on 2022-04-26 23:00_

zsh has a [standard style](https://zsh.sourceforge.io/Doc/Release/Completion-System.html#Standard-Styles) called `extra-verbose` that can be used for stuff like this. `_rg_types` could support it with a small modification, and then users who want the more elaborate completion results can turn them on in their shell profile with `zstyle`.

Maybe the visual/maintenance burden still isn't worth it, idk, but i could put a PR together if you did want to provide the option

---

_Comment by @BurntSushi on 2022-04-26 23:23_

Oh I would be okay with that! Good call.

---

_Reopened by @BurntSushi on 2022-04-26 23:23_

---

_Label `wontfix` removed by @BurntSushi on 2022-04-26 23:23_

---

_Label `enhancement` added by @BurntSushi on 2022-04-26 23:23_

---

_Label `help wanted` added by @BurntSushi on 2022-04-26 23:23_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 13:51_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
