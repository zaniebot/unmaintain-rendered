```yaml
number: 49
title: .gitignore entry ignored depending on path parameter
type: issue
state: closed
author: mengelbrecht
labels:
  - bug
assignees: []
created_at: 2016-09-24T04:35:32Z
updated_at: 2016-09-24T20:26:25Z
url: https://github.com/BurntSushi/ripgrep/issues/49
synced_at: 2026-01-12T18:23:11Z
```

# .gitignore entry ignored depending on path parameter

---

_@mengelbrecht_

Depending on the path parameter supplied to ripgrep on the commandline .gitignore patterns are ignored.

Setup:

``` shell
$ mkdir -p test/foo/bar
$ touch test/foo/bar/baz
$ echo 'foo/bar' > test/.gitignore
```

When run with `test` as path parameter the `bar` directory is not ignored.

``` shell
$ rg --debug --files "" test
DEBUG:grep::search: regex ast:
Empty
DEBUG:rg::ignore: test/.gitignore ignored because it is hidden
test/foo/bar/baz
```

When run inside test it is correctly ignored:

``` shell
$ cd test
$ rg --debug --files ""
DEBUG:grep::search: regex ast:
Empty
DEBUG:rg::ignore: ./.gitignore ignored because it is hidden
DEBUG:rg::ignore: ./foo/bar ignored by Pattern { from: "./.gitignore", original: "foo/bar", pat: "foo/bar", whitelist: false, only_dir: false }
```


---

_Comment by @BurntSushi on 2016-09-24 04:47_

I think this is a dupe of #25. I'll be sure to include your setup as a regression test.


---

_Label `bug` added by @BurntSushi on 2016-09-24 04:47_

---

_Comment by @BurntSushi on 2016-09-24 20:26_

Actually, this is a dupe of #16. Fix incoming.


---

_Closed by @BurntSushi on 2016-09-24 20:26_

---
