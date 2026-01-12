```yaml
number: 1368
title: support empty alternate glob patterns
type: issue
state: open
author: StaticPH
labels:
  - enhancement
assignees: []
created_at: 2019-09-07T22:41:49Z
updated_at: 2020-05-08T11:35:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1368
synced_at: 2026-01-12T16:13:23Z
```

# support empty alternate glob patterns

---

_@StaticPH_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Installed with cargo

#### What operating system are you using ripgrep on?

Windows 8.1 Pro (x86_64)
If it's relevant, I'm also using the latest version of msys2-x86_64

#### Describe your question, feature request, or bug.

Bash and ripgrep evaluate globs differently, which seems like a bug to me.
If this behavior is actually intended, I'd like help finding a glob pattern that will give the result bash does for my test pattern.

#### If this is a bug, what are the steps to reproduce the behavior?

In a directory I have 5 files containing only the string 'qwertyuiop'
The files are named: 
`'.foo'  '.foo_baz'  'bar.foo'  'baz.foo_bar'  'SHOULD_NOT_BE_GLOBBED'`

I do not have any .ignore-like files, am not instructing ripgrep to use a configuration file, and have not set `RIPGREP_CONFIG_PATH`

The glob pattern I am using for this test is `{*,}.foo{_*,}`
The regex is simply "\D", which should match every character in the 5 files

#### If this is a bug, what is the actual behavior?

See https://gist.github.com/StaticPH/c52d9ddee8a1a43a23196f5f46f1503e
#### If this is a bug, what is the expected behavior?

Simply calling `echo {*,}.foo{_*,}` in bash prints out `baz.foo_bar bar.foo .foo_baz .foo`, so I would have expected both
```
~/.cargo/bin/rg -uu --type-add 'foo: {*,}.foo{_*,}' -tfoo "\D"
and
~/.cargo/bin/rg -uu -g '{*,}.foo{_*,}' "\D"
```
to output
```
.foo_baz
1:qwertyuiop

.foo
1:qwertyuiop

bar.foo
1:qwertyuiop

baz.foo_bar
1:qwertyuiop
```
and for 
```
~/.cargo/bin/rg -uu --type-add 'foo:{*,}.foo{_*,}' -Tfoo --debug "\D"
```
to output 
```
SHOULD_NOT_BE_GLOBBED
1:qwertyuiop
```

---

_Comment by @StaticPH on 2019-09-07 22:45_

Possibly related to globbing behavior mentioned in https://github.com/BurntSushi/ripgrep/issues/1221 ?

---

_Comment by @okdana on 2019-09-07 23:02_

That ticket seems only tangentially related. I think your problem here is related to the fact that ripgrep just drops empty alternates, so where you're expecting for example `{_*,}` to be converted to something like `(_[^/]*|)` (or technically `(_[^/]*)?`, since the underlying regex engine doesn't support empty alternates itself), it's actually converting it to `_[^/]*`

---

_Renamed from "ripgrep's globbing doesn't match that of bash" to "support empty alternate glob patterns" by @BurntSushi on 2020-05-08 11:35_

---

_Label `enhancement` added by @BurntSushi on 2020-05-08 11:35_

---
