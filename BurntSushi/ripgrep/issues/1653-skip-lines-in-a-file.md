```yaml
number: 1653
title: Skip lines in a file
type: issue
state: closed
author: majkinetor
labels:
  - enhancement
  - question
  - wontfix
assignees: []
created_at: 2020-08-14T08:27:45Z
updated_at: 2024-08-05T01:15:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1653
synced_at: 2026-01-12T16:13:24Z
```

# Skip lines in a file

---

_@majkinetor_

I use rg to display log errors on log files that can become quite big over time.  I do this in a loop or when log file changes. Is it possible to instruct rg to continue from the line where it finished in previous run ? 

As an example:

```
$out = rg <PATTERN> app.log
$LASTLINE = (get-from-result) OR (get-line-count-if-no-results)

rg --skip-lines $LASTLINE app.log 
```

I couldn't find anything like that in rg which implies that it must be done via external tool (such tail) which somewhat defeats the performance benefits of rg.  

---

_Comment by @BurntSushi on 2020-08-14 11:21_

Yes, you must do this with an external tool. I don't quite understand why you think this defeats the performance benefits of ripgrep though. I don't think ripgrep would be able to skip lines any faster than head or tail would.

The usual way I've done this sort of thing in the past is to do `tail -f logfile | rg foo`.

---

_Comment by @majkinetor on 2020-08-14 11:25_

> I don't think ripgrep would be able to skip lines any faster than head or tail would

It would certainly be  faster as it doesn't require any IPC and external process invocation.
It also doesn't require any external tool. On Windows for example there is no `tail` OTB, and `Get-Content <file> | Skip -First $lines` is noticeable  slow.

---

_Comment by @BurntSushi on 2020-08-14 11:46_

> It would certainly be faster as it doesn't require any IPC and external process invocation.

You might be literally correct, but I suppose I should have said, "meaningfully faster." And I don't agree that it would be.

> It also doesn't require any external tool. On Windows for example there is no tail OTB, and `Get-Content <file> | Skip -First $lines` is noticeable slow.

I don't think it's fair for ripgrep to take on the burden of missing or poor tooling in every environment. With that said, if this is easy to implement and maintain, then I might be inclined to take it on.

Could you write a detailed specification for the feature you want? Ideally, the specification would correspond to the documentation you'd like to see for this feature in ripgrep's `--help` output.

---

_Label `enhancement` added by @BurntSushi on 2020-08-14 11:46_

---

_Label `question` added by @BurntSushi on 2020-08-14 11:46_

---

_Comment by @majkinetor on 2020-08-14 11:53_

> I don't think it's fair for ripgrep to take on the burden of missing or poor tooling in every environment.

Yeah, I agree. And i like 'separation of concerns' I am just not sure if this concern is not part of the rg domain. For example C++ function `find` has `pos` argument - _The initial position from where the string search is to begin_  and that is basically the same in any other language. So if rg is to be used in looping scenarios (like above mentioned function is often used), it should probably have this parameter.

> Could you write a detailed specification for the feature you want? 

OK, I will do that in the near future.



---

_Comment by @krobelus on 2020-08-14 18:13_

Sounds a bit specific. Using `sed` or `awk` to skip those lines before piping into `rg` should be quite fast.

If your file gets really big then a `rg --skip-lines` would also  get slower linearly because it has to parse newlines to know where to start searching. I think GNU grep and ripgrep are so fast because they can skip looking at most characters depending on the pattern. When counting newlines they can't skip a single byte, so that option could even make `rg` slower.

If it really matters you can cache the file offset to directly continue where you left off for maximum efficiency:
```perl
perl -e '
    $offset = -f "app.log.offset" ? `cat "app.log.offset"` : 0; # read cached offset
    open IN, "<", "app.log" or die "open: $!";
    seek IN, $offset, 0;
    print while (<IN>);
    $offset = tell IN;
    system "echo $offset >app.log.offset" # remember offset for next run
' | rg PATTERN
```

I guess there could be a byte-offset option to ripgrep but I don't know if that's worth it.

---

_Comment by @BurntSushi on 2020-08-14 18:34_

>  I think GNU grep and ripgrep are so fast because they can skip looking at most characters depending on the pattern. When counting newlines they can't skip a single byte, so that option could even make rg slower.

Just a small note that this isn't really true any more. ripgrep is fast at searching primarily because of its vectorized routines. (The same is true of GNU grep.) And indeed, line counting is [also vectorized](https://github.com/llogiq/bytecount) and is quite fast. It's _probably_ faster than what sed does to be honest, but I don't know by how much or whether that makes worth re-creating that functionality in ripgrep.

Presumably if the OP doesn't have head or tail then they probably don't have sed or awk either. So that's part of the problem too I suppose.

---

_Comment by @majkinetor on 2020-08-15 08:00_

> And indeed, line counting is also vectorized and is quite fast.

Ah this is a good read. Thx

> rg --skip-lines would also get slower linearly because it has to parse newlines to know where to start searching.

rg already counts lines because it returns line number on each match. It will actually speed up rg in that case as it will not look for the pattern before adequate lines are reached.

> Presumably if the OP doesn't have head or tail then they probably don't have sed or awk either. So that's part of the problem too I suppose.

Yeah, if OSes universally came with performant line counting tools then we would probably not having this conversation. Given that main selling point of rg is speed, and that skipping content is more or less expected behavior I don't see it as too specific. 



---

_Comment by @BurntSushi on 2023-11-25 19:05_

I don't think I've changed my mind on this in the intervening years and it overall seems like it's out of scope for ripgrep.

---

_Closed by @BurntSushi on 2023-11-25 19:05_

---

_Label `wontfix` added by @BurntSushi on 2023-11-25 19:05_

---

_Comment by @Jcparkyn on 2024-08-05 01:15_

One thing not mentioned here that I've run into is that using `tail` means the byte offsets reported by ripgrep's `-b` flag are all relative to the starting point, not the start of the file. This obviously makes sense when reading from stdin, but it'd be much nicer if I could have byte offsets relative to the whole file so that I can use them on subsequent calls to `rg`. (yes, I can manually add them to the start offset, but it's annoying and error-prone).

As far as I can tell, this is only really feasible if `rg` had something like a `--skip-bytes` flag. This would probably need to disable the line number output for performance.

---
