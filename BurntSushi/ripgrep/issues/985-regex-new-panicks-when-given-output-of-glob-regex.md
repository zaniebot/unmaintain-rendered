```yaml
number: 985
title: "Regex::new() panicks when given output of Glob::regex()"
type: issue
state: closed
author: vadimcn
labels:
  - doc
assignees: []
created_at: 2018-07-19T07:08:57Z
updated_at: 2018-07-21T17:25:56Z
url: https://github.com/BurntSushi/ripgrep/issues/985
synced_at: 2026-01-12T16:13:22Z
```

# Regex::new() panicks when given output of Glob::regex()

---

_@vadimcn_

This code
```
    let glob = globset::Glob::new("/foo/bar/*").unwrap();
    regex::Regex::new(glob.regex()).unwrap();
```
panicks with the following message:
```
thread 'test::test' panicked at 'called `Result::unwrap()` on an `Err` value: Syntax(
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
regex parse error:
    (?-u)^/foo/bar/.*$
                   ^
error: pattern can match invalid UTF-8
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
)', libcore/result.rs:945:5
```
I am not entirely sure what is it trying to tell me...

Versions: globset = 0.4.0;  regex = 1.0.2

---

_Comment by @BurntSushi on 2018-07-19 10:31_

Regex::new isn't panicking. It is returning an error, and you are calling unwrap, which turns the error into a panic.

Globs match on arbitrary bytes, and therefore may match invalid utf-8. This is intentional. Therefore, you can't use the str based API. You need to use bytes::Regex instead. The documentation for this crate should explain this.

---

_Closed by @BurntSushi on 2018-07-19 10:31_

---

_Comment by @vadimcn on 2018-07-19 17:02_

Makes sense, sorry for the noise.   

Though, you might consider the fact that I honestly RTFM'ed and googled that error, for like an hour, before submitting this bug report.  Maybe worth considering adding a note to the description of [`Glob::regex()`](https://docs.rs/globset/0.4.0/globset/struct.Glob.html#method.regex) method?  
In fact, I did find the [paragraph](https://docs.rs/regex/1.0.2/regex/index.html#unicode) related to `(?-u)` option, but still failed to make connection to the error, because it reads as if `(?-u)` merely modifies matching behavior.

---

_Reopened by @BurntSushi on 2018-07-21 17:24_

---

_Label `doc` added by @BurntSushi on 2018-07-21 17:24_

---

_Comment by @BurntSushi on 2018-07-21 17:25_

@vadimcn Does the [next section on opting out of Unicode support](https://docs.rs/regex/1.0.2/regex/index.html#opt-out-of-unicode-support) resolve the confusion here?

(Also, consider what would happen if the `str` API allowed you to write a regex that matched invalid or incomplete UTF-8. The match offsets you'd get back from the API would in turn not be guaranteed to be valid slice offsets, and there would be no clear answer on how to extract matches as `&str`.)

I will update the docs in `globset`.

---

_Closed by @BurntSushi on 2018-07-21 17:25_

---
