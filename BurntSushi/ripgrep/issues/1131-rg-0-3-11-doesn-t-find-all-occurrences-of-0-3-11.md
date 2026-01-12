```yaml
number: 1131
title: "rg \"0\\.3\\.11\" doesn't find all occurrences of \"0.3.11\""
type: issue
state: closed
author: bbarker
labels:
  - invalid
assignees: []
created_at: 2018-12-02T01:35:28Z
updated_at: 2018-12-02T01:48:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1131
synced_at: 2026-01-12T16:13:23Z
```

# rg "0\.3\.11" doesn't find all occurrences of "0.3.11"

---

_@bbarker_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

nixpkgs

#### What operating system are you using ripgrep on?

Ubuntu 18.04

```
Linux brandon-750-170se 4.15.0-39-generic #42-Ubuntu SMP Tue Oct 23 15:48:01 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```

#### Describe your question, feature request, or bug.

It seems this may be a bug: my expectation is that `rg "0\.3\.11" ` would turn up all textual occurrences of the string "0.3.11".

#### If this is a bug, what are the steps to reproduce the behavior?

Clone the repo at (note the tag): https://github.com/githwxi/ATS-Postiats/tree/v0.3.12

Run `rg "0\.3\.11" ` from the root of the cloned repo at the specified tag/branch.

Note that several results do come up, but not the result in `./doc/DISTRIB/ATS-Postiats/configure.ac`
which has the line:

```
AC_INIT([ATS2/Postiats], [0.3.11], [gmpostiats@gmail.com])       
```

#### If this is a bug, what is the actual behavior?

```
$ rg "0\.3\.11"
CHANGES-ats2
35:0.3.11

doc/DISTRIB/ATS-Postiats/RELEASE/ats2-postiats-0.3.11-release.html
5:<title>ATS2-0.3.11</title>
11:ats2-postiats-0.3.11:
17:ATS2-0.3.11.tgz:

doc/PROJECT/MEDIUM/ats2langweb/thePageRBodyLContent/Downloads.php
22:<a href="http://sourceforge.net/projects/ats2-lang/download">ATS2-0.3.11</a>.
264:<a href="http://sourceforge.net/projects/ats2-lang/files/ats2-lang/ats2-postiats-0.3.11/.">ATS2-contrib-0.3.11</a>.
320:<a href="http://sourceforge.net/projects/ats2-lang/files/ats2-lang/ats2-postiats-0.3.11/.">ATS2-include-0.3.11</a>.

doc/PROJECT/MEDIUM/ats2langweb/thePageRBodyRight/Community.php
12:>ATS2-0.3.11 has been released</a><br>
```

The output with `--debug` is quite large on the entire repo (or even the folder, but here is the debug output from the folder doc/DISTRIB/ATS-Postiats): https://gist.github.com/bbarker/1a3899c97ca141f5a259e172905aab00


I first tried to `cd` into the directory with 'configure.ac' to see what was going on, and noticed while doing so that specifying the glob patter "*.ac" actually causes a hit:

```
$ pwd
/home/brandon/workspace/ATS-Postiats/doc/DISTRIB/ATS-Postiats
$ rg "0\.3\.11" 
RELEASE/ats2-postiats-0.3.11-release.html
5:<title>ATS2-0.3.11</title>
11:ats2-postiats-0.3.11:
17:ATS2-0.3.11.tgz:
$ rg "0\.3\.11" -g "*.ac" 
configure.ac
49:AC_INIT([ATS2/Postiats], [0.3.11], [gmpostiats@gmail.com])
```

Interestingly, if I move the configure.ac to a folder by itself, I get a hit without the glob:

```
$ rg "0\.3\.11"                             
configure.ac
49:AC_INIT([ATS2/Postiats], [0.3.11], [gmpostiats@gmail.com])
```

#### If this is a bug, what is the expected behavior?

My expectation is that `rg "0\.3\.11" ` would turn up all textual occurrences of the string "0.3.11".



---

_Comment by @BurntSushi on 2018-12-02 01:47_

Thanks for the great bug report!

ripgrep automatically filters out files, such as hidden files and files matching your `gitignore`. The `--debug` output is supposed to tell you why:

```
$ rg '0\.3\.11' --debug 2>&1 | rg -F 'doc/DISTRIB/ATS-Postiats/configure.ac'
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./doc/DISTRIB/ATS-Postiats/configure.ac: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "doc/DISTRIB/ATS-Postiats/config*", actual: "doc/DISTRIB/ATS-Postiats/config*", is_whitelist: false, is_only_dir: false })))
```

In this case, it's specifically telling you that you have a rule that is causing your file to get ignored. Indeed, line 211 in your `.gitignore` is one such rule:

```
doc/DISTRIB/ATS-Postiats/config*
```

You'll want to do a read of the [guide's section on automatic filtering](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering).

N.B. Running `rg -u "0\.3\.11"` gives the output you're looking for.

---

_Closed by @BurntSushi on 2018-12-02 01:47_

---

_Comment by @okdana on 2018-12-02 01:47_

`doc/DISTRIB/ATS-Postiats/config*` is present in `.gitignore`; you can use `rg -u` if you don't want it to respect the rules in that file

Edit: Oops, sorry @BurntSushi 

---

_Label `invalid` added by @BurntSushi on 2018-12-02 01:47_

---
