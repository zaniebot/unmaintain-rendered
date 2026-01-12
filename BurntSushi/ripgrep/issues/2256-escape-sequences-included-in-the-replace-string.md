```yaml
number: 2256
title: Escape sequences included in the replace string are not interpreted 
type: issue
state: closed
author: yuki-tsubaki
labels:
  - duplicate
  - wontfix
assignees: []
created_at: 2022-07-13T05:22:40Z
updated_at: 2022-07-13T10:06:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2256
synced_at: 2026-01-12T16:13:24Z
```

# Escape sequences included in the replace string are not interpreted 

---

_@yuki-tsubaki_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

I installed from the Arch Linux repositories (`pacman -S ripgrep`).

#### What operating system are you using ripgrep on?

Arch Linux (GNU/Linux).

#### Describe your bug.

Escape sequences included in the replacement string (`-r`), are not interpreted as the character they encode for in the output, and are instead included literally.
(I assume this is a bug, but it may actually be a feature request.)

#### What are the steps to reproduce the behavior?

Run `rg` using the `-r` option and giving a replacement string that contains an escape character.
In my case, running the following command that I was trying to use to parse my battery info with:

```bash
upower -d | rg -Uo -m 1 -e '\p{any}*battery\n\s*present:\s*(\w*)\p{any}*?state:\s*(\w*)\p{any}*?percentage:\s*(\d\d%)\p{any}*' -r 'Battery present: ${1}\nState: ${2}\nPercentage: ${3}'
```

Results in this:
`Battery present: yes\nState: charging\nPercentage: 59%`



#### What is the actual behavior?

Using the `--debug` flag and cat with a sample file that has the output of `upower -d`:
```bash
cat upower_dump | rg -Uo -m 1 --debug -e '\p{any}*battery\n\s*present:\s*(\w*)\p{any}*?state:\s*(\w*)\p{any}*?percentage:\s*(\d\d%)\p{any}*' -r 'Battery present: ${1}\nState: ${2}\nPercentage: ${3}'
```

Results in this:
```
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
Battery present: yes\nState: charging\nPercentage: 59%
```

#### What is the expected behavior?

I was expecting the output to substitute the proper characters for the escape sequences. Using `sd`, a Rust `sed` alternative, I was able to achieve the result I expected ripgrep to output.

Running the following command:

```bash
upower -d | sd -f ms '.*battery\n\s*present:\s*(\w*)\p{any}*?state:\s*(\w*)\p{any}*?percentage:\s*(\d\d%).*' 'Battery present: ${1}\nState: ${2}\nPercentage: ${3}'
```

Which results in:
```
Battery present: yes
State: charging
Percentage: 59%
```

---

_Comment by @BurntSushi on 2022-07-13 10:06_

This is behaving as documented and as intended.

See: https://github.com/BurntSushi/ripgrep/discussions/1953

See: https://github.com/BurntSushi/ripgrep/discussions/2253

Also, note for future issues, it is helpful to provide the smallest example of your issue. The one you've provided is quite large. :)

---

_Closed by @BurntSushi on 2022-07-13 10:06_

---

_Label `duplicate` added by @BurntSushi on 2022-07-13 10:06_

---

_Label `wontfix` added by @BurntSushi on 2022-07-13 10:06_

---
