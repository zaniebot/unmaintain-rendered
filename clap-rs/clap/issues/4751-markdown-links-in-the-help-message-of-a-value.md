```yaml
number: 4751
title: Markdown links in the help message of a value enum causes ZSH completion to error
type: issue
state: closed
author: KSXGitHub
labels:
  - C-bug
  - A-completion
  - E-easy
assignees: []
created_at: 2023-03-09T08:04:48Z
updated_at: 2023-04-20T20:53:03Z
url: https://github.com/clap-rs/clap/issues/4751
synced_at: 2026-01-12T16:14:16Z
```

# Markdown links in the help message of a value enum causes ZSH completion to error

---

_@KSXGitHub_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.67.1

### Clap Version

4.1.8

### Minimal reproducible code

The complete code is here: https://github.com/KSXGitHub/bug-report-clap-completion-when-help-contains-special-characters

### Steps to reproduce the bug with the above code

The complete steps to reproduce is also here: https://github.com/KSXGitHub/bug-report-clap-completion-when-help-contains-special-characters

### Actual Behaviour

The following error messages appear:

```
(eval):1: unmatched "
_arguments:465: command not found: (
(eval):1: unmatched "
_arguments:465: command not found: (
(eval):1: unmatched "
_arguments:465: command not found: (
```

### Expected Behaviour

Completion works normally.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @KSXGitHub on 2023-03-09 08:04_

---

_Label `A-completion` added by @epage on 2023-03-09 15:05_

---

_Label `E-easy` added by @epage on 2023-03-09 15:05_

---

_Comment by @changhc on 2023-04-08 19:52_

Hi @epage, I would like to try to fix this bug. I confirm that I can reproduce the bug with the sample code.

For now, should we just throw an error at compile time when there is markdown code in a help message, as @KSXGitHub suggested, instead of trying to escape anything? I need to look into this a bit more to understand the root cause, but at least for now I assume that it's highly likely that there will be edge cases missed out or at least cases that cannot be handled, e.g., links to images, if we try to transform the markdown code. What do you think?

---

_Comment by @KSXGitHub on 2023-04-09 04:04_

For edge cases, I think just emit an error is sufficient. If the user wants to preserve the markdown documentation, they may specify `#[clap(help)]` attribute (which should be hinted at in the error message). 

---

_Comment by @KSXGitHub on 2023-04-09 04:10_

BTW, all the transformation and error message should be done at compile time, in the derive macros, because it is highly unlikely that a user would make the silly mistake of putting markdown code in the help message if they are using the arg builder directly.

---

_Comment by @changhc on 2023-04-09 21:21_

So you are still in favour of at least converting hyperlinks? I'm not sure if it would lead to more confusion due to inconsistency. When an error is thrown and users are told that the help message with markdown is invalid, they might wonder why hyperlinks work but other markdown syntaxes don't.
Anyway, I'll try to throw an error when markdown is detected in a help message. We figure out if we want to allow any specific case later.

---

_Comment by @KSXGitHub on 2023-04-10 02:17_

I think the user can understand that the terminal has its own limitations. If the error message explains that the terminal is incapable of displaying something, then there would be no confusion.

---

_Comment by @epage on 2023-04-10 15:22_

Any erroring off of content of the help message would be a breaking change.  I also do not want to get into the business of trying to outguess the user for what they can and cannot include in their help message.

We should make sure we are properly escaping.

---

_Referenced in [clap-rs/clap#4848](../../clap-rs/clap/pulls/4848.md) on 2023-04-19 23:18_

---

_Closed by @epage on 2023-04-20 20:53_

---
