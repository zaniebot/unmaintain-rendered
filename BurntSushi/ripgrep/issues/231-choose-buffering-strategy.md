```yaml
number: 231
title: choose buffering strategy
type: issue
state: closed
author: jots
labels:
  - enhancement
  - question
assignees: []
created_at: 2016-11-11T23:54:03Z
updated_at: 2023-06-20T09:31:18Z
url: https://github.com/BurntSushi/ripgrep/issues/231
synced_at: 2026-01-12T16:13:21Z
```

# choose buffering strategy

---

_@jots_

Would like to do something like:

tail -f log.txt | rg term1 | rg term2 | etc.

and have data emitted right when data that would match hits the logfile.
Is this possible?



---

_Comment by @BurntSushi on 2016-11-12 14:04_

I don't believe it's possible today, but I agree it should be.

I think fixing this will involve looking at how other search tools behave.


---

_Label `enhancement` added by @BurntSushi on 2016-11-12 14:04_

---

_Label `question` added by @BurntSushi on 2016-11-12 14:04_

---

_Comment by @BurntSushi on 2016-11-12 14:05_

And we probably don't want to actually disable buffering, but perhaps switch to line buffering.


---

_Renamed from "disable buffering" to "choose buffering strategy" by @BurntSushi on 2016-11-12 14:05_

---

_Closed by @BurntSushi on 2016-11-20 20:01_

---

_Comment by @nkh on 2023-06-20 08:35_

@BurntSushi 

Hi, I'm a bit confused about the buffering, I have a command that generates a large output, I want to act as soon as a word is matched. I tee the output in a file so I can tail it.

command: rm xxx ; tail -F xxx | perl -pe '/NOT/ && exit'
result: stops within a second
discussion: tail does some buffering, in this case around 200 lines of the command's output

command: rm xxx ; tail -F xxx | rg . | perl -pe '/NOT/ && exit'
result: stops within 10 seconds
discussion: I guess some buffering is done by rg but as all the lines are matching it soon flushes.


command: rm xxx ; tail -F xxx | rg NOT | perl -pe '/NOT/ && exit'
result: never stops
discussion: tail -F never stops and I guess that rg's buffer is not flushed.

Did I misunderstand this ticket?


---

_Comment by @BurntSushi on 2023-06-20 09:07_

Please open new issues in the future. I don't understand why people keep replying to ancient issues that may or may not be related.

In any case, try the `--line-buffered` flag. (I'm on mobile, I believe that's what it's called. Look in the man page.)

If that doesn't work please *create a new issue with a complete reproduction.*

---

_Comment by @nkh on 2023-06-20 09:27_

it worked with --line-buffered

people reply to ancient issues because their issue is related (when they do spend the time)

now that --line-buffered is named people reading this ticket will have a clue. I found no --line-buffered in #240 nor in the patch.

---

_Comment by @BurntSushi on 2023-06-20 09:31_

... because it was added later.

I'm seriously considering locking all closed issues. Squirreling away discussion in old issues leads to a disorganized mess.

There's a reason why Discussions is enabled and it specifically encourages Q&A. Please use that next time.

---

_Locked by @ghost on 2023-06-20 09:31_

---
