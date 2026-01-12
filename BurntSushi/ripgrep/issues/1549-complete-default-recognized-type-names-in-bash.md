```yaml
number: 1549
title: Complete default recognized type names in Bash
type: issue
state: open
author: commonquail
labels:
  - enhancement
assignees: []
created_at: 2020-04-11T12:16:26Z
updated_at: 2023-11-21T20:57:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1549
synced_at: 2026-01-12T16:13:23Z
```

# Complete default recognized type names in Bash

---

_@commonquail_

#### What version of ripgrep are you using?

```
ripgrep 12.0.1 (rev 1143888259)
+SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Compile from source.

#### What operating system are you using ripgrep on?

Ubuntu 19.10

#### Describe your question, feature request, or bug.

I would like to be able to complete default recognized type names in Bash.

I compile ripgrep from source; mostly as a historical curiosity. Because I already compile from source I patch ripgrep's generated Bash completion to support type names: commonquail/ripgrep@1ce1701f9751bd743ca79b560254c04bb7063053:

```sh
$ rg -t j
java     jinja    jl       js       json     jsonl    julia    jupyter
```

That has been fairly useful to me, if [technically not quite worth it](https://xkcd.com/1205/).

Since the completion file is generated, it's not really feasible to propose that version for upstreaming. Fortunately, clap has built-in support for enumerating valid values and it's already being used a few places in ripgrep. Unfortunately, the type names seem very difficult to access at that point in time:
commonquail/ripgrep@36447f5f0b804e264ce13fbaa7aa4ab777fb55b0:

I appreciate that the generated completion scripts are _at best_ a secondary concern, included purely "because we can", so I understand not wanting to spend any time on this.

See also

- #1543

---

_Label `enhancement` added by @BurntSushi on 2023-11-21 00:59_

---

_Comment by @BurntSushi on 2023-11-21 01:01_

I don't have any plans of working on this, but once #2626 is merged, ripgrep will own the generation process for bash shell completions. Previously, it asked a dependency (the Clap crate) to do it. This _should_ make it much easier to augment the generation code to include all of the file types.

With that said, the ideal here would be for the completion script to use `rg --type-list` to determine the complete list of types. Since they can be added to (or removed) by the end user.

---

_Comment by @commonquail on 2023-11-21 20:57_

For Bash

```rs
impl Flag for Type {
    fn doc_choices(&self) -> &'static [&'static str] {
        &["$(rg --type-list | cut -d: -f1)"]
    }
}
```

is functionally equivalent to the modification I made. Nakedly shelling out this way would not work for Fish and PowerShell, of course, so some means of signalling dynamic generation would be necessary -- a richer return type or a new function definition, essentially.

`--type-list` could be taught a flag to omit the prefix and obviate `cut`, or an alternative argument (hidden?) could do the same to leave `--type-list` alone. This could allow an alternative design more specialized than arbitrarily shelling out, which may or may not be preferable.

---
