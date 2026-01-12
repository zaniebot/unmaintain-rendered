```yaml
number: 816
title: When using --hidden and PATH, ignored subdirs of .hidden shown if hidden path part of ignored-string
type: issue
state: closed
author: unhammer
labels:
  - bug
  - duplicate
assignees: []
created_at: 2018-02-18T09:43:09Z
updated_at: 2018-02-20T12:43:43Z
url: https://github.com/BurntSushi/ripgrep/issues/816
synced_at: 2026-01-12T16:13:22Z
```

# When using --hidden and PATH, ignored subdirs of .hidden shown if hidden path part of ignored-string

---

_@unhammer_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.8.0 (rev 23d1b91ead)
+SIMD -AVX
```

#### What operating system are you using ripgrep on?

Xubuntu 17.10

#### Describe your question, feature request, or bug.

The behaviour of --hidden when using fully specified ignore-paths is
confusing, possibly a bug since it's inconsistent with
partially-specified ignore-paths (or I'm not sure what the intended
behaviour is).

In short, it seems that ignored subdirectories of hidden directories
are shown if they are specified in `.gitignore` prefixed with the
hidden directory *and* a PATH argument is given to rg.

#### If this is a bug, what are the steps to reproduce the behavior?

Download and unzip
[repro.zip](https://github.com/BurntSushi/ripgrep/files/1734559/repro.zip)
where `.gitignore` contains

    /.hidden/ignored
    relativelyignored

and the reproduction script `repro.sh` contains

    rg -l          -- '[k]ake'   # as expected
    rg -l          -- '[k]ake' . # as expected
    rg -l --hidden -- '[k]ake'   # as expected
    rg -l --hidden -- '[k]ake' . # wat

where the last command has the unexpected output.

#### If this is a bug, what is the actual behavior?

    $ rg --debug -l --hidden -- '[k]ake' .
    DEBUG/grep::search/grep/src/search.rs:195: regex ast:
    Concat(
        [
            Class(
                CharClass {
                    ranges: [
                        ClassRange {
                            start: 'k',
                            end: 'k'
                        }
                    ]
                }
            ),
            Literal {
                chars: [
                    'a',
                    'k',
                    'e'
                ],
                casei: false
            }
        ]
    )
    DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(kake)], limit_size: 250, limit_class: 10 }
    DEBUG/globset/globset/src/lib.rs:401: built glob set; 1 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    file
    .hiddenfile
    DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./.hidden/relativelyignored: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "relativelyignored", actual: "**/relativelyignored", is_whitelist: false, is_only_dir: false })))
    shown/file
    .hidden/file
    .hidden/ignored/file

#### If this is a bug, what is the expected behavior?

I would've expected ignored directories to have stayed ignored even
though they're inside hidden directories even if I specified --hidden
(I rarely if ever want to make any distinction between hidden and
non-hidden directories as regards `rg`, I use `.gitignore` or
`.ignore` for when I want `rg` to not show things).

Note that, confusingly, the file inside `relativelyignored` inside
`.hidden` is *not* shown even when using both `--hidden` and a PATH.


---

_Renamed from "When using --hidden and PATH, ignored subdirs of .hidden shown iff full path ignored" to "When using --hidden and PATH, ignored subdirs of .hidden shown if hidden path part of ignored-string" by @unhammer on 2018-02-18 09:43_

---

_Comment by @okdana on 2018-02-18 09:50_

I think this is the same as #807, which was fixed just the other day. `rg` built from current `master` produces this output:

```
% \rg -j1 -l '[k]ake'
file
shown/file
% \rg -j1 -l '[k]ake' .
file
shown/file

% \rg -j1 -l --hidden '[k]ake'
.hidden/file
.hiddenfile
file
shown/file
% \rg -j1 -l --hidden '[k]ake' .
.hidden/file
.hiddenfile
file
shown/file
```

---

_Comment by @unhammer on 2018-02-18 10:03_

Oh, somehow I missed that issue when looking :-S It seems from your testing that this can be closed (I haven't got a rust env here so I'll take your word for it). Sorry for the noise!

---

_Closed by @unhammer on 2018-02-18 10:03_

---

_Label `bug` added by @BurntSushi on 2018-02-20 12:43_

---

_Label `duplicate` added by @BurntSushi on 2018-02-20 12:43_

---
