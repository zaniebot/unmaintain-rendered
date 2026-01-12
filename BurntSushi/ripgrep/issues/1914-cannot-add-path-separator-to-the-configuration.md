```yaml
number: 1914
title: Cannot add --path-separator // to the configuration file
type: issue
state: closed
author: aoloe
labels:
  - invalid
assignees: []
created_at: 2021-06-28T09:04:23Z
updated_at: 2021-06-29T05:48:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1914
synced_at: 2026-01-12T16:13:24Z
```

# Cannot add --path-separator // to the configuration file

---

_@aoloe_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Release file from Github


#### What operating system are you using ripgrep on?

Windows 10


#### Describe your bug.

I cannot set the path separator to / from the configuration file.

Using `--path-separator //` directly in the command line does work as expected.

If I add other arguments from the Configuration section in the README (and comment out the `--path-separator`) the configuration file does not produce any error.

#### What are the steps to reproduce the behavior?

My `~/.ripgrep` files contains the only line:

```
--path-separator //
```

If I run ripgrep, i get the error:

```
error: Found argument '--path-separator //' which wasn't expected, or isn't valid in this context
        Did you mean --path-separator?

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
```

#### What is the actual behavior?

```
$ rg --debug test app
DEBUG|rg::config|crates/core\config.rs:40: P:/.ripgrep: arguments loaded from config file: ["--path-separator //"]
DEBUG|rg::args|crates/core\args.rs:543: final argv: ["P:\\bin\\rg", "--path-separator //", "--debug", "test", "app"]
error: Found argument '--path-separator //' which wasn't expected, or isn't valid in this context
        Did you mean --path-separator?

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
```

#### What is the expected behavior?

ripgrep should have shown me the occurrences of test in the app directory, and use `/` for the paths.


---

_Comment by @BurntSushi on 2021-06-28 11:25_

As documented in the man page, _every line is a distinct argument_:

https://github.com/BurntSushi/ripgrep/blob/abf115228e886494e6198a64c519db7285ff29af/doc/rg.1.txt.tpl#L189

As the examples further below in the docs show, you need to do either

```
--path-separator=/
```

or

```
--path-separator
/
```

Also, given that this is in a config file and not in a shell, I suspect you want `/` and not `//`.

---

_Closed by @BurntSushi on 2021-06-28 11:25_

---

_Label `invalid` added by @BurntSushi on 2021-06-28 11:26_

---

_Comment by @aoloe on 2021-06-29 05:48_

Thanks for having a look into it.

Indeed, I missed the detail about the equal sign.
After so many years using the shell, I'm still confused by all the combinations that exist for defining the command line arguments!
I was so focused on getting the `/` to be correctly recognized in a Windows environment (which is the most unusual part for me) that I forgot to check that part.

---
