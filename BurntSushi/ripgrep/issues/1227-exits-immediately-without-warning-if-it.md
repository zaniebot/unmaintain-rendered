```yaml
number: 1227
title: Exits immediately without warning if it encounters a NUL byte inside the file to be searched, might exit with wrong exit code depending on the position of the match
type: issue
state: closed
author: xtaran
labels:
  - duplicate
assignees: []
created_at: 2019-03-26T18:13:14Z
updated_at: 2019-03-26T18:31:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1227
synced_at: 2026-01-12T16:13:23Z
```

# Exits immediately without warning if it encounters a NUL byte inside the file to be searched, might exit with wrong exit code depending on the position of the match

---

_@xtaran_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`ripgrep` package version 0.10.0-2 from Debian Buster (current "Testing", soon to be Debian 10):

```
~ → apt-cache policy ripgrep
ripgrep:
  Installed: 0.10.0-2
  Candidate: 0.10.0-2
  Version table:
 *** 0.10.0-2 990
        990 https://debian.ethz.ch/debian sid/main amd64 Packages
        600 https://debian.ethz.ch/debian testing/main amd64 Packages
        100 /var/lib/dpkg/status
```

#### What operating system are you using ripgrep on?

```
~ → lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux buster/sid
Release:        unstable
Codename:       sid
```

#### Describe your question, feature request, or bug.

with several GB of log files via STDIN (actually `xzcat` outout), `rg` as well as `rg -F` immediately exited without any output while `fgrep` found many hits until it issued the warning `Binary file (standard input) matches`.

#### If this is a bug, what are the steps to reproduce the behavior?

Consider the following example (based on [this file](https://github.com/BurntSushi/ripgrep/files/3009527/aeh.txt)):

```
→ cat -v aeh.txt
a
M-CM-$
^@
a
→ cat aeh.txt | fgrep a
Binary file (standard input) matches
→ cat aeh.txt | fgrep -a a
a
a
→ cat aeh.txt | rg a
a
→ cat aeh.txt | rg -a a
a
a
```

In the third example with "rg a", rg neither crashed nor issued a warning. fgrep in comparison issued a warning.

While the above example might be close to what fgrep does, just without the warning, the following example is even worse:

```
→ cat aeh.txt | fgrep ä
Binary file (standard input) matches
→ echo $?
0
→ cat aeh.txt | fgrep ö
→ echo $?
1
→ cat aeh.txt | rg ä
→ echo $?
1
→ cat aeh.txt | rg ö
→ echo $?
1
→ cat aeh.txt | rg -a ä
ä
→ echo $?
0
```

So fgrep properly indicates with the exit code if there was a hit even though it didn't output anything besides the warning about binary junk.

But even though the hit would have been before the NUL byte, `rg` claims (via exit code) that there is no hit inside the STDIN despite `rg -a` says otherwise (via output and exit code).

`cat aeh.txt | strace rg ä` shows that it exits rather quickly after having read the NUL byte:

```
read(0, "a\n\303\244\n\0\na\n", 8192)   = 9
sigaltstack({ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=8192}, NULL) = 0
munmap(0x7fbfac3d0000, 8192)            = 0
exit_group(1)                           = ?
+++ exited with 1 +++
```

Constraints to trigger the issue: data must contain a NUL byte and neither of the options `-a` and `--text` must be set. On larger files (gigabytes) it is obvious that rg exits preliminarily if the NUL byte is close to the beginning solely because of how quick the command exits. We actually discovered the issue that way: Searching through syslog files which contained anything remote syslog servers sent to our server, including garbled binary junk. rg exited way too quickly and without any output at all, especially in comparison to fgrep.

Impact: Does not indicate that there were hits and preliminarily exits without further notice, hence can yield wrong results (exit code as well as output) without any indication of there being an issue with binary junk.

Workaround: always use option `-a` or `--text` when contents might contain binary junk instead of relying on warnings from `rg` as fgrep does.

#### If this is a bug, what is the actual behavior?

```
→ cat aeh.txt| rg --debug a
DEBUG|grep_regex::literal|/usr/share/cargo/registry/ripgrep-0.10.0/debian/cargo_registry/grep-regex-0.1.1/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(a)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/usr/share/cargo/registry/ripgrep-0.10.0/debian/cargo_registry/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/usr/share/cargo/registry/ripgrep-0.10.0/debian/cargo_registry/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 0 regexes
a
```

#### If this is a bug, what is the expected behavior?

Behaving as grep/egrep/fgrep: Informing the user about binary junk in the input instead of silently exiting as if no match was found or all matches were found despite this wasn't true.

#### Notes

* Yes, fgrep/grep/egrep also has its issues there like the warning being on STDOUT, not STDERR, but it's still much more clear in indicating the issue compared to `rg`.
* I also tried to see if the options `-F` and `--no-encoding` make a difference in this case, but they don't.
* This might be related to #1207.
* I reported this initially [in Debian as #925544](https://bugs.debian.org/925544)
* Via Github's "possibly related issues" (but when initially searching for `nul` and `null` in the list of open issues) I stumbled upon #306 which seems to declare the observed behaviour as feature named "skipping binary files". But this is IMHO definitely the wrong approach as you can't guess that in advance when e.g. getting gigabytes fed on STDIN just to discover that at some point there's binary junk inmidst already grepped text. So IMHO you should just behave here like grep/egrep/fgrep does and report the fact that you've found a hit in a binary file instead of silently exiting. (It might be ok-ish if you just skip such files when doing recursive greps, but e.g. not if that files was explicitly listed as parameter or data comes from STDIN.)


---

_Comment by @BurntSushi on 2019-03-26 18:25_

Thanks for the thorough report. As you guessed, this is indeed a duplicate of #306. So I'm going to close this in favor of that.

> I stumbled upon #306 which seems to declare the observed behaviour as feature named "skipping binary files". But this is IMHO definitely the wrong approach as you can't guess that in advance when e.g. getting gigabytes fed on STDIN just to discover that at some point there's binary junk inmidst already grepped text. So IMHO you should just behave here like grep/egrep/fgrep does and report the fact that you've found a hit in a binary file instead of silently exiting. (It might be ok-ish if you just skip such files when doing recursive greps, but e.g. not if that files was explicitly listed as parameter or data comes from STDIN.)

Did you read my [analysis on this](https://github.com/BurntSushi/ripgrep/issues/306#issuecomment-442931406)? Please consider giving that a read and commenting on that issue with your thoughts. In particular, my suggested path forward is indeed to do as you say: print a message saying that the binary file matched and the skip the rest. There is some unfortunate implementation complexity with respect to doing this when searching via memory maps.

---

_Closed by @BurntSushi on 2019-03-26 18:25_

---

_Label `duplicate` added by @BurntSushi on 2019-03-26 18:25_

---

_Comment by @xtaran on 2019-03-26 18:31_

> Thanks for the thorough report. As you guessed, this is indeed a duplicate of #306. So I'm going to close this in favor of that.

Fair enough.

> Did you read my [analysis on this](https://github.com/BurntSushi/ripgrep/issues/306#issuecomment-442931406)?

Not completely as I noticed it rather late when I had most of the bug report already written—as this text is mostly the same as in the Debian bug report, just with some Markdown reformatting and squeezing it into the given template.

> Please consider giving that a read and commenting on that issue with your thoughts.

Will do.

> In particular, my suggested path forward is indeed to do as you say: print a message saying that the binary file matched and the skip the rest.

Ok, good know. Thanks for that pointer.

---
