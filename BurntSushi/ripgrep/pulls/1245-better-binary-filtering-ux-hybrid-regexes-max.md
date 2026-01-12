```yaml
number: 1245
title: better binary filtering UX + hybrid regexes + max column preview + and more
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/stuff
created_at: 2019-04-14T21:45:01Z
updated_at: 2019-04-15T11:01:37Z
url: https://github.com/BurntSushi/ripgrep/pull/1245
synced_at: 2026-01-12T18:23:13Z
```

# better binary filtering UX + hybrid regexes + max column preview + and more

---

_@BurntSushi_

This PR fixes several bugs and adds a few new features that I want to get into the next release. Notably, this prepares the next release to be `ripgrep 11.0.0` and _not_ `ripgrep 0.11.0`.

---

_Review comment by @okdana on `complete/_rg`:273 on 2019-04-14 21:59_

These two should be made exclusive with each other, probably by breaking them out into a group with `+ '(max-column-preview)'` or similar

Also, i would suggest the description `show preview for long lines (with -M)` — that matches the wording for similar options, and might be a little clearer

---

_@okdana reviewed on 2019-04-14 21:59_

---

_Review comment by @okdana on `complete/_rg`:185 on 2019-04-14 22:04_

If i'm understanding correctly (that it has basically the same usage semantics as `--version`), this belongs under the `+ '(exclusive)'` group

---

_@okdana reviewed on 2019-04-14 22:04_

---

_@okdana reviewed on 2019-04-14 22:08_

---

_Review comment by @okdana on `complete/_rg`:48 on 2019-04-14 22:08_

Kind of a nit-pick, but describing the affected functionality differently between the enable and disable options feels a little odd (and inconsistent with other options)

---

_Comment by @okdana on 2019-04-14 22:14_

`--max-column-preview` works really nicely, thank you for going to the trouble of implementing it (and the other stuff too, obv)

---

_Comment by @BurntSushi on 2019-04-14 23:11_

@okdana Thanks very much for your review. :-) I think everything you pointed out should be fixed! And thanks for testing the preview functionality. I'm near certain that there will be bugs there because correctly testing the color spans was more work than I wanted to do.

---

_Comment by @BurntSushi on 2019-04-14 23:14_

@okdana If all goes well with the max column preview, I'm contemplating turning it on by default in ripgrep 12. You were completely right about it being the "expected" behavior here. (I've put it in my .ripgreprc.)

---

_Merged by @BurntSushi on 2019-04-14 23:29_

---

_Closed by @BurntSushi on 2019-04-14 23:29_

---

_Branch deleted on 2019-04-14 23:30_

---

_Comment by @okdana on 2019-04-15 00:14_

Oh, actually, on reflection, do you think it should be `--max-columns-preview` (with an `s`) instead? That was the name you originally suggested in #1078, and it seems to make sense since it enables a `preview` for `--max-columns`. I'm sure you put some more thought into the name, but i thought i'd bring it up now, just in case, before it's completely set in stone

---

_Comment by @BurntSushi on 2019-04-15 00:50_

I don't think I thought about it too much. I just felt like `--max-column-preview` sounded better in my head. Do you like `--max-columns-preview` better?

---

_Comment by @okdana on 2019-04-15 01:17_

For what it's worth, yeah, i think so

---

_Comment by @BurntSushi on 2019-04-15 11:01_

@okdana All righty, SGTM. Done!

---
