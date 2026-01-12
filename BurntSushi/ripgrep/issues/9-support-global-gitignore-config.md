```yaml
number: 9
title: support global gitignore config
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2016-09-21T12:30:00Z
updated_at: 2016-10-30T01:21:56Z
url: https://github.com/BurntSushi/ripgrep/issues/9
synced_at: 2026-01-12T18:23:11Z
```

# support global gitignore config

---

_@BurntSushi_

e.g., `$HOME/.config/git/ignore` or `$XDG_CONFIG_HOME/git/ignore` or whatever file is specified by the configuration variable `core.excludesFile`. Another source might be `$GIT_DIR/info/exclude`.


---

_Label `bug` added by @BurntSushi on 2016-09-21 12:30_

---

_Comment by @BurntSushi on 2016-09-22 11:45_

While supporting `core.excludesFile` or a global `.gitignore` seems easy enough, support `$GIT_DIR/info/exclude` is actually a little tricky. There are two issues:
1. Firstly, it's an additional file to stat for in _every_ directory we descend. That list of files is currently at two (`.rgignore` and `.gitignore`), and I know we'd like to bump it to three so we can include `.hgignore` too. Adding `$GIT_DIR/info/exclude` would bring this list to 4 entries. Owch.
2. A `$GIT_DIR/info/exclude` file seems to have precedence strictly lower than _all_ .gitignore files in the current directory all the way up to the parent directory. (See `man gitignore`). This means we need to: 1) Check all `.gitignore` files, 2) then check all `$GIT_DIR/info/exclude` files and finally 3) check the global gitignore config.

We should at least support `core.excludesFile`. Supporting `$GIT_DIR/info/exclude` seems like it might cost us too much for little gain. (My impression is that it isn't widely used.)


---

_Comment by @Deewiant on 2016-09-24 07:41_

My 2¢: I've used `$GIT_DIR/info/exclude` in practically every repo I've worked on for a significant period of time, whereas I've never used `core.excludesFile`, so for me the priorities there would be exactly reversed.

One alternative solution might be to have a parameter or separate tool to auto-generate one or more `.ignore` files based on these sources that are otherwise trickier to handle. The usefulness of this is predicated on people not committing their `.ignore` files. I'm not sure how likely that is...


---

_Comment by @vi on 2016-09-26 01:20_

Maybe there should be an option for specifying additional (or replacement) ignore files manually?


---

_Comment by @BurntSushi on 2016-09-26 01:43_

@vi That's always an option, but it's not particularly satisfying.


---

_Comment by @rjayatilleka on 2016-09-27 23:22_

I also think `$GIT_DIR/info/exclude` would be more useful than `core.excludesFile`. But if there is a worry about performance, I think the flag for specifying extra ignore files is best. It can always be aliased or put in a short function.


---

_Comment by @Julian on 2016-10-10 11:38_

(Just for some counterbalance -- I'm certainly in the bucket of people you originally mentioned. `$XDG_CONFIG_HOME/git/ignore` is way way way more important for me.)


---

_Comment by @john-kurkowski on 2016-10-10 18:21_

`core.excludesFile` for me


---

_Comment by @rjayatilleka on 2016-10-20 21:51_

Actually, changed my mind. `core.excludesFile` for me too. How high on the priority list is this feature btw?


---

_Comment by @BurntSushi on 2016-10-25 18:19_

It's being worked on.

On Oct 20, 2016 5:51 PM, "Ramith Jayatilleka" notifications@github.com
wrote:

> Actually, changed my mind. core.excludesFile for me too. How high on the
> priority list is this feature btw?
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/9#issuecomment-255238570,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34puYJ9ir4rQPWTT_kcOTPAHu0mlWks5q1-JegaJpZM4KCvdR
> .


---

_Closed by @BurntSushi on 2016-10-30 01:19_

---

_Comment by @BurntSushi on 2016-10-30 01:21_

Fixed in #202.


---
