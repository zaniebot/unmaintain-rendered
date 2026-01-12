```yaml
number: 527
title: "list all files that the \"smart search\" would search through"
type: issue
state: closed
author: wbolster
labels:
  - question
assignees: []
created_at: 2017-06-24T15:20:48Z
updated_at: 2017-06-24T15:42:33Z
url: https://github.com/BurntSushi/ripgrep/issues/527
synced_at: 2026-01-12T16:13:22Z
```

# list all files that the "smart search" would search through

---

_@wbolster_

the `ag` tool has a `ag -l` option that lists all files that it would search through. it would be great if `rg` also had something like `rg --list`.

this is useful for a variety of use cases, e.g. for [entr](http://entrproject.org/):

```
$ ag -l | entr make
```

...and as a "smart" alternative to fiddling with `git ls-files` and so on.

---

_Comment by @BurntSushi on 2017-06-24 15:24_

Firstly, `ag -l` doesn't list all files it would search through. It lists all files that contain at least one match. ripgrep has this flag. (Oh, interestingly, it will print everything if no query is given. One wonders what the difference is between `ag -g .` and `ag -l`.)

Secondly, ripgrep has a `--files` flag that does list all files that it would search. Why doesn't that work for you?

---

_Label `question` added by @BurntSushi on 2017-06-24 15:24_

---

_Comment by @wbolster on 2017-06-24 15:42_

ah, `rg --files` exists already. thanks. that solves it for me.

i did not notice it looking for it in the `--help` output, and the reason is the rather odd alignment:

```
    -E, --encoding <ENCODING>
            Specify the text encoding that ripgrep will use on all files
            searched. The default value is 'auto', which will cause ripgrep
            to do a best effort automatic detection of encoding on a per-file
            basis. Other supported values can be found in the list of labels
            here: https://encoding.spec.whatwg.org/#concept-encoding-get
             
    -f, --file <FILE>...
            Search for patterns from the given file, with one pattern per
            line. When this flag is used or multiple times or in combination
            with the -e/--regexp flag, then all patterns provided are
            searched. Empty pattern lines will match all input lines, and the
            newline is not counted as part of the pattern.
             
        --files
            Print each file that would be searched without actually
            performing the search. This is useful to determine whether a
            particular file is being searched or not.
             
    -l, --files-with-matches
            Only show the paths with at least one match.
```

may i suggest left aligning the headers, even if no `-o` `-n` `-e` letter shortcut is available?

and yeah, `ag` makes the query optional:

```
       -l --files-with-matches
              Only print the names of files  containing  matches,  not  the
              matching  lines.  An  empty  query  will print all files that
              would be searched.
```

the difference with `ag -g .` would be empty files perhaps.

---

_Closed by @wbolster on 2017-06-24 15:42_

---
