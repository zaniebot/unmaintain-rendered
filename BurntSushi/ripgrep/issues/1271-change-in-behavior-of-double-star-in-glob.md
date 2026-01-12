```yaml
number: 1271
title: Change in behavior of double star in glob
type: issue
state: closed
author: danadam
labels:
  - question
assignees: []
created_at: 2019-04-26T18:33:18Z
updated_at: 2019-04-26T19:23:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1271
synced_at: 2026-01-12T16:13:23Z
```

# Change in behavior of double star in glob

---

_@danadam_

Up until rg 0.7.1, with the following file structure:
```
mkdir -p foo/test/bar foo/prod/bar
echo "hello" > foo/test/bar/hello.txt
cp foo/test/bar/hello.txt foo/prod/bar/
```
If I wanted to match (or exclude) specific dir anywhere in the file path I did:
```
]$ rg -g'test/**' hello
foo/test/bar/hello.txt
1:hello

]$ rg -g'!test/**' hello
foo/prod/bar/hello.txt
1:hello
```
Using double star at the front worked only with negation:
```
$ rg -g'**/test/' hello
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

]$ rg -g'!**/test/' hello
foo/prod/bar/hello.txt
1:hello
```
In rg 0.8.1 this changed. Double star at the end doesn't work:
```
]$ rg -g'test/**' hello
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

]$ rg -g'!test/**' hello
foo/test/bar/hello.txt
1:hello

foo/prod/bar/hello.txt
1:hello
```
Behavior of double star at the front didn't change.

Using double star on both sides, i.e., `**/test/**` works as expected in both versions.

Is that a bug? Or is `**/test/**` really the minimal correct pattern that can be simply negated with `!` and work as expected?

---

_Comment by @BurntSushi on 2019-04-26 19:23_

Yup, this is indeed correct. I'm not going to dig around for the specific change that caused this, but `test/**` should match `test/foo` but not `bar/test/foo`. It might make sense to add `--include-dir` or `--exclude-dir` flags as part of #809, but that isn't much shorter.

> Or is `**/test/**` really the minimal correct pattern that can be simply negated with ! and work as expected?

I'm less sure about this. My thinking is that `-g '*test*'` should also work _in this case_, but it doesn't. However, this matches strictly more things than `**/test/**`. (`*test*` should match `foo/atestb/bar` where as `**/test/**` shouldn't.)

---

_Label `question` added by @BurntSushi on 2019-04-26 19:23_

---

_Closed by @BurntSushi on 2019-04-26 19:23_

---
