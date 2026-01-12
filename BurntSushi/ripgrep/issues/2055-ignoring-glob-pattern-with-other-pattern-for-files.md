```yaml
number: 2055
title: Ignoring glob pattern with other pattern for files…
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2021-11-10T16:16:41Z
updated_at: 2021-11-10T16:44:23Z
url: https://github.com/BurntSushi/ripgrep/issues/2055
synced_at: 2026-01-12T16:13:24Z
```

# Ignoring glob pattern with other pattern for files…

---

_@ghost_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
dnf install 
ripgrep-13.0.0-1.fc34.x86_64
````

#### What operating system are you using ripgrep on?

```
Linux nightwatch 5.14.16-201.fc34.x86_64 #1 SMP Wed Nov 3 13:57:29 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
```

#### Describe your bug.

Glob is only ignored if I do not glob files.

Note that I am using `zsh`, thus the escaped `!`.

#### What are the steps to reproduce the behavior?

```
√ ; mkdir ook eek monkey
√ ; echo "test" > (ook|eek|monkey)/file
√ ; rg "test"
monkey/file
1:test

eek/file
1:test

ook/file
1:test
```

#### What is the actual behavior?

This is the expected behaviour with no extra filtering.

```
√ ; rg "test" -g "\!ook"
monkey/file
1:test

eek/file
1:test
```

This is what happens when I ask `rg`  to look at all the files called `file`. Yes, this is a simple case.

```
√ ; rg "test" -g "\!ook" **/file
ook/file
1:test

monkey/file
1:test

eek/file
1:test

√ ; rg "test" **/file -g "\!ook"
ook/file
1:test

monkey/file
1:test

eek/file
1:test
```

#### What is the expected behavior?

I expected anything in the `ook` to be ignored despite it matching the second glob.

I used a similar command on a large code base and want to search only golang files but not those in `vendor/` so what I want is this:

```
rg -g "\!vendor" "someName" **/*.go
```

Which does not ignore anything in `vendor/`…


---

_Comment by @BurntSushi on 2021-11-10 16:27_

This is correct and expected behavior. If you give a file path to ripgrep on the CLI, then ripgrep always searches it, regardless of any other rules.

If you want to, for example, "only search golang files but not those in `vendor`," then you could do:

```
rg 'someName' -tgo -g '!vendor'
```

Or you could use `find` to construct the list of files you want to search.

---

_Comment by @ghost on 2021-11-10 16:44_

Right, it was me not understanding. Thank you for your help!

`rg 'someName' -tgo -g '!vendor'` does indeed do what I need.

---

_Closed by @ghost on 2021-11-10 16:44_

---
