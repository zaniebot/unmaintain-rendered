```yaml
number: 1318
title: How to generate completion scripts?
type: issue
state: closed
author: chevdor
labels:
  - C-enhancement
  - A-docs
  - A-completion
  - E-easy
assignees: []
created_at: 2018-07-12T09:53:46Z
updated_at: 2022-04-29T13:42:25Z
url: https://github.com/clap-rs/clap/issues/1318
synced_at: 2026-01-12T16:14:10Z
```

# How to generate completion scripts?

---

_@chevdor_

I am not seeing any where where the completion script is actually generated:
- I don´t see the 'completions' feature that I have seen in some old issues
- no completion seems to be generated when using `--release`

How does the generation of the completion scripts work ?

After building a simple sample using `--release`, I get the following:
```
$ tree
.
├── Cargo.lock
├── Cargo.toml
├── README.adoc
├── src
│   ├── cli.yml
│   └── main.rs
└── target
    └── release
        ├── build
        ├── cli-args
        ├── cli-args.d
        ├── deps
        │   ├── ansi_term-dc00aa8213642655.d
        │   ├── atty-9fb9c107a1aae491.d
        │   ├── bitflags-12d5ff0cd1d1e3c1.d
        │   ├── clap-628f3835cbf1ff58.d
        │   ├── cli_args-1ce89ace9d3ffa2f
        │   ├── cli_args-1ce89ace9d3ffa2f.d
        │   ├── libansi_term-dc00aa8213642655.rlib
        │   ├── libatty-9fb9c107a1aae491.rlib
        │   ├── libbitflags-12d5ff0cd1d1e3c1.rlib
        │   ├── libc-015d13a8fc234244.d
        │   ├── libclap-628f3835cbf1ff58.rlib
        │   ├── liblibc-015d13a8fc234244.rlib
        │   ├── libstrsim-173d696401437ef5.rlib
        │   ├── libtextwrap-b669f98b21ed064a.rlib
        │   ├── libunicode_width-fc1225c02239f755.rlib
        │   ├── libyaml_rust-004b87347b1a132f.rlib
        │   ├── strsim-173d696401437ef5.d
        │   ├── textwrap-b669f98b21ed064a.d
        │   ├── unicode_width-fc1225c02239f755.d
        │   └── yaml_rust-004b87347b1a132f.d
        ├── examples
        ├── incremental
        └── native
```


---

_Comment by @kbknapp on 2018-07-12 20:10_

I should probably add an example to examples dir that explains this. But the details can be found here:

https://docs.rs/clap/2.32.0/clap/struct.App.html#method.gen_completions



---

_Comment by @kbknapp on 2018-07-12 20:11_

I'm going to close this for now, and mark as "after v3 release"

---

_Closed by @kbknapp on 2018-07-12 20:11_

---

_Label `P4: nice to have` added by @kbknapp on 2018-07-12 20:12_

---

_Label `D: easy` added by @kbknapp on 2018-07-12 20:12_

---

_Label `C: examples` added by @kbknapp on 2018-07-12 20:12_

---

_Label `M: mentored` added by @kbknapp on 2018-07-12 20:12_

---

_Label `C: completion gen` added by @kbknapp on 2018-07-12 20:12_

---

_Label `W: after v3 release` added by @kbknapp on 2018-07-12 20:13_

---

_Comment by @chevdor on 2018-07-16 13:11_

Thanks @kbknapp that´s the link I was missing.

---

_Referenced in [clap-rs/clap#1380](../../clap-rs/clap/issues/1380.md) on 2018-11-10 06:15_

---

_Label `C: docs` added by @CreepySkeleton on 2020-02-02 02:15_

---

_Label `W: after v3 release` removed by @CreepySkeleton on 2020-02-02 02:15_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-02 02:15_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-02 02:15_

---

_Reopened by @CreepySkeleton on 2020-02-02 02:16_

---

_Removed from milestone `3.1` by @pksunkara on 2020-04-09 07:26_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 07:26_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Removed from milestone `3.0` by @epage on 2021-10-16 18:11_

---

_Added to milestone `3.1` by @epage on 2021-10-16 18:11_

---

_Referenced in [epage/clapng#98](../../epage/clapng/issues/98.md) on 2021-12-06 17:36_

---

_Label `C: examples` removed by @epage on 2021-12-08 20:22_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 18:36_

---

_Label `D: easy` removed by @epage on 2021-12-09 18:36_

---

_Label `C-enhancement` added by @epage on 2021-12-09 18:36_

---

_Label `E-medium` removed by @epage on 2021-12-09 18:36_

---

_Label `E-easy` added by @epage on 2021-12-09 18:36_

---

_Comment by @epage on 2022-04-29 13:42_

Looking over this, its documented (currently at https://docs.rs/clap_complete/latest/clap_complete/generator/fn.generate_to.html).  There isn't an indication for why this was re-opened.   Since the need in this is documented, I'm going to go ahead and close this.  Please open a new issue for any additional requests on top of this.

---

_Closed by @epage on 2022-04-29 13:42_

---
