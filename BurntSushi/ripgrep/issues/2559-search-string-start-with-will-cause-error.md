```yaml
number: 2559
title: "Search string start with \"->\" will cause error."
type: issue
state: closed
author: lonelyeagle
labels:
  - invalid
assignees: []
created_at: 2023-07-17T19:07:08Z
updated_at: 2023-07-17T19:56:34Z
url: https://github.com/BurntSushi/ripgrep/issues/2559
synced_at: 2026-01-12T16:13:24Z
```

# Search string start with "->" will cause error.

---

_@lonelyeagle_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Homebrew on mac os

#### What operating system are you using ripgrep on?
MacOS 13.4.1 also happens on Windows 10

#### Describe your bug.
Search text start with "->" will cause error.

#### What are the steps to reproduce the behavior?

If I want to search a function call in a C++ project.
I use  
rg -g "*.cpp" -F "->getPlaceholder()"

The terminal will return with an error.

If the corpus is too big and you cannot decrease its size, file the bug anyway
and the ripgrep maintainers will help figure out next steps.

#### What is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

If the output is large, put it in a gist: https://gist.github.com/

`rg -g "*.cpp" -F "->call"
error: Found argument '->' which wasn't expected, or isn't valid in this context

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] -e PATTERN ... [PATH ...]
    rg [OPTIONS] -f PATTERNFILE ... [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN
    rg [OPTIONS] --help
    rg [OPTIONS] --version

For more information try --help`

#### What is the expected behavior?

What do you think ripgrep should have done?


---

_Comment by @VladimirMarkelov on 2023-07-17 19:14_

It is the way how arguments are processed. Everything that starts with `-` is an option, That is why it fails. E.g, `grep "->call"` fails with the similar error `Invalid option -- '>'`.

To make it work, you should escape `-` if it is the first character of a regex. In `bash` and `powershell` you can do this:

```
rg "\->call"
```

Edit: though, this escaping does not work for me in case of `-F` is used. Without `-F` everything works great.

---

_Comment by @BurntSushi on 2023-07-17 19:56_

This is mentioned in the man page under the "POSITIONAL ARGUMENTS" section:

> A regular expression used for searching. To match a pattern beginning with a dash, use the `-e/--regexp` option.

So, use `rg -e '->call'` or `rg -F -e '->call'`. There are a variety of ways to work around this, but `-e -` is the standard way that always works and doesn't require modifying the regex pattern.

---

_Closed by @BurntSushi on 2023-07-17 19:56_

---

_Label `invalid` added by @BurntSushi on 2023-07-17 19:56_

---
