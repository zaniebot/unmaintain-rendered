```yaml
number: 2445
title: "--replace breaks when concatenating placeholder and string (input provided)"
type: issue
state: closed
author: ink-splatters
labels:
  - duplicate
assignees: []
created_at: 2023-03-06T10:46:09Z
updated_at: 2023-03-06T12:24:52Z
url: https://github.com/BurntSushi/ripgrep/issues/2445
synced_at: 2026-01-12T16:13:24Z
```

# --replace breaks when concatenating placeholder and string (input provided)

---

_@ink-splatters_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

using nix 

`nix-env -iA nixpkgs.ripgrep`

#### What operating system are you using ripgrep on?

```
ProductName:            macOS
ProductVersion:         13.2.1
BuildVersion:           22D68
```
#### Describe your bug.

`--replace` breaks in a strange way when **a placeholder is concatenated with a string and  not vice versa: concatenating a string on the left side with placeholder on the right side - works** 

#### Context

Given:

```shell
ic@airstation ~ % which rg
/nix/var/nix/profiles/default/bin/rg
```
I want to replace `bin/rg` with `Applications`.

#### Working example

I came up with regex which does give correct output

```shell
ic@airstation ~ % which rg | rg '(default/).+'
/nix/var/nix/profiles/default/bin/rg
```
---

#### Bug

Things break when `--replace` argument is added to the call. As you see, the placeholder is postfixed with a string, which is important.

```shell
ic@airstation ~ % which rg | rg '(default/).+' --replace '$1Applications'
/nix/var/nix/profiles/
```

#### Expected output

`/nix/var/nix/profiles/default/Applications`

#### More examples

lets call things like that:
- `$1` is _placeholder_
- `Applications` is _string_

swap placeholder and string, **works**:

```shell
ic@airstation ~ % which rg | rg '(default/).+' --replace 'Applications $1'
/nix/var/nix/profiles/Applications default/
```

**works** even w/o space in between:

```shell
ic@airstation ~ % which rg | rg '(default/).+' --replace 'Applications$1'
/nix/var/nix/profiles/Applicationsdefault/
```

swap placeholder and 1-st string's character, **broken**:

```shell
ic@airstation ~ % which rg | rg '(default/).+' --replace 'A$1pplications'
/nix/var/nix/profiles/A
```

added one more capture group, **works**:

```shell
ic@airstation ~ % which rg | rg '(default/)(.+)' --replace '$1$2'
/nix/var/nix/profiles/default/bin/rg
```

added one more capture group, squeeze string in between, **broken**:

```shell
ic@airstation ~ % which rg | rg '(default/)(.+)' --replace '$1Applications$2'
/nix/var/nix/profiles/bin/rg
```




---

_Renamed from "--replace breaks when concatenating placeholder and string and not vice versa (input provided)" to "--replace breaks when concatenating placeholder and string (input provided)" by @ink-splatters on 2023-03-06 10:46_

---

_Comment by @BurntSushi on 2023-03-06 12:24_

The behavior is correct, but the docs are poor. #2108 tracks updating the docs. The docs basically need to pull in the docs from the regex crate capture expansion API, as that is what is used: https://docs.rs/regex/latest/regex/struct.Captures.html#method.expand

For your case, you should be using `${1}Applications`.

---

_Closed by @BurntSushi on 2023-03-06 12:24_

---

_Label `duplicate` added by @BurntSushi on 2023-03-06 12:24_

---
