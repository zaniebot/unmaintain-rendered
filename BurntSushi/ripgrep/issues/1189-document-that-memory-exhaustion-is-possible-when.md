```yaml
number: 1189
title: document that memory exhaustion is possible when using parallelism
type: issue
state: closed
author: domenukk
labels:
  - doc
assignees: []
created_at: 2019-02-07T22:12:34Z
updated_at: 2019-04-14T23:29:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1189
synced_at: 2026-01-12T16:13:23Z
```

# document that memory exhaustion is possible when using parallelism

---

_@domenukk_

#### What version of ripgrep are you using?
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Precompiled msvc binary for Windows-x64

#### What operating system are you using ripgrep on?

Windows 7, some current pach level

#### Describe your question, feature request, or bug.

If I redirect the ripgrep output to a file on windows, the memory usage of `rg.exe` increases slowly but steady, probably with each found element.
After a few GB, `rg.exe` then crashes with a segmentation fault.
Inside `procmon`, I see the results seem to only be flushed to the file after the crash.
A workaround is to force single threading with `-j1`. This seems to be directly related to [this old issue](https://github.com/BurntSushi/ripgrep/issues/4) 

Or are the reads and hits simply too fast to be written to disk if multi threaded?

#### If this is a bug, what are the steps to reproduce the behavior?

Trying to extract all mail addresses from a large current password leak with 130k files, 8k folders and 1.62TB in total (with different filessizes) crashes rg.

Inside git bash, run:
```
$ ./rg.exe -uu --no-filename -o '[a-zA-Z0-9.%&’*+/=?^_`{}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*' ./Passwords/ > all_mails.txt
```
For obvious reasons, I cannot include the corpus here.

#### If this is a bug, what is the actual behavior?

```
$ ./rg.exe --debug -uu --no-filename -o '[a-zA-Z0-9.%&’*+/=?^_`{}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*' ./Passwords/ > all_mails.txt
DEBUG|grep_regex::literal|grep-regex\src\literal.rs:110: required literal found: "@"
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
memory allocation of 5795684597665262959 bytes failedSegmentation fault
```

#### If this is a bug, what is the expected behavior?

The user should be able to `rg` multi-threaded on any kind of large dataset.
Maybe a synchronization point when the buffer gets too big or the choice to disable caching (I don't need the output in order, for example) could be options.

---

_Renamed from "Buffers never seem to be flushed on multithreading" to "Buffers don't seem to be flushed (quickly enough?) on multithreading" by @domenukk on 2019-02-07 22:14_

---

_Comment by @BurntSushi on 2019-02-07 22:26_

This happens when the search results for a _single_ file exceed the amount of memory available. This is fundamentally a result of combining parallelism and the requirement that the output of each file is not interleaved. It sounds like you said you'd be OK with the output from different files being interleaved, but I'm not keen on adding this option to ripgrep. Instead, you have a few work-arounds available to you, assuming we've correctly diagnosed the problem:

* Use parallelism via `xargs` or some other tool, just like you would with standard grep. e.g., `find ./ -print0 | xargs -0 rg foo`.
* Use ripgrep's `-m/--max-count` flag to limit the number of matching lines it prints per file. This could be quite undesirable since it makes it hard to know whether a match was missed or not.

---

_Label `question` added by @BurntSushi on 2019-02-07 22:26_

---

_Comment by @domenukk on 2019-02-07 23:33_

Thank you for your quick response! Awesome!
Indeed there is a file with 80 gb which might match your description of a single large file pretty well.
I guess I'll try my luck with the workarounds then and see what happens.

That being said, it could still be handy to have a "single file, multiple workers, as fast as possible, interleave if you must" grep mode for these rare occasions - as long as a single result always arrives in one piece in the output. But the occasion might be rare enough.

And a different idea: print which files it choked on when it crashed (just to have fewer github support issues)

Keep up the good work ;)

(oh off-topic, that it tries to allocate 5 exabyte seems like a fun overflow somewhere below the rust layer...)

---

_Comment by @BurntSushi on 2019-02-07 23:42_

Rust's standard library doesn't allow one to easily recover from allocation failure, so it's impractical to print the file on which this choked. Of course, I agree it would be nice to improve failure modes, but I think we're stuck here.

Your proposed option is undoubtedly handy, but it's not a good fit since it's an extraordinarily niche feature with some simple work-arounds.

I'm mark this ticket as a doc bug and find a place to add a note about memory exhaustion being possible when parallelism is enabled, and document the work-arounds.

---

_Label `doc` added by @BurntSushi on 2019-02-07 23:42_

---

_Label `question` removed by @BurntSushi on 2019-02-07 23:42_

---

_Renamed from "Buffers don't seem to be flushed (quickly enough?) on multithreading" to "document that memory exhaustion is possible when using parallelism" by @BurntSushi on 2019-02-07 23:43_

---

_Comment by @BurntSushi on 2019-02-07 23:44_

Note that memory exhaustion is not unique to ripgrep, or even parallelism. Both grep and ripgrep are subject to memory exhaustion errors when searching files that have a single line that exceeds available memory. For example, something like `grep ZQZQZQZQZQ /dev/sda -c -a` is one way I've caused this to happen fairly reliably.

---

_Closed by @BurntSushi on 2019-04-14 23:29_

---
