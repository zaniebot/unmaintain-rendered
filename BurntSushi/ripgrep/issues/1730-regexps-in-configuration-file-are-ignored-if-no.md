```yaml
number: 1730
title: Regexps in configuration file are ignored if no other regexp is present in the command line
type: issue
state: closed
author: cyrus-and
labels:
  - doc
assignees: []
created_at: 2020-11-15T01:35:50Z
updated_at: 2020-11-15T20:40:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1730
synced_at: 2026-01-12T16:13:24Z
```

# Regexps in configuration file are ignored if no other regexp is present in the command line

---

_@cyrus-and_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
brew install ripgrep
```

#### What operating system are you using ripgrep on?

macOS 11.0.1

#### Describe your bug.

If regexps (`--regexp`) are **exclusively** specified in the configuration file, then they are ignored.

#### What are the steps to reproduce the behavior?

```
$ cat /tmp/.ripgreprc
--regexp
foo
$ echo 'xxx foo xxx' | RIPGREP_CONFIG_PATH=/tmp/.ripgreprc rg
```

#### What is the actual behavior?

```
$ echo 'xxx foo xxx' | RIPGREP_CONFIG_PATH=/tmp/.ripgreprc rg --debug
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
```

#### What is the expected behavior?

```
xxx foo xxx
```

(With `foo` being highlighted.)

---

_Comment by @BurntSushi on 2020-11-15 01:37_

Could you please explain why you want to do this?

---

_Comment by @cyrus-and on 2020-11-15 01:42_

I'm using ripgrep as a *backend* for some script and I'd like to pass all the arguments/configuration in the same way. Using the configuration file seems more elegant than passing individual parameters on the command line which (albeit unlikely nowadays) is subject to limitation.

---

_Label `doc` added by @BurntSushi on 2020-11-15 19:58_

---

_Closed by @BurntSushi on 2020-11-15 20:05_

---

_Comment by @BurntSushi on 2020-11-15 20:05_

Thanks for clarifying. Ultimately, this would be difficult to fix and I don't think your use case is worth it. I updated the man page's section on config files to clarify this case.

---

_Comment by @cyrus-and on 2020-11-15 20:20_

I see your point and admittedly I'm not aware of the internals, but still this somewhat smells fishy (no disrespect) for a number of reason, e.g., the fact that the usage hints that `rg` can run without arguments:

    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]

And also that it can be worked around with something like (for the sake of the argument):

```console
$ echo 'xxx foo xxx' | RIPGREP_CONFIG_PATH=/tmp/.ripgreprc rg -e '$x'
xxx foo xxx
```

Anyway thanks for such a quick action and for this project!

---

_Comment by @BurntSushi on 2020-11-15 20:40_

Ah, yes, the usage hints in the `-h/--help` output appear to have gotten out of sync with the man page. The man page has the correct usage hints. I've fixed that.

The usage hints were also written long before the config file functionality existed. So just running `rg` with no arguments never really made sense. There wouldn't be anything for `rg` to do. And even with support for config files, this is a rather extreme edge case. Configuration files were never really intended to be a way to supply 100% of all arguments to ripgrep. They are intended to solve the problem of passing common arguments. The only reason why this sort of thing even looks possible is because of the config file format: it's just CLI flags. If the config format were, say, a Yaml file, there wouldn't be any expectation that this would work I think.

---
