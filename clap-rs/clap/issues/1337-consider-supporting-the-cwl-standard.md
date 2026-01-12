```yaml
number: 1337
title: Consider supporting the CWL standard CommandLineTool for YAML specification of a CLI interface
type: issue
state: closed
author: mr-c
labels:
  - S-wont-fix
assignees: []
created_at: 2018-09-05T11:19:28Z
updated_at: 2022-01-11T18:47:57Z
url: https://github.com/clap-rs/clap/issues/1337
synced_at: 2026-01-12T16:14:10Z
```

# Consider supporting the CWL standard CommandLineTool for YAML specification of a CLI interface

---

_@mr-c_

### Bug or Feature Request Summary

Specification: https://www.commonwl.org/v1.0/CommandLineTool.html
User guide: https://www.commonwl.org/user_guide/

CWL project homepage: https://www.commonwl.org/

If some properties that you have aren't yet in CWL you can add them as "vendor extensions" and retain compatibility. We'd want to study your extensions for possible future inclusion.


---

_Comment by @CreepySkeleton on 2020-02-02 05:54_

I haven't read the spec completely but looks pretty interesting. cc @clap-rs/admins What do you think?


---

_Label `T: RFC / question` added by @CreepySkeleton on 2020-02-02 05:56_

---

_Comment by @pksunkara on 2020-02-02 13:56_

So, I looked over the spec and I am really hesitant to support it. It will have too much noise compared to clap's yaml which will make it harder for users to use.

---

_Comment by @mr-c on 2020-02-03 10:03_

@pksunkara What do you think about CWL export from clap-using apps?

---

_Comment by @pksunkara on 2020-02-03 10:04_

I don't mind that, but it will have to go clap_generate crate and even then, I am not sure this is not niche.

---

_Comment by @CreepySkeleton on 2020-02-03 11:15_

@mr-c The main goal of `clap` is to provide a flexible an feature-full parser for command line arguments, but the full scope of CWL is way beyond this goal. I don't think we'll ever consider supporting all of it.

About CLI part only - well, if it's all about building `clap::App` from corresponding `cwl.yaml` file it can be seen as "yet another format akin to YAML and TOML". We are considering to make use of `serde` (#1630). If/when we manage to implement it, you will be free to make your own implementation of it , like `serde_json` or `serde_yaml`.

Even if we decide not to support it, you are still free to make your own "converter" crate that reads `cwl.yaml` and builds the corresponding `clap::App`.

I'm closing this because I'm almost sure we don't want to maintain such an implementation in this repo. 

---

_Closed by @CreepySkeleton on 2020-02-03 11:15_

---

_Comment by @Dylan-DPC-zz on 2020-02-03 12:18_

I'd like to consider this in the future. Let's keep it open. 

---

_Reopened by @Dylan-DPC-zz on 2020-02-03 12:18_

---

_Comment by @mr-c on 2020-02-03 12:43_

@Dylan-DPC @CreepySkeleton I am happy to write a Google Summer of Code proposal and co-mentor alongside a rust person to work on CWL export from `clap` applications.

---

_Comment by @luizirber on 2020-02-03 23:27_

This is pretty much exactly why I used the YAML feature to write [my CLI][0]. I had previously handwritten the equivalent command ([compute][1]) in CWL, and I think it would be extremely useful to have both more Rust CLIs in bioinformatics and larger adoption of CWL tool descriptions.

[0]: https://github.com/luizirber/decoct/blob/b53cdc76070cc9997a1aecac410bff99262f9d0e/src/decoct.yml#L115
[1]: https://github.com/luizirber/sourmash-cwl/blob/d76c15bd079c7625358ed271aeb9514702770f59/sourmash-compute.cwl


---

_Comment by @pksunkara on 2020-02-04 07:51_

@codesections Could this come under documentation crate? Can one of the generators be CWL?

---

_Comment by @medcelerate on 2020-02-04 19:15_

I would absolutely love this feature set, would make our biofx tools more powerful and easier to add to workflows.

---

_Comment by @Dylan-DPC-zz on 2020-02-04 19:33_

Let's discuss this once we are done with the 3.0.0 release 

---

_Referenced in [clap-rs/clap#1729](../../clap-rs/clap/issues/1729.md) on 2020-03-17 21:24_

---

_Referenced in [epage/clapng#101](../../epage/clapng/issues/101.md) on 2021-12-06 17:36_

---

_Comment by @epage on 2021-12-08 21:03_

We have decided to deprecate the YAML API.  For more details, see https://github.com/clap-rs/clap/issues/3087.  I am assuming this would fall in the same category.  If there is interest in a data-driven API, it will likely be broken out into a `clap_yaml` and developed independent from the core of clap.

---

_Closed by @epage on 2021-12-08 21:03_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:47_

---
