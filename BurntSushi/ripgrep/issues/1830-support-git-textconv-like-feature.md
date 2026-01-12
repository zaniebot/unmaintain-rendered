```yaml
number: 1830
title: Support git textconv-like feature
type: issue
state: closed
author: zachriggle
labels: []
assignees: []
created_at: 2021-03-20T00:43:53Z
updated_at: 2021-03-20T01:04:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1830
synced_at: 2026-01-12T16:13:24Z
```

# Support git textconv-like feature

---

_@zachriggle_

Git has a neat feature, called `textconv`, that is used when generating diffs, or when searching code with git-grep.

You can do lots of neat pre-processing, e.g. to reformat source code on-the-fly or unwrap wrapped lines, so that each expression fully fits on one line.

It would be nice if RipGrep supported a `--textconv` flag, which invokes a script/binary with the original data as `stdin` and then `rg` works on the output of the script.

This does introduce a good deal of performance overhead, but the benefits that it provides are immeasurable (and it's an optional, default-off flag).

### Example 1

A simple example would be to replace tabs with newlines.

```sh
$ cat fix-whitespace
sed -E 's|\t|    |g'
$ rg --textconv fix-whitespace -e foo
```

### Example 2

A more complex example does expression wrapping, so that

```c
$ cat foo.c
int main() {
    void foo = bar(a,
               b,
               c);
};
```

I have a `sed` script (which I can't share due to it being an internal tool) called `formatting-unwrap` that converts this.

```c
$ formatting-unwrap < foo.c
int main() {
    void foo = bar(a, b, c);
};
```

This makes grep results much more useful, for example if I'm looking for lines that contain `bar` calls with variable `c`.

```
git grep -e bar --and -e c
```

Without the `--textconv` flag, no matches are found.  With `--textconv` passed and some settings (e.g. `git -c "diff.c.textconv=formatting-unwrap"`), we can find matches.


---

_Comment by @BurntSushi on 2021-03-20 00:47_

Why doesn't the `--pre` flag work for your use case? See: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#preprocessor

---

_Comment by @zachriggle on 2021-03-20 00:51_

Indeed it does! I had no idea about that flag, that's really great!  Thanks again for making such a great tool <3

I definitely need to RTFM some more.

---

_Closed by @zachriggle on 2021-03-20 00:56_

---
