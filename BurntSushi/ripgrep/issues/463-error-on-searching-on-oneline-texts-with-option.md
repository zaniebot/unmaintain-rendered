```yaml
number: 463
title: Error on searching on oneline texts with option --only-matching
type: issue
state: closed
author: funkill
labels:
  - duplicate
assignees: []
created_at: 2017-04-27T11:45:55Z
updated_at: 2017-04-27T16:20:32Z
url: https://github.com/BurntSushi/ripgrep/issues/463
synced_at: 2026-01-12T16:13:22Z
```

# Error on searching on oneline texts with option --only-matching

---

_@funkill_

When searching in oneline text (e.g. minified json) with option `--only-matching`, I got the result only first match.

```bash
$ rg --version
ripgrep 0.5.1
$ cat ./test.json 
[{"character":"a","code":"U+0061"},{"character":"b","code":"U+0062"},{"character":"c","code":"U+0063"},{"character":"d","code":"U+0064"},{"character":"e","code":"U+0065"},{"character":"f","code":"U+0066"},{"character":"g","code":"U+0067"}]
$ rg '"character":"[\w]"' ./test.json 
1:[{"character":"a","code":"U+0061"},{"character":"b","code":"U+0062"},{"character":"c","code":"U+0063"},{"character":"d","code":"U+0064"},{"character":"e","code":"U+0065"},{"character":"f","code":"U+0066"},{"character":"g","code":"U+0067"}]
$ rg '"character":"[\w]"' -o ./test.json 
1:"character":"a"
1:"character":"a"
1:"character":"a"
1:"character":"a"
1:"character":"a"
1:"character":"a"
1:"character":"a"
```

---

_Comment by @BurntSushi on 2017-04-27 11:48_

I think this is a dupe of #451 is fixed on master.

---

_Label `duplicate` added by @BurntSushi on 2017-04-27 11:48_

---

_Comment by @kpp on 2017-04-27 13:53_

Already fixed

```
~/ripgrep$ git rev-parse HEAD
362abed44a000da3b31e5dd027e16eb4e36e1bed
~/ripgrep$ cargo run -- '"character":"[\w]"' -o ./tests/test.json
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg '"character":"[\w]"' -o ./tests/test.json`
1:"character":"a"
1:"character":"b"
1:"character":"c"
1:"character":"d"
1:"character":"e"
1:"character":"f"
1:"character":"g"
```

---

_Closed by @BurntSushi on 2017-04-27 16:20_

---
