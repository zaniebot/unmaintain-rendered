```yaml
number: 989
title: "ripgrep: add --pre flag"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/generic-decode
created_at: 2018-07-21T20:19:46Z
updated_at: 2018-07-21T21:44:42Z
url: https://github.com/BurntSushi/ripgrep/pull/989
synced_at: 2026-01-12T18:23:13Z
```

# ripgrep: add --pre flag

---

_@BurntSushi_

The preprocessor flag accepts a command program and executes this
program for every input file that is searched. Instead of searching the
file directly, ripgrep will instead search the stdout contents of the
program.

Closes #978, Closes #981

This PR is based off of @c-blake's work in #981. See [this comment](https://github.com/BurntSushi/ripgrep/pull/981#issuecomment-406821212) for a description of the differences.

cc @okdana 

---

_Comment by @c-blake on 2018-07-21 21:01_

Replying to the differences comment here.  I've been testing this as my primary grep ever since I posted my pull request.  I haven't run into many issues.

For ripgrep's ``stdin`` you don't really want/need a preprocessor.  There's only one of them.  You can probably just insert any preprocessing stage you want in the pipeline before ripgrep in that case, but yeah it should be noted.

In my testing the preprocessor could read stdin already.  So, I left that attach-y code in but commented out.

There is one gotcha with regard to ``stderr``/preprocessor log message handling - and this goes for the decompressor, too.  If the decompressor or preprocessor fills up the ``stderr`` pipe buffer to the point where a write blocks then ripgrep hangs forever.  On Linux this is about 64KB of writes.  That is more of a gotcha with some chatty programs like pdftotext that might spit out gobs of "warnings" but not really fail than for programs like decompressors, but other OSes might have smaller buffers.  What I do is just redirect ``stderr`` in my preprocessor to keep ripgrep's buffer from ever getting anything.  It might also make sense to just never read stderr or send it to oblivion and have preprocessors send them to a file first if user's want to keep them.

I knew that man page formatting was a disaster.  I figured it would be easy to fix if you liked this.  Thanks!

I think just a long ``--pre`` is reasonable.  If you wanted, we could also do ``--input-preproc`` with ``-I`` as a short spelling.  Most of the obvious names are already taken like, --[fF]ilter and so on.  Like 34 of the 52 characters are taken already.  If someone has a script wrapper that turns it on, and then wants to turn it off on their typed command line they would only have to add ``-I=``, but two more letters without a shift key feels about the same.  Anyway, just a long option seems fine if the long option is fairly short like ``--pre``.  I'm mostly just happy you're cool with this new feature, and I won't have to maintain some patch forever or write my own with some hypothetical libripgrep.

User-specified commands/programs fail to be found/installed all the time.  Somebody in Rust-land should do a better job with that someday, but I don't think we need to here.  Anyway, I like all your changes.  No real pushback from me.

---

_Comment by @BurntSushi on 2018-07-21 21:25_

@c-blake Excellent, thanks for the feedback! Your note about stderr and deadlock is a good reminder, thank you. I filed #990 for that, and hopefully we can pick a forward course.

Otherwise, I'm glad you're happy with `--pre`, so let's run with that!

---

_Merged by @BurntSushi on 2018-07-21 21:25_

---

_Closed by @BurntSushi on 2018-07-21 21:25_

---

_Branch deleted on 2018-07-21 21:25_

---

_Comment by @BurntSushi on 2018-07-21 21:28_

> In my testing the preprocessor could read stdin already.

This is very weird. The first thing I tried was actually your example script in the `--help` docs, and it hung when attempting to search a file that was neither a PDF nor compressed. Namely, this caused it to hit the fallback `cat` case, which just blocked on stdin. This seemed to make sense from my reading of the code as well, since ripgrep didn't have anything on stdin, so reading from it isn't going to get you anywhere, and it certainly won't contain the contents of the file being searched.

---

_Comment by @c-blake on 2018-07-21 21:30_

Oops.  Yeah, you probably want to put the filename after "cat".  You don't for some things like gunzip where if you give it a filename it will replace your compressed file with a decompressed version.

---

_Comment by @c-blake on 2018-07-21 21:43_

Oh, and of course, I would prefer it if we did attach each new input file to the ``stdin`` of the preprocessor.  I just didn't get that far, but that kind of redundancy (both ``stdin`` and a filename) would be better, IMO.  I could swear I tested that and it was working, but maybe I'm misremembering.  It's been a while.

---

_Comment by @BurntSushi on 2018-07-21 21:44_

@c-blake This PR does that. :-)

---
