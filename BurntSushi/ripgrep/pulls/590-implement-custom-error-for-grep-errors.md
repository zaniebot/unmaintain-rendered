```yaml
number: 590
title: Implement custom error for grep errors
type: pull_request
state: closed
author: andrea-prearo
labels: []
assignees: []
base: master
head: error-parsing-message
created_at: 2017-08-30T20:03:24Z
updated_at: 2019-02-04T15:45:48Z
url: https://github.com/BurntSushi/ripgrep/pull/590
synced_at: 2026-01-12T18:23:13Z
```

# Implement custom error for grep errors

---

_@andrea-prearo_

This PR tries to address the issues with error messages returned by [regex](https://github.com/rust-lang/regex):
- Issue https://github.com/BurntSushi/ripgrep/issues/478
- Issue https://github.com/BurntSushi/ripgrep/issues/395

It does that by implementing a custom error to wrap such errors and truncating them before the portion that contains all the details about the AST (` at character ...`).

For instance, the output for the example provided in Issue https://github.com/BurntSushi/ripgrep/issues/478 is:
```sh
$ rg -e foo -f src/search_stream.rs 
Regex syntax error caused by: Error parsing regex near 'fer.|*/|z{'
```

Here are other few examples of error messages:
```sh
$ rg "s{"
Regex syntax error caused by: Error parsing regex near '\s{'
```
```sh
$ rg "a*{"
Regex syntax error caused by: Error parsing regex near 'a*{'
```


---

_Comment by @BurntSushi on 2017-08-30 20:58_

Hmmm... Thanks for trying to fix this! Unfortunately, I'm not sure this is quite the right path to take here. By implementing a custom error, I meant using the regex parser directly and then implementing a different error message than the one produced by `regex-syntax` directly: https://github.com/rust-lang/regex/blob/32eb9643b45bebd2ce5169e75daed6e8579e74b2/regex-syntax/src/lib.rs#L1549 --- Basically, do the same case analysis, but don't try to print any sub-expressions in the error message.

There are a couple problems with this PR as is. Firstly, the error message truncation ends up cutting off important pieces of the message. For example, `rg 's{'` used to be

```
$ rg 's{'
Error parsing regex near 's{' at character offset 2: Missing maximum in counted
repetition operator.
```

but now it's

```
$ rg 's{'                                                                                                                                                                                     
Regex syntax error caused by: Error parsing regex near 's{'
```

Personally, I think the unpleasant corner case isn't worth dropping the extra information for all regex parse errors.

The other issue I have with this PR as-is is that it's reporting less information to the caller by translating the error to a much less structured form. I'd expect to see this fixed by retaining a structured error message and then fixing the output.

At some point, I'd like to fix this for real by using a better regex parser/pretty printer.

---

_Comment by @andrea-prearo on 2017-08-30 21:17_

It makes perfect sense. I am going to close this PR.
If nobody is working on tweaking the message produced by `regex-syntax`, I may look into that.
Thanks!

---

_Closed by @andrea-prearo on 2017-08-30 21:20_

---

_Branch deleted on 2017-08-30 21:20_

---

_Comment by @BurntSushi on 2017-08-30 21:40_

@andrea-prearo Yeah I think a PR to the regex crate that removes the sub-expressions from the messages would be great!

---

_Comment by @andrea-prearo on 2017-08-30 21:52_

@BurntSushi Sounds good. I'll look into that.

---
