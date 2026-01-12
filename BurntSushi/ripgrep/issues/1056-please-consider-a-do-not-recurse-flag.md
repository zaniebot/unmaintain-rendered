```yaml
number: 1056
title: "Please consider a \"do not recurse\" flag"
type: issue
state: closed
author: strindberg
labels:
  - wontfix
assignees: []
created_at: 2018-09-16T18:43:07Z
updated_at: 2024-07-05T18:53:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1056
synced_at: 2026-01-12T16:13:22Z
```

# Please consider a "do not recurse" flag

---

_@strindberg_

#### What version of ripgrep are you using?
ripgrep 0.10.0

Consider the following pipeline: `find . -iname '*properties' -type f | xargs rg java`. It's intended to search for the string 'java' in all property files. However, if there are no files matching find's filter, no file name is passed to rg, which then proceeds to look for the string 'java' in _all_ files in the current directory. This is confusing and hardly what the user intended.

If one is using GNU xargs, this can be helped with the '-r' option to xargs, which stops xargs from executing rg if no arguments are passed to it. This fixes the problem, but since it is a GNU extension to xargs, it cannot be used everywhere.

It would be great if ripgrep could have a "do not recurse" flag, or at least a "do nothing on no input" flag, so that this type of pipe could be used safely.

---

_Comment by @okdana on 2018-09-16 19:20_

If you're using GNU `xargs`, the `-r` option will prevent it from trying to execute the command when there's no input. (BSD `xargs` seem to behave that way by default.)

But, more generally, i think `rg --max-depth=0` should work?

---

_Comment by @strindberg on 2018-09-17 06:49_

Ah, yes. `rg --max-depth=0` does the trick.

---

_Closed by @strindberg on 2018-09-17 06:49_

---

_Label `question` added by @BurntSushi on 2018-09-17 11:27_

---

_Comment by @juhp on 2022-12-13 13:43_

I think a `--no-recurse` kind of option would still be useful (and compatible with grep):
there are times when one knows one only wants to search the files in the given directory.

Is there some path workaround?

---

_Comment by @BurntSushi on 2022-12-13 13:57_

@juhp 

> Is there some path workaround?

As mentioned above, `--max-depth=0`.

---

_Label `question` removed by @BurntSushi on 2022-12-13 13:57_

---

_Label `wontfix` added by @BurntSushi on 2022-12-13 13:57_

---

_Comment by @juhp on 2022-12-13 14:02_

Okay right, thank you - though it is a little long and not so memorable...

---

_Comment by @BurntSushi on 2022-12-13 14:09_

I think ripgrep has almost 100+ flags. If I added a new one for every little case like this, it would have _a lot_ more. Sorry.

I consider this use case rather niche. I have never wanted it personally, for example. So having to use an existing mechanism a little more creatively is perfectly fine IMO.

---

_Comment by @juhp on 2022-12-14 04:10_

Okay, thanks Andrew, that's fair and your call of course. :-)
(But it is the default behavior of grep (though it errors on directories :man_facepalming:), so calling it niche seems a little far-fetched;)

Maybe I will try an alias like `lrg` for this anyway.

---

_Comment by @BurntSushi on 2022-12-14 12:42_

I stand by what I said. I have never seen anyone rely on that behavior of grep and I have never done so myself. Remember what niche means: something that is specialized. I use the word as a proxy for "this is a somewhat uncommon use case, and adding better support for it would end up making the entire enterprise more complicated." Niche is not a value judgment on the importance or utility of what you're trying to do. It's an acknowledgment that it is uncommon.

We do not have data about what is common or not, so all we have to go on is perception and shared experience.

---

_Comment by @useranon350 on 2024-07-05 18:53_

I use the default behavior of grep all the time. It's probably about 40/60 on whether I want "process this finite list of files/stream" vs. "process this directory tree". It's also an obstacle to being able to make an ```alias grep="rg --no-recurse"``` entry in my .zshrc for compatibility with my muscle memory. (Assuming rg --no-recurse -r would lead to a recursive search).

---
