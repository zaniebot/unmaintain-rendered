```yaml
number: 1544
title: "doc: use asciidoctor instead of a2x"
type: pull_request
state: closed
author: yous
labels:
  - rollup
assignees: []
base: master
head: use-asciidoctor
created_at: 2020-04-06T16:12:08Z
updated_at: 2020-05-09T14:14:48Z
url: https://github.com/BurntSushi/ripgrep/pull/1544
synced_at: 2026-01-12T18:23:14Z
```

# doc: use asciidoctor instead of a2x

---

_@yous_

Ref: Homebrew/linuxbrew-core#19885

AsciiDoc development is continued under asciidoctor. See https://github.com/asciidoc/asciidoc.

I tried to run CI several times, but on nightly build, Rust fails to find the asciidoctor binary even `/usr/bin/asciidoctor` exists.

Attaching `rg.1.asciidoc` from current build, and `rg.1.asciidoctor` from new build.
- [rg.1.asciidoc.txt](https://github.com/BurntSushi/ripgrep/files/4439537/rg.1.asciidoc.txt)
- [rg.1.asciidoctor.txt](https://github.com/BurntSushi/ripgrep/files/4439540/rg.1.asciidoctor.txt)

---

_Comment by @yous on 2020-04-06 16:33_

Seems that replacing `{` and `}` is no longer needed. Just updated and here is the updated `rg.1.asciidoctor`.

- [rg.1.asciidoctor.txt](https://github.com/BurntSushi/ripgrep/files/4439642/rg.1.asciidoctor.txt)


---

_Review comment by @BurntSushi on `ci/macos-install-packages`:3 on 2020-04-06 20:10_

I wonder if `docbook-xsl` is still needed? We can leave it here for now and I'll experiment with it later. IIRC, asciidoc complained when this wasn't installed before. No idea if asciidoctor also needs it.

---

_@BurntSushi requested changes on 2020-04-06 20:15_

Thanks very much! I had no idea that this was the root cause of the Homebrew issue and also had no idea that asciidoc was itself being EOL'd this year.

So one thing that I'm concerned about here is the abrupt switchover to asciidoctor. I think what I'd rather do is to use asciidoctor like you have in this PR, but fallback to a2x if asciidoctor isn't available. That will make it easy to release this change in a point release with a note that `a2x` support will be removed and man page generation will require asciidoctor in ripgrep 13. That will give package maintainers a bit more slack to deal with this change.

I know it's more of a pain to do that here. Let me know how you'd like to proceed. If you don't do it, then I'd be happy to take over this PR and do it myself, but I don't know when that will be. If you do want to do it, then I'd recommend just having two totally different paths between asciidoc and asciidoctor. No need to try to reuse code between them in `build.rs`. Then we can just lift out `a2x` when we're ready.

Thanks again!

---

_@yous reviewed on 2020-04-07 06:18_

---

_Review comment by @yous on `ci/macos-install-packages`:3 on 2020-04-07 06:18_

I don't think it's needed as when I don't have installed `docbook` or `docbook-xsl`, asciidoctor worked. How about Ubuntu packages? Are `libxslt1-dev`, `docbook-xsl`, `xsltproc`, `libxml2-utils` related only to asciidoc? I'll try running the CI without them.

---

_Comment by @yous on 2020-04-09 07:30_

I renamed the original `generate_man_page` to `legacy_generate_man_page`, and added `generate_man_page` that uses asciidoctor. Please have a look.

---

_Label `rollup` added by @BurntSushi on 2020-05-09 02:37_

---

_@BurntSushi approved on 2020-05-09 02:37_

Brilliant, thank you!

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---

_Branch deleted on 2020-05-09 14:14_

---
