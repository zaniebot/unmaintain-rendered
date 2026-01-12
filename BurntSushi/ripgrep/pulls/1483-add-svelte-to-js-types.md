```yaml
number: 1483
title: "Add *.svelte to js types"
type: pull_request
state: closed
author: zarkone
labels: []
assignees: []
base: master
head: patch-1
created_at: 2020-02-14T12:38:10Z
updated_at: 2020-12-02T14:21:51Z
url: https://github.com/BurntSushi/ripgrep/pull/1483
synced_at: 2026-01-12T18:23:13Z
```

# Add *.svelte to js types

---

_@zarkone_

filetype of https://svelte.dev/

---

_Comment by @BurntSushi on 2020-02-14 12:48_

Hmmm, I'm not so sure about this. I don't know anything about svelte, but looking at your link, svelte files look more like HTML files to me. In particular, the `js` type in ripgrep does _not_ contain HTML files, even though they can contain Javascript inside them.

Would others care to weigh in on this?

---

_Comment by @zarkone on 2020-02-14 15:56_

@BurntSushi well, svelte files looks pretty much the same as `*.vue` files, which mask is in `js` category

---

_Comment by @zarkone on 2020-02-14 16:02_

for me personally it is more convenient to have both `vue` and `svelte` in js, but it is arguable of course

---

_Comment by @BurntSushi on 2020-02-14 16:03_

@zarkone I think it'd be a useful exercise to evaluate this addition on its own. It could have been a mistake to add vue to js, I don't know. Are svelte (or vue) files _principally_ Javascript?

---

_Comment by @zarkone on 2020-02-14 17:40_

principally no, i think. This files contains html,css and js -- all mixed

---

_Comment by @BurntSushi on 2020-02-15 14:21_

All right, so if it's not principally Javascript, then I'm not quite sure I feel comfortable putting it under the `js` type.

I'd be happy to be overruled by this, but I think I'd want to see some consensus from relevant stakeholders.

---

_Closed by @BurntSushi on 2020-02-15 14:21_

---

_Branch deleted on 2020-12-02 14:21_

---
