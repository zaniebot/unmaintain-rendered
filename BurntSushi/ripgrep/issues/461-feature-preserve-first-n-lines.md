```yaml
number: 461
title: "feature: preserve first N lines"
type: issue
state: closed
author: eminence
labels: []
assignees: []
created_at: 2017-04-26T15:41:39Z
updated_at: 2017-05-08T21:31:14Z
url: https://github.com/BurntSushi/ripgrep/issues/461
synced_at: 2026-01-12T16:13:22Z
```

# feature: preserve first N lines

---

_@eminence_

This is a new feature request.

Some command line utilities return data in a tabular format, with rows and columns, and often some type of header printed in the first 1 or 2 lines.  For example:

```
achin@bigbox ~ $ /sbin/zpool iostat -v
               capacity     operations    bandwidth
pool        alloc   free   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
storage     6.05T  7.51T     74     62  2.19M   537K
  raidz1    4.57T   885G     37     22  1.53M   160K
    sdb         -      -     15     14   793K  82.0K
    sdc         -      -     16     14   792K  82.0K
    sdh         -      -     15     14   792K  82.0K
  raidz1    1.48T  6.65T     36     39   680K   377K
    sdg         -      -     14     12   345K   253K
    sdf         -      -     14     12   348K   253K
    sde         -      -     15     12   346K   253K
----------  -----  -----  -----  -----  -----  -----
```

Grepping for something within this table is non-ideal because generally the column headers are removed, making it harder to understand the resulting data:

```
achin@bigbox ~ $ /sbin/zpool iostat -v |rg sdb
    sdb         -      -     15     14   793K  82.0K
```

What about a new option that would preserve the first `N` lines of text.  In this case, I'd want to preserve the first 2 or 3 lines.   Using this proposed/hypothetical new option, the result might look like:

```
achin@bigbox ~ $ /sbin/zpool iostat -v |rg --show-first=3 sdb
               capacity     operations    bandwidth
pool        alloc   free   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
    sdb         -      -     15     14   793K  82.0K
```


Open questions:
 * What would be the behavior if there were no matches at all?  Should the first `N` lines of text still be shown?   I think they should not.
* What would be the behavior when searching multiple files?  I think:  if a file matches, show the first `N` lines, and then the match(s).  If there are no matches, print nothing.

---

_Comment by @BurntSushi on 2017-04-26 15:46_

I think this is beyond the scope of ripgrep.

---

_Comment by @eminence on 2017-04-26 15:53_

To advocate very briefly for this feature request, it seems like it's not too much of a conceptual addition beyond `--context` and the other options that make it easier for a user to understand the results of their search.   But I for sure understand that this feature request represents expanded functionality that not at the core of what ripgrep does.

---

_Comment by @BurntSushi on 2017-04-26 16:11_

This is a perfect example of why "saying No" is literally the hardest thing about maintaining an open source project. I cannot really deny that this *particular* feature is a small conceptual leap, but adding it makes things that would otherwise be large conceptual leaps become small conceptual leaps. In other words, feature creep.

Consider some of these things:

1. ripgrep has a ton of options already. Even among the people that would benefit from this feature, how many of them will actually be able to learn about it? The more flags we add for niche things like this, the worse this becomes.
2. "context" is a well defined concept from grep and has decent mindshare. It means, "the lines surrounding a match." It even has its own delimiter when printed to stdout. Does the output resulting from the `--show-first=3` flag have its own delimiter? What if it overlaps with context that would be emitted from a match in the fourth line? What is the delimiter then?
3. With this flag, other flags like `--show-last` suddenly don't seem that far off either. And if this is being applied across multiple files, then some folks are certainly going to want this printed conditionally, perhaps, "`--show-first=3` if and only if `pattern` matches in the first 2 lines" or something like that. It sounds a little ridiculous right now, but I think there are actually open feature requests to do just that for the existing context options.

Consider a less-than-ideal work-around if you really want this:

```
$ rg -e operations -e sdb /tmp/461
1:               capacity     operations    bandwidth
6:    sdb         -      -     15     14   793K  82.0K
```

---

_Comment by @eminence on 2017-04-26 16:43_

Yep, as mentioned I for sure understand.  

I opened a feature request, you evaluated it, this is exactly how open-source development is supposed to happen, even if it's hard :smile: 

---

_Comment by @djczaski on 2017-04-26 17:39_

A little clunky, but you could do something like:

````text
$ ps aux | tee >(head -n1 >&2) | (sleep 0.1;rg rcuos)
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         9  0.1  0.0      0     0 ?        S    Apr22   7:06 [rcuos/0]
root        18  0.0  0.0      0     0 ?        S    Apr22   1:50 [rcuos/1]
root        25  0.0  0.0      0     0 ?        S    Apr22   1:36 [rcuos/2]
root        32  0.0  0.0      0     0 ?        S    Apr22   1:36 [rcuos/3]
````


---

_Closed by @BurntSushi on 2017-05-08 21:31_

---
