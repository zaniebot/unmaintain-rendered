```yaml
number: 2201
title: Some characters placed just after capture groups break replaced output
type: issue
state: closed
author: tamwile
labels:
  - doc
  - rollup
assignees: []
created_at: 2022-05-08T00:25:17Z
updated_at: 2023-11-25T20:03:59Z
url: https://github.com/BurntSushi/ripgrep/issues/2201
synced_at: 2026-01-12T16:13:24Z
```

# Some characters placed just after capture groups break replaced output

---

_@tamwile_

#### What version of ripgrep are you using?

`rg --version

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)`

#### How did you install ripgrep?

cargo install

#### What operating system are you using ripgrep on?

Archlinux 5.17.4-arch1-1

#### Describe your bug.

When we use the replace flag -r, some characters seem to break the replaced result 
if they are placed just after a capture group like $1.
For example underscore (_).

#### What are the steps to reproduce the behavior?

Run 
`ls file.mkv | rg '(.*).mkv' -r '$1_en.ass'`
Here, _ is just after the 1 in '$1', and it will break the output.
This also happen if we have any alphabet letter after the 1.

#### What is the actual behavior?

Here are some commands i tried.

`
ls file.mkv | rg '(.*).mkv' -r '$1_en.ass'`
`.ass`
The $1 is ignored and _ has apparently eaten the "en" part of the pattern.

`ls file.mkv | rg '(.*).mkv' -r '$1$_en.ass'`
`file.ass`
Since $$ is used to escape $, i tried to write $_, 
and while it still eats the "en" part, the $1 is correctly replaced this time.

`ls file.mkv | rg '(.*).mkv' -r '$1 en.ass'`
`file en.ass`
Here we can see that the output is correct if a space follow the 1, this is also the case for ., !, and probably others.

#### What is the expected behavior?

The expected behavior to the first command listed above is : 
`file_en.ass`


I think this is a bug, but if this is intentional, then please show me where in the doc i could have find this, because i have not.
Is there a way to escape the capture group and/or find a way for underscore to not break the output ?


---

_Comment by @BurntSushi on 2022-05-08 00:53_

The behavior is correct. The docs sadly do not mention the `${1}` syntax. It's [documented in the regex engine](https://docs.rs/regex/latest/regex/struct.Captures.html#method.expand), but you shouldn't need to dig into that. Specifically, this bit:

> The longest possible name consisting of the characters `[_0-9A-Za-z]` is used. e.g., `$1a` looks up the capture group named 1a and not the capture group at index 1. To exert more precise control over the name, or to refer to a capture group name that uses characters outside of `[_0-9A-Za-z]`, use braces, e.g., `${1}a` or `${foo[bar].baz}`. When using braces, any sequence of characters is permitted. If the sequence does not refer to a capture group name in the corresponding regex, then it is replaced with an empty string.

---

_Label `doc` added by @BurntSushi on 2022-05-08 00:53_

---

_Comment by @tamwile on 2022-05-08 01:42_

Thanks, this totally answers my question.
I leave the issue open until the doc is updated.


---

_Comment by @brunetton on 2023-03-16 18:40_

Sorry for my duplicate in #2455 

It came to my mind that this might be this kind of problem; and I've to admit: I didn't go deep enough into the doc to find this section. **But**, IMHO (very humble), it doesn't seem very logical to me that the priority is given to users that use named groups over users (97% I suppose) that don't.

I expected that to match `$1_en.ass` one should use `${1_en.ass}`, and `$1_en.ass` to be equivalent to `${1}_en.ass`

---

_Comment by @BurntSushi on 2023-03-16 18:56_

Yes, if I could go back in time, I might tweak these rules. But I don't feel comfortable making a breaking change of this sort at this point. The problem is that it would be a silent breaking change. It would silently change the semantics of replacement strings that previously worked.

---

_Label `rollup` added by @BurntSushi on 2023-11-24 20:03_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---
