```yaml
number: 363
title: Can we show file name paths?
type: issue
state: closed
author: ryanberckmans
labels: []
assignees: []
created_at: 2017-02-13T20:58:39Z
updated_at: 2017-02-13T21:20:11Z
url: https://github.com/BurntSushi/ripgrep/issues/363
synced_at: 2026-01-12T16:13:21Z
```

# Can we show file name paths?

---

_@ryanberckmans_

It'd be very helpful in large codebases to show file names with paths relative to rg pwd:

```
$ rg --new-option-much-wow Yay // similar to -H --with-filename

yay_ripgrep.go  // i.e. this file is in pwd
74:     if result.YayRipgrep {
143:    if IsYay {

types/widgets/yellow_widget.go // subdir
96:     type YellowWidgetYay struct {
```

Current behavior in ripgrep 0.4.0

```
$ rg Yay

yay_ripgrep.go
74:     if result.YayRipgrep {
143:    if IsYay {

yellow_widget.go
96:     type YellowWidgetYay struct {
```


---

_Comment by @BurntSushi on 2017-02-13 21:10_

> It'd be very helpful in large codebases to show file names with paths relative to rg pwd:

I don't understand. This is already the default behavior.

I can't reproduce your example. Can you reproduce it on a repository we can both access and show the output of `rg --debug ...`? e.g.,

```
$ git clone git://github.com/BurntSushi/ripgrep
$ cd ripgrep
$ rg foobar
README.md
235:$ rg foobar
249:$ rg -uu foobar  # similar to `grep -r`
250:$ rg -uuu foobar  # similar to `grep -a -r`
304:$ rg -thtml -tcss foobar
310:$ rg -Tjs foobar
318:$ rg --type-add 'foo:*.{foo,foobar}' -tfoo bar
321:The type `foo` will now match any file ending with the `.foo` or `.foobar`

tests/tests.rs
865:#parse ("widgets/foobarhiddenformfields.vm")

ignore/src/gitignore.rs
524:    const ROOT: &'static str = "/home/foobar/rust/rg";

ignore/src/types.rs
662:            "combo:foobar:html,rust",
```

---

_Comment by @ryanberckmans on 2017-02-13 21:20_

My fault, was wrong. Thanks for looking into it.

---

_Closed by @ryanberckmans on 2017-02-13 21:20_

---
