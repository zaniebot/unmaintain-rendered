```yaml
number: 82
title: "Comma separated file types (e.g. --type-add html:*.html,*.htm) do not work as stated"
type: issue
state: closed
author: SigmundVik
labels: []
assignees: []
created_at: 2016-09-25T13:32:43Z
updated_at: 2016-09-25T18:37:10Z
url: https://github.com/BurntSushi/ripgrep/issues/82
synced_at: 2026-01-12T18:23:11Z
```

# Comma separated file types (e.g. --type-add html:*.html,*.htm) do not work as stated

---

_@SigmundVik_

From the `rg --help` output:

```
    --type-add ARG ...
        Add a new glob for a particular file type.
        Example: --type-add html:*.html,*.htm
```

Here are three test cases using this to grep for the word `uniqueness` in a tree containing the source code and documentation for the `boost` library:

```
[BOOTCAMP] [sigmund-MAC-dev] 15:09 0:00:00.13 88956M d:\dev\base-00\lib\boost>rg --type-add html:*.html,*.htm -t html uniqueness .
doc\html\intrusive\advanced_lookups_insertions.html
350:        are known to fulfill order (sets and multisets) and uniqueness (sets) invariants.
353:        up assignments from other associative containers: if the ordering and uniqueness
362:        ordering and uniqueness preconditions are not assured by the caller.</strong></span>

doc\html\jam\language.html
88:        with grist, a string value used to assure uniqueness. A file target with
```

The case above gives the expected output.

```
[BOOTCAMP] [sigmund-MAC-dev] 15:10 0:00:00.47 88956M d:\dev\base-00\lib\boost>rg --type-add html:*.xxx -t html uniqueness .
doc\html\intrusive\advanced_lookups_insertions.html
350:        are known to fulfill order (sets and multisets) and uniqueness (sets) invariants.
353:        up assignments from other associative containers: if the ordering and uniqueness
362:        ordering and uniqueness preconditions are not assured by the caller.</strong></span>

doc\html\jam\language.html
88:        with grist, a string value used to assure uniqueness. A file target with
```

The case above does not give the expected output.  There are no files in the tree with file extensions "xxx".

```
[BOOTCAMP] [sigmund-MAC-dev] 15:10 0:00:00.59 88956M d:\dev\base-00\lib\boost>rg --type-add xxx:*.html,*.htm -t xxx uniqueness .
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

The case above does not give the expected output.  The expected otput is the same as for the first case.

This was produced running rg version 0.1.17.


---

_Comment by @BurntSushi on 2016-09-25 13:49_

OK, so the first problem is that the example is wrong: there is no support for comma separated globs. The docs need to be fixed.

The second problem is that `--type-add` is additive. Since `html` is already defined in `ripgrep`, running `--type-add 'html:*.xxx'` adds `*.xxx` to the already existing globs. If you want to start fresh, use `--type-clear html`.

The docs could use more examples.


---

_Comment by @SigmundVik on 2016-09-25 14:01_

I see, thank you!

There is already a suggestion for supporting curly brace glob syntax (https://github.com/BurntSushi/ripgrep/issues/80).

With this, the documentation could be updated to:

```
    --type-add ARG ...
        Add a new glob for a particular file type.
        Example: --type-add html:*.{html,htm}
```


---

_Comment by @BurntSushi on 2016-09-25 14:05_

That's a good point, thanks. :-)


---

_Closed by @BurntSushi on 2016-09-25 18:37_

---
