```yaml
number: 185
title: invalid issue
type: issue
state: closed
author: lilianmoraru
labels:
  - question
assignees: []
created_at: 2016-10-17T14:10:35Z
updated_at: 2016-11-14T20:47:54Z
url: https://github.com/BurntSushi/ripgrep/issues/185
synced_at: 2026-01-12T18:23:11Z
```

# invalid issue

---

_@lilianmoraru_

I decided today to test the new `arm-unknown-linux-musleabihf` target on an ARM device with `ripgrep`.
The application works but it does not do `recursive` directory search.
My first guess would be that `musl` has a bug on ARM(`walkdir`), but I am not sure, so I open the issue here.

If I search in the folder where the file with the contents is, then it find it.
If I search in the parent folder, than it doesn't find it.

`rg --debug` doesn't give much info(note that the `terminfo` error did not appear on another device, but the bug I report was still present):

```
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'S',
        'p',
        'e',
        'e',
        'c',
        'h'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(Speech)], limit_size: 250, limit_class: 10 }
DEBUG:rg::out: error loading terminfo for coloring: could not find a terminfo entry for this terminal
```


---

_Comment by @BurntSushi on 2016-10-17 14:42_

@lilianmoraru I'm not able to test this, so unfortunately you're on your own. ARM isn't something I'm capable of supporting given the resources I have, but I'm willing to help in any way I can. In this case, I'd suggest trying to reproduce the bug against the `walkdir` crate if there is indeed a problem with something as simple as directory traversal.


---

_Label `question` added by @BurntSushi on 2016-10-17 22:02_

---

_Comment by @lilianmoraru on 2016-10-19 08:24_

The [walkdir example](https://github.com/BurntSushi/walkdir/blob/master/examples/walkdir.rs) works alright on the ARM device.


---

_Comment by @BurntSushi on 2016-10-22 00:28_

I don't think I read this issue closely enough before.

Could you please come up with a concrete and reproducible example? That way, we can test it on multiple platforms and rule out platform specific bugs. Maybe this is some other bug.


---

_Comment by @lilianmoraru on 2016-10-22 11:08_

This is not reproducible on PC/x86_64.
I had on the ARM device a systemd service that was starting a binary that contained in the name "Speech".
If I would go directly in `/lib/systemd/system/` where the file is, and run `rg "Speech"` it would find it.
If I would then go into `/lib/systemd` or `/lib` and run `rg "Speech"` it would instantly finish without returning anything.


---

_Comment by @BurntSushi on 2016-10-22 11:57_

Could you please try to come up with an example that uses mkdir, echo and cat in a fresh directory? As it stands right now, I have no idea how to reproduce your problem.


---

_Comment by @lilianmoraru on 2016-10-22 15:36_

I'll do better. I'll record an `asciinema`.
But this will be on Monday, when I get back to work...


---

_Comment by @BurntSushi on 2016-10-22 15:47_

Thanks, but I don't think a recording is necessary. What I'd like is a precise sequence of commands that I (or anyone else) can run to reproduce your problem. This means either creating the corpus yourself manually (which is ideal, since it will be small and easy to analyze) or using an existing corpus that we can both access easily (for example, a repository on Github). Your system's `systemd` directory wouldn't fall into that category, because it's setup by your system's package manager and isn't something I can easily reproduce.


---

_Comment by @lilianmoraru on 2016-10-22 16:14_

`mkdir -p test/1/2/ && echo "text" > test/1/2/file.txt && rg "text"` - should do it, but I have to test at work to confirm this.


---

_Comment by @BurntSushi on 2016-10-22 19:06_

@lilianmoraru Thanks. Please also include the output of running `strace rg "text"`. Thanks. :-)


---

_Renamed from "ripgrep does not search recursively on ARM target with musleabihf std" to "ripgrep does not follow symbolic links on ARM target with musleabihf std" by @lilianmoraru on 2016-11-14 18:09_

---

_Renamed from "ripgrep does not follow symbolic links on ARM target with musleabihf std" to "invalid issue" by @lilianmoraru on 2016-11-14 18:14_

---

_Comment by @lilianmoraru on 2016-11-14 18:15_

Ok, now it is clear why it was not working.
I was searching in `/lib/systemd` - on PC this folder has other folders and files but on that ARM device, it has only 1 symbolic link(which I was not aware at the time - didn't check).
I tried `rg -L`(also didn't know that you need to pass that flag but after identifying the issue and playing on PC with symbolic links, I realized that it had nothing to do with ARM specifically) and it worked alright.


---

_Closed by @lilianmoraru on 2016-11-14 18:15_

---

_Comment by @BurntSushi on 2016-11-14 18:21_

Yay!


---

_Comment by @BurntSushi on 2016-11-14 19:17_

@lilianmoraru Thank you so much for following up on this by the way. :-)


---

_Comment by @lilianmoraru on 2016-11-14 20:47_

@BurntSushi Yes, sorry it took so long.
I moved to a new project and was very busy for a while...


---
