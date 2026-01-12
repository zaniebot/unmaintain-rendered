```yaml
number: 50
title: .rgignore glob?
type: issue
state: closed
author: kaushalmodi
labels: []
assignees: []
created_at: 2016-09-24T04:37:14Z
updated_at: 2016-09-26T12:49:02Z
url: https://github.com/BurntSushi/ripgrep/issues/50
synced_at: 2026-01-12T18:23:11Z
```

# .rgignore glob?

---

_@kaushalmodi_

I am trying to figure out how to ignore a particular dir name (that dir name could occur multiple times in the search path) using `.rgignore`.

Let's say I have the `.rgignore` as `/proj/SOMEPRJ/.rgignore`, and I have these directories
- `/proj/SOMEPRJ/abc/def/XYZ/`
- `/proj/SOMEPRJ/ghi//XYZ/`

What do I put in `/proj/SOMEPRJ/.rgignore` to ignore both the `XYZ/` dirs?

For `ag`, I can simply put `XYZ` in the `.agignore` and be done with it. But the same does not work here.

`*XYZ` also does not work.

For now I need to put in the full paths to `XYZ/` (both) in the `.rgignore`.

What is the right way?


---

_Comment by @kaushalmodi on 2016-09-24 04:41_

The benefit of allowing `XYZ/` to ignore both of the above dirs is that I could be initiating my search from any of these directories and that ignore pattern will still work:
- `/proj/SOMEPRJ/`
- `/proj/SOMEPRJ/abc/`
- `/proj/SOMEPRJ/abc/def/`
- `/proj/SOMEPRJ/ghi/`


---

_Comment by @BurntSushi on 2016-09-24 04:50_

Hmm, I can't reproduce:

```
[andrew@Cheetah 50] tree
.
└── proj
    └── SOMEPRJ
        ├── abc
        │   └── def
        │       └── XYZ
        │           └── bar
        └── ghi
            └── XYZ
                └── bar

7 directories, 2 files
[andrew@Cheetah 50] rg foo
proj/SOMEPRJ/abc/def/XYZ/bar
1:foo

proj/SOMEPRJ/ghi/XYZ/bar
1:foo
[andrew@Cheetah 50] echo XYZ > .rgignore
[andrew@Cheetah 50] rg foo
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

A few questions:
1. What directory are running `rg` in?
2. Could you run `rg` with `--debug` set and post the output here?

Thanks!


---

_Comment by @kaushalmodi on 2016-09-24 05:09_

Sorry, actually the ignore pattern was something like `XXX/YYY/`.

```
km²~/temp/:proj> tree                                                                          09/24 1:07am
.
└── SOMEPRJ
    ├── abc
    │   └── def
    │       └── XXX
    │           └── YYY
    │               └── bar
    └── ghi
        └── XXX
            └── YYY
                └── bar

8 directories, 2 files
km²~/temp/:proj> cat SOMEPRJ/abc/def/XXX/YYY/bar                                                                                                                                                     09/24 1:08am
foo
km²~/temp/:proj> cat SOMEPRJ/.rgignore                                                                                                                                                               09/24 1:08am
XXX/YYY/
km²~/temp/:proj> rg --no-ignore-parent foo                                                                                                                                                           09/24 1:08am
SOMEPRJ/abc/def/XXX/YYY/bar
1:foo

SOMEPRJ/ghi/XXX/YYY/bar
1:foo
km²~/temp/:proj> rg --no-ignore-parent foo --debug                                                                                                                                                   09/24 1:08am
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'f',
        'o',
        'o'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG:rg::ignore: ./SOMEPRJ/.rgignore ignored because it is hidden
SOMEPRJ/abc/def/XXX/YYY/bar
1:foo

SOMEPRJ/ghi/XXX/YYY/bar
1:foo
```


---

_Comment by @BurntSushi on 2016-09-24 05:26_

OK, I think this is a dupe of #25. I'll be sure to include your case in a regression test when I fix it.


---

_Comment by @BurntSushi on 2016-09-24 20:25_

Actually, this is a dupe of #16. Fix incoming.


---

_Closed by @BurntSushi on 2016-09-24 20:25_

---

_Comment by @kaushalmodi on 2016-09-26 12:49_

I confirm the fix. Thanks!


---
