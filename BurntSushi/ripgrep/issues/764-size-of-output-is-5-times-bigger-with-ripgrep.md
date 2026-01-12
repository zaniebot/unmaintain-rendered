```yaml
number: 764
title: Size of output is 5 times bigger with ripgrep than with thesilversearcher for color=always
type: issue
state: closed
author: edi9999
labels:
  - bug
assignees: []
created_at: 2018-01-29T09:54:46Z
updated_at: 2018-08-20T11:10:22Z
url: https://github.com/BurntSushi/ripgrep/issues/764
synced_at: 2026-01-12T16:13:22Z
```

# Size of output is 5 times bigger with ripgrep than with thesilversearcher for color=always

---

_@edi9999_

This is very similar to this issue : https://github.com/BurntSushi/ripgrep/issues/696.

But I think I found out what the underlying problem is.

rg --color always --colors line:fg:yellow --colors line:style:bold --colors path:fg:green --colors path:style:bold --colors match:fg:black --colors match:bg:yellow --colors match:style:nobold '.' --no-heading --line-number >/tmp/foo_rg 
ag '.' --color --nogroup --column  > /tmp/foo_ag 

When I then run : 

```
cat /tmp/foo_rg | less -R
# and 
cat /tmp/foo_ag | less -R
```

I see that the output colors are "similar", but the size of the files are very different : 

```
ls -lah /tmp/foo*
-rw-r--r-- 1 edgar edgar  22M Jan 29 10:48 /tmp/foo_ag
-rw-r--r-- 1 edgar edgar 109M Jan 29 10:48 /tmp/foo_rg
```

And the number of lines in each file is the same.

I have taken a screenshot of the same lines of each efile for api/check.py: 
On top is the ripgrep output, On the bottom the ag output : 

![selection_001](https://user-images.githubusercontent.com/2071336/35504217-c3a27dd2-04e2-11e8-9a80-98e99f6e0e4d.png)

As you can see, ripgrep has a huge amount of escape sequences that ag doesn't have.

---

_Renamed from "Size of output is 10 times bigger with ripgrep than with thesilversearcher for color=always" to "Size of output is 5 times bigger with ripgrep than with thesilversearcher for color=always" by @edi9999 on 2018-01-29 09:54_

---

_Comment by @okdana on 2018-01-29 11:23_

That doesn't seem ideal, but it's only relevant in cases where `rg` has been asked to match individual characters — e.g., patterns like `.` and `\w`. `.*` for example does not have that problem (though `rg` is still a *little* more noisy with the format stuff than `ag` because it doesn't combine sequences).

I don't use fzf but it doesn't seem likely that the original reporter from #696 would have been searching for a pattern like `.` (right?). Maybe there could be some interplay between the way `rg` outputs sequences and whatever fzf does with them, though, idk

---

_Comment by @BurntSushi on 2018-01-29 11:53_

This is indeed interesting! I agree that we should probably fix this. It might make sense to delay fixing this until the printer is refactored though.

---

_Comment by @edi9999 on 2018-01-29 12:59_

Yes, the usecase in fzf is really to query with `.` , this way, we actually have all lines which we then filter throuhg fzf (which is an interactive grep). I find it good to be able to search for `.` because this makes the search more interactive, if for example I type `seacr` instead of `searc`, I will see immediately that there are no more results.



---

_Comment by @okdana on 2018-01-29 13:04_

Ohh. That makes sense then

---

_Label `bug` added by @BurntSushi on 2018-02-01 22:14_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
