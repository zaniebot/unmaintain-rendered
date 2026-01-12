```yaml
number: 1048
title: "--color parameter IN A CONFIG FILE raises error on run"
type: issue
state: closed
author: OmanF
labels: []
assignees: []
created_at: 2018-09-12T10:33:06Z
updated_at: 2018-09-12T10:47:07Z
url: https://github.com/BurntSushi/ripgrep/issues/1048
synced_at: 2026-01-12T16:13:22Z
```

# --color parameter IN A CONFIG FILE raises error on run

---

_@OmanF_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Using `brew`.

#### What operating system are you using ripgrep on?

macOS High Sierra (10.13.6)

#### Describe your question, feature request, or bug.

Using the `--color` directive in a config file raises an error when trying to run `rg` claiming the argument `--color` wasn't expected or isn't valid in that context.

#### If this is a bug, what are the steps to reproduce the behavior?

I've set a config file according to official guide.
On that file I put: `--color always`.
I then run `rg <pattern> <location>`.

#### If this is a bug, what is the actual behavior?

```
DEBUG|rg::config|src/config.rs:37: /Users/ohadfk/.ripgreprc: arguments loaded from config file: ["--color always", "--smart-case"]
DEBUG|rg::args|src/args.rs:508: final argv: ["rg", "--color always", "--smart-case", "--debug", "io.puts", "ticker"]
error: Found argument '--color always' which wasn't expected, or isn't valid in this context
	Did you mean --color?

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
```

#### If this is a bug, what is the expected behavior?

I expect the query to run correctly.

#### Note

* Commenting out the `--color always` argument, the `--smart-case` argument works correctly.
* The incorrect behavior happens no matter which of the *valid* values for the `--color` is used (e.g. `always`, `never`, `ansi`).
* Using `--color` *WITHOUT* specifying a value gives the (correct) error that this parameter takes 1 value but was passed 0.
* Using `--color <val>` as a command-line parameter (with the rest of the config file *still in play*) also works correctly.


---

_Comment by @BurntSushi on 2018-09-12 10:43_

The error message isn't great, but this isn't a bug. Please consider [reading the docs on config files](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file). In particular:

> When we use a flag that has a value, we either put the flag and the value on the same line but delimited by an = sign (e.g., `--max-columns=150`), or we put the flag and the value on two different lines. This is because ripgrep's argument parser knows to treat the single argument `--max-columns=150` as a flag with a value, but if we had written `--max-columns 150` in our configuration file, then ripgrep's argument parser wouldn't know what to do with it.
>
> Putting the flag and value on different lines is exactly equivalent and is a matter of style.

---

_Closed by @BurntSushi on 2018-09-12 10:43_

---

_Comment by @OmanF on 2018-09-12 10:47_

Yes, sorry for missing the part about formatting the file.
It works correctly.

---
