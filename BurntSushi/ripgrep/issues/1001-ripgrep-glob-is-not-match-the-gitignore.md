```yaml
number: 1001
title: ripgrep glob is not match the gitignore
type: issue
state: closed
author: jacky1193610322
labels:
  - invalid
assignees: []
created_at: 2018-08-04T14:00:42Z
updated_at: 2019-01-24T16:39:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1001
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep glob is not match the gitignore

---

_@jacky1193610322_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

brew install ripgrep

#### What operating system are you using ripgrep on?

macos 10.14

#### Describe your question, feature request, or bug.

```
➜  temp tree test1
test1
└── test2
    └── a.txt

1 directory, 1 file
➜  temp rg --files -g "test2/" --debug
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./jsonschema.json: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./testarg.py: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./dto/IamJsonschemaDTO.java: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG/ignore::walk/ignore/src/walk.rs:1431: whitelisting ./test1/test2: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "test2/", actual: "**/test2", is_whitelist: false, is_only_dir: true })))))
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./test1/test2/a.txt: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
```

but in the man gitignore
![image](https://user-images.githubusercontent.com/16514353/43677256-7fdfc518-9831-11e8-972c-e2e200ec8e34.png)

so why rg not support the rule

I use rg to search test2 directory and paths underneath it，but it doesn't work.

and in the gitignore, if I add test2, which also match test1/test2/a and test2/a,
but rg "test" -g "test2" only match test2 in the current directory(./test2), not match ./test1/test2
I see the debug output rg "test" -g "test2" which mean rg "test" -g "**/test2"
it match the test2 directory, but it doesn't match the subpath of test2, which is also written in the [globset](https://docs.rs/globset/0.4.1/globset/),
but rg document write Globbing rules match .gitignore globs, but it's actually not.
![image](https://user-images.githubusercontent.com/16514353/43678272-fe9e387e-9842-11e8-8843-eab83f8c7b3d.png)


---

_Comment by @BurntSushi on 2019-01-24 16:39_

As far as I can tell, this is behaving as expected. `-g test2/` causes any directory named `test2` to match, but nothing else. `rg -g 'test2/**'` should do what you want.

---

_Closed by @BurntSushi on 2019-01-24 16:39_

---

_Label `invalid` added by @BurntSushi on 2019-01-24 16:39_

---
