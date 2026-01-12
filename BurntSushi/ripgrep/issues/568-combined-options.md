```yaml
number: 568
title: Combined options
type: issue
state: closed
author: dsciarra
labels: []
assignees: []
created_at: 2017-07-29T14:02:16Z
updated_at: 2017-07-30T21:55:27Z
url: https://github.com/BurntSushi/ripgrep/issues/568
synced_at: 2026-01-12T16:13:22Z
```

# Combined options

---

_@dsciarra_

The following commands in the ripgrep root folder return different results.

`rg -rni hello`
globset/src/glob.rs
1166:    matches!(matchpat1, "*ni.txt", "ni.txt");
1167:    matches!(matchpat2, "*ni.txt", "gareth_says_ni.txt");
1168:    matches!(matchpat3, "*ni.txt", "some/path/to/ni.txt");
1169:    matches!(matchpat4, "*ni.txt", "some\\path\\to\\ni.txt");
1170:    matches!(matchpat5, "*ni.txt", "/an/absolute/path/to/ni.txt");
1171:    matches!(matchpat6, "*some/path/to/ni.txt", "some/path/to/ni.txt");
1172:    matches!(matchpat7, "*some/path/to/ni.txt",
1173:             "a/bigger/some/path/to/ni.txt");
1223:    nmatches!(matchnot16, "*ni.txt", "ni.txt-and-then-some");
1224:    nmatches!(matchnot17, "*ni.txt", "goodbye.txt");
1225:    nmatches!(matchnot18, "*some/path/to/ni.txt",
1226:              "some/path/to/ni.txt-and-then-some");
1227:    nmatches!(matchnot19, "*some/path/to/ni.txt",
1228:              "some/other/path/to/ni.txt");


whereas

`rg -r -n -i hello`
globset/src/glob.rs
1166:    matches!(matchpat1, "*hello.txt", "hello.txt");
1167:    matches!(matchpat2, "*hello.txt", "gareth_says_hello.txt");
1168:    matches!(matchpat3, "*hello.txt", "some/path/to/hello.txt");
1169:    matches!(matchpat4, "*hello.txt", "some\\path\\to\\hello.txt");
1170:    matches!(matchpat5, "*hello.txt", "/an/absolute/path/to/hello.txt");
1171:    matches!(matchpat6, "*some/path/to/hello.txt", "some/path/to/hello.txt");
1172:    matches!(matchpat7, "*some/path/to/hello.txt",
1173:             "a/bigger/some/path/to/hello.txt");
1223:    nmatches!(matchnot16, "*hello.txt", "hello.txt-and-then-some");
1224:    nmatches!(matchnot17, "*hello.txt", "goodbye.txt");
1225:    nmatches!(matchnot18, "*some/path/to/hello.txt",
1226:              "some/path/to/hello.txt-and-then-some");
1227:    nmatches!(matchnot19, "*some/path/to/hello.txt",
1228:              "some/other/path/to/hello.txt");

grep returns in both cases the same result. 

---

_Comment by @okdana on 2017-07-29 15:10_

The `-r` option for `rg` doesn't work like the one for `grep`:

```
-r, --replace <ARG>                     
        Replace every match with the string given when printing results. Neither this flag nor
        any other flag will modify your files.
        
        Capture group indices (e.g., $5) and names (e.g., $foo) are supported in the replacement
        string.
        
        Note that the replacement by default replaces each match, and NOT the entire line. To
        replace the entire line, you should match the entire line.
```

So when you give it `rg -rni hello`, you're telling it to replace the string `hello` in the output by the string `ni`.

`rg -r -n -i hello` should work the same way, except it should replace `hello` by `-n`. The fact that it doesn't is a bug, probably in `clap` (the argument-handling library `rg` uses) — maybe #482.

---

_Closed by @BurntSushi on 2017-07-30 21:55_

---
