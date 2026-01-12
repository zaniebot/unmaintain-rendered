```yaml
number: 528
title: Remove all inline attributes
type: pull_request
state: closed
author: vegai
labels: []
assignees: []
base: master
head: remove_inlines
created_at: 2017-06-26T07:28:27Z
updated_at: 2017-07-23T19:12:33Z
url: https://github.com/BurntSushi/ripgrep/pull/528
synced_at: 2026-01-12T18:23:13Z
```

# Remove all inline attributes

---

_@vegai_

Inline attributes don't seem to have any effect on performance on my own tests. However, I was unable to properly verify this, as the benchsuite program did not quite work on my box.

Feel free to kill this PR if you know I'm not right :)

---

_Comment by @BurntSushi on 2017-06-26 10:53_

Undoubtedly, some of these probably should be removed. But if you aren't seeing any difference for any of them, then that makes me immediately skeptical. There needs to be a more thorough analysis, because I did put them there for a reason. Namely, at some point in time, I did notice a performance difference.

---

_Comment by @vegai on 2017-06-27 07:14_

Would it be possible that rustc has improved in a way that makes the explicit inlines obsolete? But I agree that my own benchmarks were primitive and probably did not stress the relevant parts enough.

I'd be happy to run the benchsuite against this version and a regular release version, but I wasn't able to on Arch Linux at least.

---

_Comment by @BurntSushi on 2017-06-27 11:59_

Out of curiosity, what problem are you trying to solve here? Why do you want to remove these inline attributes?

> Would it be possible

Anything is possible. :-)

Note that some of these inline attributes will only impact certain invocations of ripgrep. For example, `src/decoder.rs` is only used when decoding UTF-16, which isn't tested by `benchsuite`.

> I'd be happy to run the benchsuite against this version and a regular release version, but I wasn't able to on Arch Linux at least.

I use Archlinux. There's an example here: http://blog.burntsushi.net/ripgrep/#benchmark-runner --- If you can't get it to work, then please file an issue and we can figure it out. Please include more details than "it doesn't work."

---

_Comment by @DoumanAsh on 2017-07-21 05:26_

I could test your PR on windows i suppose.
I'm not really fond on doing manual suggestions to compiler, but sometimes it might be necessary and there is nothing wrong with it.
After all you have inline attributes for a reason.

---

_Comment by @BurntSushi on 2017-07-21 10:47_

The only way I'd merge this PR is if there is a careful and thorough analysis of each `inline` annotation. That's what I would do. I think it would be better if we tackled it one at a time, so I'm going to close this for now.

---

_Closed by @BurntSushi on 2017-07-21 10:47_

---

_Comment by @vegai on 2017-07-23 19:12_

Yeah, agreed.

---

_Branch deleted on 2017-07-23 19:12_

---
