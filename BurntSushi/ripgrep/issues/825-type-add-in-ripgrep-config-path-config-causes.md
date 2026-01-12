```yaml
number: 825
title: "--type-add in RIPGREP_CONFIG_PATH config causes error"
type: issue
state: closed
author: patbl
labels:
  - invalid
assignees: []
created_at: 2018-02-20T07:01:56Z
updated_at: 2018-02-20T11:56:58Z
url: https://github.com/BurntSushi/ripgrep/issues/825
synced_at: 2026-01-12T16:13:22Z
```

# --type-add in RIPGREP_CONFIG_PATH config causes error

---

_@patbl_

#### What version of ripgrep are you using?

ripgrep 0.8.0 (rev )
-SIMD -AVX

#### What operating system are you using ripgrep on?

macOS 10.13.3

#### If this is a bug, what are the steps to reproduce the behavior?

`--type-add` seems not to work when specified using `RIPGREP_CONFIG_PATH`.

```
rg --type-add 'rails:include:ruby' foo
```

works fine. But this doesn't:

```
➜ cat ~/.rgrc
--type-add 'rails:include:ruby'
➜ RIPGREP_CONFIG_PATH=$HOME/.rgrc rg --debug foo
DEBUG/rg::config/src/config.rs:42: /Users/pat/.rgrc: arguments loaded from config file: ["--type-add \'rails:include:ruby\'"]
DEBUG/rg::args/src/args.rs:143: final argv: ["rg", "--type-add \'rails:include:ruby\'", "--debug", "foo"]
error: Found argument '--type-add 'rails:include:ruby'' which wasn't expected, or isn't valid in this context
        Did you mean --type-add?

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list

For more information try --help
```

#### If this is a bug, what is the expected behavior?

I'd have expected `--type-add` in the config file to behave the same as it does when it's specified inline. Also, the error is not helpful ("Did you mean --type-add?").

---

_Comment by @okdana on 2018-02-20 10:04_

The config-file format has two limitations (i guess you could call them):

1. Each line represents one element of `argv` — it is not possible to put multiple white-space-separated elements on a single line

2. There is no handling of quotes or escapes like you might expect from the shell or from some other config-file formats

So there are two ways you can write this:

```sh
# Single argv element with =
--type-add=rails:include:ruby

# Multiple argv elements separated by new-line
--type-add
rails:include:ruby
```

---

_Comment by @BurntSushi on 2018-02-20 11:56_

Thanks @okdana!

@patbl This exact issue is discussed explicitly in the documentation: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file

---

_Closed by @BurntSushi on 2018-02-20 11:56_

---

_Label `invalid` added by @BurntSushi on 2018-02-20 11:56_

---
