```yaml
number: 1678
title: "Feature Request: Add support for removal of adjacent lines with invert match mode."
type: issue
state: closed
author: mingwandroid
labels:
  - question
assignees: []
created_at: 2020-09-10T19:26:03Z
updated_at: 2021-06-07T04:08:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1678
synced_at: 2026-01-12T16:13:24Z
```

# Feature Request: Add support for removal of adjacent lines with invert match mode.

---

_@mingwandroid_

When building boost, it emits a huge log mostly full of lines like:

```
common.copy C:\opt\conda\conda-bld\bst-1.73.0_18\_h_env\Library\include\boost\metaparse\v1\repeated_reject_incomplete.hpp
C:\opt\conda\conda-bld\bst-1.73.0_18\work\boost\metaparse\v1\repeated_reject_incomplete.hpp
        1 file(s) copied.
```

I'd like to elide all 3 of these lines (via using rg in a pipe) with a single rg command if possible. Does this sound sensible and/or possible?

Many thanks for ripgrep.

---

_Comment by @BurntSushi on 2020-09-13 13:01_

I don't really understand, sorry. This is what shell pipelines are for: `rg ... | rg -v copied | rg -v common.copy` or whatever.

When making feature requests, _please_ include the desired command you want to run, along with inputs and the desired output.

---

_Label `question` added by @BurntSushi on 2020-09-13 13:02_

---

_Comment by @MagicMuscleMan on 2021-06-07 01:57_

I am not exactly sure if this is what @mingwandroid  wants, but it sounds like what I wanted to request, so I put it here. If you, @mingwandroid , wanted something else, please tell me, then I'll create a new issue.

It should be possible to search for one string but exclude another string in one rg call. Reasons:

1. If you are in a large loop creating two processes for each iteration step is slower.
2. More advanced shell calls tend to use `xargs` calls a lot (often with parallel execution), where the syntax of piping is very cumbersome (an extra shell needs to be opened explicitly)
3. If you combine both issues, i.e. have xargs calls in a loop, this makes performance worse

Assume you want to search for `a` but exclude matches of `b`, then the following possible future calls come to my mind

```
echo 'ab
         a
         ac
         c' | rg -e a -v -e b
```
All regexp parameters after `-v` would be inverted, but all before `-v` would not be inverted. This feels most familiar, but might be problematic due to backwards compatibility (with older versions and with grep). This problem can be seen with the current implementation, which returns `c` instead of `a` and `ac` as the `-v` somehow non-intuitively is also used for the regular expression coming before `-v'.

```
echo 'ab
         a
         ac
         c' | rg -e a -V -e b
```
Using `-V` might be an alternative to avoid the compatibility problem. Note that currently  `-V` prints the version (not documented in the man page, but in the `--help` output). This could be changed in such a way that the version is only printed if it's the only argument given in the call to ripgrep. If this seems ugly, another letter could be used, of course, but `-V` has a nice analogy to the well-known `-v` from in**v**ert.

```
echo 'ab
         a
         ac
         c' | rg -e a -V b
```
This might be even simpler to type, to read and to implement, as `-V` would not change the interpretation of all future regexp parameters but would only invert the now following regexp parameter (acting like a single inverted `-e` parameter). This would preserve order-independence and therefore `rg -V b -e a` would have identical output. Of course, `-V` could be again exchanged by another letter, if seen necessary.

---

_Comment by @BurntSushi on 2021-06-07 04:08_

Dupe of #875 

---

_Closed by @BurntSushi on 2021-06-07 04:08_

---
