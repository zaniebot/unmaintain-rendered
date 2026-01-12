```yaml
number: 517
title: glob exclude option does not work as expected on Windows
type: issue
state: closed
author: stalwartgiraffe
labels: []
assignees: []
created_at: 2017-06-15T15:47:41Z
updated_at: 2017-06-15T17:56:52Z
url: https://github.com/BurntSushi/ripgrep/issues/517
synced_at: 2026-01-12T16:13:22Z
```

# glob exclude option does not work as expected on Windows

---

_@stalwartgiraffe_

The glob exclude option does not seem to work as expected.  Attached is a zip of a dir with several txt files in it which contain the string literal whatever.  I tried to do a search which excluded files which ended in .map and .log but on Windows 10 received this output
[rgtest.zip](https://github.com/BurntSushi/ripgrep/files/1078175/rgtest.zip)

c:\rgtest>rg -g !*.map.log  -F whatever
whatever.txt
2:whatever

sub1\whatever.map
2:whatever

sub1\whatever.txt
2:whatever

sub1\whatever.log
2:whatever

c:\rgtest>rg -g '!*.map.log'  -F whatever
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

c:\rgtest>rg -g "!*.map.log"  -F whatever
whatever.txt
2:whatever

sub1\whatever.log
2:whatever

sub1\whatever.map
2:whatever

sub1\whatever.txt
2:whatever


---

_Comment by @BurntSushi on 2017-06-15 16:02_

Wait, you don't have any files that end with `.map.log` though? The glob `*.map.log` only matches things like `foo.map.log` but *not* `foo.map` or `foo.log`. If you want to match `.map` or `.log`, then use `*.{map,log}`. Alternatively, `-g '!*.log' -g '!*.map'` would work.

---

_Comment by @stalwartgiraffe on 2017-06-15 17:56_

ok I  you may want to update the doc page
https://github.com/BurntSushi/ripgrep
on git with the above example in the email.  The one in the page must have
confused me...

-----------------

Or exclude files matching a particular glob:

$ rg foo -g '!*.min.js'


-----------------------
and I misread it to mean it that it would to apply to files ending in *.min
OR *.js
So maybe doc clarification is in order, which spells out the cases

ALSO, for me, single quoting the glob pattern simple does not work on the
dos shell.  Double quote and no quote does work using the pattern you
suggest in email.  I will close the issue.


cc:\MetaVR\Dev\rgtest>rg -g '!*.log' whatever
No files were searched, which means ripgrep probably applied a filter you
didn't expect. Try running again with --debug.

c:\MetaVR\Dev\rgtest>rg -g "!*.log" whatever
sub1\whatever.map
2:whatever

sub1\whatever.txt
2:whatever

whatever.txt
2:whatever

c:\MetaVR\Dev\rgtest>rg -g !*.log whatever
whatever.txt
2:whatever

sub1\whatever.map
2:whatever

sub1\whatever.txt
2:whatever



On Thu, Jun 15, 2017 at 12:03 PM, Andrew Gallant <notifications@github.com>
wrote:

> Wait, you don't have any files that end with .map.log though? The glob
> *.map.log only matches things like foo.map.log but *not* foo.map or
> foo.log. If you want to match .map or .log, then use *.{map,log}.
> Alternatively, -g '!*.log' -g '!*.map' would work.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/517#issuecomment-308785146>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AGGkLCGP_yEtQ4hArRNfresyWr-xMdpGks5sEVW1gaJpZM4N7Z03>
> .
>


---

_Closed by @stalwartgiraffe on 2017-06-15 17:56_

---
