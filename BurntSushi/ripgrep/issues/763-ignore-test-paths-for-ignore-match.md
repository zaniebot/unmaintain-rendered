```yaml
number: 763
title: "ignore: test paths for ignore::Match"
type: issue
state: closed
author: dns2utf8
labels: []
assignees: []
created_at: 2018-01-28T23:28:35Z
updated_at: 2018-01-31T02:00:46Z
url: https://github.com/BurntSushi/ripgrep/issues/763
synced_at: 2026-01-12T16:13:22Z
```

# ignore: test paths for ignore::Match

---

_@dns2utf8_

I would like to watch all the files of a source tree.
Currently I setup the file watches for each file individually and reset the whole thing if I notice a new folder.

Is it currently possible to match a Path against a stored object? My guess is, probably not the walker?

Best,
Stefan

---

_Comment by @BurntSushi on 2018-01-28 23:32_

There is nothing out of the box in the `ignore` crate that will do this for you, no. But the building blocks are there.

---

_Comment by @dns2utf8 on 2018-01-28 23:43_

So can I build a matcher outside of the ignore crate or would I have to extend it?

---

_Comment by @BurntSushi on 2018-01-29 00:10_

I don't understand your question.

---

_Comment by @dns2utf8 on 2018-01-29 00:15_

Let me try again: In your opinion, would you try to implement this feature inside the `ignore` crate? Or can I do it with the public interface?

---

_Comment by @BurntSushi on 2018-01-29 01:08_

@dns2utf8 I think there is a place for it in the `ignore` crate, but it should be doable using the existing public API of `ignore`. That is, you shouldn't need any internals to build it. Note that this is not an easy thing to get right!

---

_Comment by @BurntSushi on 2018-01-31 01:57_

I am going to close this issue because I personally don't have any plans to work on this, and I'm not sure I even have the bandwidth to mentor it. @dns2utf8 If you want to ask more questions though, then doing it in this issue is fine and I'd be open to re-opening it if someone really wanted to take this on.

---

_Closed by @BurntSushi on 2018-01-31 02:00_

---
