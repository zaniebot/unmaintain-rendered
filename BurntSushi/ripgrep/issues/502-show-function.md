```yaml
number: 502
title: "--show-function"
type: issue
state: closed
author: anntzer
labels:
  - question
  - icebox
assignees: []
created_at: 2017-06-04T02:06:50Z
updated_at: 2021-04-19T19:19:53Z
url: https://github.com/BurntSushi/ripgrep/issues/502
synced_at: 2026-01-12T16:13:22Z
```

# --show-function

---

_@anntzer_

It would be nice if rg gained the ability to show the function containing the current match, similarly to `git grep --show-function`.

---

_Comment by @BurntSushi on 2017-06-04 02:48_

How many languages does git grep support? How many should ripgrep support? Can you sketch out what an implementation would look like and what kind of maintenance would be required? Should it stop at functions? What about classes? Methods? Modules? Namespaces? Comments?

---

_Comment by @anntzer on 2017-06-04 03:04_

Default hunk headers supported by git are defined at the top of https://github.com/git/git/blob/3bc53220cb2dcf709f7a027a3f526befd021d858/userdiff.c; I count 17 entries and it seems fair to support the same (by copy-pasting the regexes).  It is obviously not perfect (this is difficult to do in absence of fully-fledged parsing, plus who knows what (e.g.) the C preprocessor could be doing), but still good enough ("as good as git formats its hunk headers") to be helpful.

The idea is simply to provide an additional regex ("xfuncname") that defines entry into a new (toplevel) function.  Whenever a match to the user-provided correct regex is made, move backwards to find the closest line that matches "xfuncname" and output it first as the context (in fact it is probably more efficient to check whether each line matches xfuncname while looking for the user-provided regex and just remember the current toplevel function).

You could of course make xfuncname user-configurable via a command-line switch (... per filetype) but that doesn't look like a priority.

(The maintenance would simply be to copy-paste the regexes from the git source whenever they update theirs.)

---

_Comment by @BurntSushi on 2017-06-05 16:45_

> Whenever a match to the user-provided correct regex is made, move backwards

How far backwards? How does that even work? ripgrep doesn't necessarily contain the entire contents of a file in memory, so it can't just arbitrarily move backwards. I suppose you could put a hard cap on it, but what should that cap be? (There is already logic for the context handling to keep the previous N lines in memory.)

> in fact it is probably more efficient to check whether each line matches xfuncname while looking for the user-provided regex and just remember the current toplevel function

I suspect not, and I think you're assuming that ripgrep searches each line individually, but it doesn't: http://blog.burntsushi.net/ripgrep/#mechanics

> but still good enough

The problem is that this is really easy for a single end user to justify, but as a maintainer, I *must* be incredibly skeptical of these arguments. What's "good enough" for you might be "useless" to many others. Therefore, if we add a "good enough" feature, it's dooming the project to gather bug reports about how the feature doesn't actually do what's advertised in some cases.

I'm not saying that we shouldn't add this feature because of this, but there needs to be a much stronger representation of the maintenance cost here. "simply copy-paste the regexes" is a gross misrepresentation IMO. Not only for the reason mentioned above, but also because *as soon* as we copy the support from `git grep`, there will be feature requests for languages X, Y and Z that `git grep` does not support. Do you think telling those users "too bad, we only support what `git grep` provides because we can't write the regexes ourselves" would be appropriate? I don't.

---

_Comment by @anntzer on 2017-06-05 17:36_

> How far backwards? How does that even work? ripgrep doesn't necessarily contain the entire contents of a file in memory, so it can't just arbitrarily move backwards. I suppose you could put a hard cap on it, but what should that cap be? (There is already logic for the context handling to keep the previous N lines in memory.)
> in fact it is probably more efficient to check whether each line matches xfuncname while looking for the user-provided regex and just remember the current toplevel function
I suspect not, and I think you're assuming that ripgrep searches each line individually, but it doesn't: http://blog.burntsushi.net/ripgrep/#mechanics

My previosu description was just to give the idea of the algorithm; I assume that the actual implementation would be ($user is the user-specified regex, $xfuncname is the xfuncname for the current function type (as determined from the file extension, as rg currently does))
```
currentfunc = null
for each match of "$user|$xfuncname" # note that we don't search for $user itself
    if it matches $xfuncname
        currentfunc = currentmatch
    if it matches $user
        write currentfunc AND currentmatch to the output buffer
```
(obviously this is more expensive than just searching for $user but that seems fair, the user *did* ask for more info)

> The problem is that this is really easy for a single end user to justify, but as a maintainer, I must be incredibly skeptical of these arguments. What's "good enough" for you might be "useless" to many others. Therefore, if we add a "good enough" feature, it's dooming the project to gather bug reports about how the feature doesn't actually do what's advertised in some cases.
> I'm not saying that we shouldn't add this feature because of this, but there needs to be a much stronger representation of the maintenance cost here. "simply copy-paste the regexes" is a gross misrepresentation IMO. Not only for the reason mentioned above, but also because as soon as we copy the support from git grep, there will be feature requests for languages X, Y and Z that git grep does not support. Do you think telling those users "too bad, we only support what git grep provides because we can't write the regexes ourselves" would be appropriate? I don't.

A tool as venerable as GNU diff has chosen to provide the `--show-c-function` and `--show-function-line=RE` flags, i.e provide a "good enough" regex for C and just let the user specify any other xfuncname they may want to use.  If you don't want to take any risks, you could even choose to not provide any xfuncname by default, only the tooling, and simply suggest that the user defines an alias similarly to how one would use --type-add, e.g.

```
alias rg='rg --add-xfuncname c "$c_regex" --add-xfuncname rust "$rust_xfuncname"'
```

---

_Comment by @zachriggle on 2017-11-09 21:25_

Filed a dupe of this request in #669 with performance metrics of `git grep` vs `git grep -p`

---

_Label `icebox` added by @BurntSushi on 2017-11-09 21:28_

---

_Label `question` added by @BurntSushi on 2017-11-09 21:28_

---

_Comment by @zachriggle on 2017-11-09 21:49_

Per #502, I've got a [simple wrapper script](https://github.com/zachriggle/config/blob/master/bin/git-rg) that allows you to `git rg` which is faster than either `git-grep` or `rg` by itself, *and* allows `ack`-like styling.

Performance, using the Linux kernel as a test bed (fastest of 10 runs taken).

```
git grep kasan_unpoison_remaining_stack  0.52s user 1.74s system 192% cpu 1.170 total
rg kasan_unpoison_remaining_stack  0.76s user 1.32s system 594% cpu 0.351 total
git rg kasan_unpoison_remaining_stack  0.66s user 1.31s system 612% cpu 0.322 total
```



---

_Comment by @Ngo-The-Trung on 2018-04-06 20:27_

Is this still needed?

---

_Comment by @anntzer on 2018-04-06 22:42_

Yes, although there's only a point in leaving this open if @BurntSushi considers that there's a chance of this ever making it to rg itself.  If he doesn't want it, well, he should close this :-)

---

_Comment by @BurntSushi on 2019-04-03 13:26_

Reflecting back on this, I think it's unlikely this will get into ripgrep. 

---

_Closed by @BurntSushi on 2019-04-03 13:26_

---

_Comment by @saumyajyoti on 2021-04-18 14:13_

Maybe a wrapper script with ctags /treesitter search for given line whould solve this?

---

_Comment by @zachriggle on 2021-04-19 19:19_

@saumyajyoti Could you elaborate on how that might look?

---
