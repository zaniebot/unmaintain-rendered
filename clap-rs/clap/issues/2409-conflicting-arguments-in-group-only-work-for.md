```yaml
number: 2409
title: Conflicting arguments in group only work for first group in YAML file
type: issue
state: closed
author: DocKDE
labels:
  - C-bug
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-03-12T16:01:50Z
updated_at: 2021-12-08T20:01:18Z
url: https://github.com/clap-rs/clap/issues/2409
synced_at: 2026-01-12T16:14:13Z
```

# Conflicting arguments in group only work for first group in YAML file

---

_@DocKDE_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.50.0

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```
name: pdbman
version: "0.1"
author: Me
about: Does stuff

settings: [DeriveDisplayOrder, ArgRequiredElseHelp, ColoredHelp, ColorAlways]
    # - UnifiedHelpMessage
    # - DeriveDisplayOrder

args:
    - INPUT:
        about: Name of PDB file
        required: true
    - Query:
        about: Query mode
        long: query
        short: Q
    - Analyze:
        about: Analysis mode
        long: analyze
        short: Y
    - Add:
        about: Add mode
        long: add
        short: A
    - Remove:
        about: Remove mode
        long: remove
        short: R
    - Residues:
        about: Residue mode
        long: residues
        short: r
    - Atoms:
        about: Atom mode
        long: atoms
        short: t
    - Sidechain:
        about: Use only sidechains of protein
        long: sidechain
        short: d
        requires:
            - Residues
    - Backbone:
        about: Use only backbones of protein
        long: backbone
        short: b
        requires:
            - Residues
    - QM1:
        about: QM1 region
        long: qm1
        short: q
    - QM2:
        about: QM1 region
        long: qm2
        short: o
    - Active:
        about: Active revion
        long: active
        short: a
    - Infile:
        about: File for list input
        long: file
        short: f
        takes_value: true
    - List:
        about: Command line list
        long: list
        short: l
        takes_value: true
        multiple: true
        use_delimiter: true
    - Sphere:
        about: Calculate sphere around atom. Requires Atom ID and radius in A as arguments.
        long: sphere
        short: s
        takes_value: true
        min_values: 2
        max_values: 2
        conflicts_with:
            - Sidechain
            - Backbone
    - Outfile:
        about: Name of PDB output file
        long: outfile
        short: e
        takes_value: true
    - Overwrite:
        about: Overwrite input PDB file
        long: overwrite
        short: w
  
groups:
    - mode:
        args: [Query, Analyze, Add, Remove]
        required: true
    - partial:
        args: [Sidechain, Backbone]
    - target:
        args: [Atoms, Residues]
    - region:
        args: [QM1, QM2, Active]
    - source:
        args: [File, List, Sphere]
    - output:
        args: [Outfile, Overwrite]
```


### Steps to reproduce the bug with the above code

Use this .yaml for any command line parsing.

### Actual Behaviour

When calling the binary like `pdbman -QY` it fails with `error: The argument '--query' cannot be used with '--analyze'`. However the same is not true if I call multiple arguments from other groups like so: `pdbman -Aqo` or `pdbman -Qrt`. These commands run without error. It appears that only the args from the topmost group in the YAML file are recognized as being mutally exclusive. I verified this by changing their order.

### Expected Behaviour

I would expect a command calling arguments from the same group to also fail with this: `error: The argument '--residues' cannot be used with '--atoms'` if they belong to the same group.


### Additional Context

_No response_

### Debug Output

_No response_



---

_Label `T: bug` added by @DocKDE on 2021-03-12 16:01_

---

_Comment by @pksunkara on 2021-03-12 20:04_

Need the command which is failing and the debug output.

---

_Closed by @pksunkara on 2021-03-12 20:04_

---

_Reopened by @pksunkara on 2021-03-12 20:04_

---

_Comment by @pksunkara on 2021-03-12 20:05_

Basically, the "actual behaviour" sections need actual command runs and their outputs. 

---

_Comment by @DocKDE on 2021-03-12 20:37_

I updated the section for expected and actual behaviour. Although I must add I'm not sure what you mean by failing commands and debug output. My issue is precisely that there is no failing command when I expected it so there's no debug output I can post.

---

_Comment by @pksunkara on 2021-03-13 05:32_

What you wrote now is good. 

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-03-13 05:32_

---

_Label `C: yaml parser` added by @pksunkara on 2021-03-13 05:32_

---

_Label `D: easy` added by @pksunkara on 2021-03-13 05:32_

---

_Comment by @pksunkara on 2021-05-31 23:40_

@DocKDE Can you please try with the latest master now?

---

_Referenced in [epage/clapng#182](../../epage/clapng/issues/182.md) on 2021-12-06 21:13_

---

_Comment by @epage on 2021-12-08 20:01_

We have decided to deprecate the YAML API.  For more details, see https://github.com/clap-rs/clap/issues/3087.  If there is interest in a YAML API, it will likely be broken out into a `clap_yaml` and developed independent from the core of clap.

---

_Closed by @epage on 2021-12-08 20:01_

---
