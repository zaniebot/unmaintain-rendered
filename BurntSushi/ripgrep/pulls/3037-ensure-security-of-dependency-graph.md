```yaml
number: 3037
title: Ensure security of dependency graph
type: pull_request
state: closed
author: janvhs
labels: []
assignees: []
draft: true
base: master
head: master
created_at: 2025-04-27T12:06:33Z
updated_at: 2025-04-30T15:51:09Z
url: https://github.com/BurntSushi/ripgrep/pull/3037
synced_at: 2026-01-12T18:23:15Z
```

# Ensure security of dependency graph

---

_@janvhs_

Hi, for a while now I'm a bit skeptical of the steep dependency graphs that a lot of Rust tools have. As a counter measure I decided to "validate" the ones I installed on my system and use regularly. ripgrep is the first one in this series.

This PR would add [cargo-vet](https://github.com/mozilla/cargo-vet), the tool that Mozilla develops and Google, Mozilla and several other projects use to ensure the code they deploy is not containing any malicious code by third parties. It's a proactive tool used for code auditing dependencies incrementally. You can import audits from people or organisations you trust, which leads to a smaller "trust-graph". I am in the process of introducing this tool at my work at SUSE and it seems to become slowly more popular in the Rust community.

This PR adds a few audits and trusts the Windows crates as well as some crates by David Tolnay. I left `jemallocator` and `crossbeam-deque` without any decision, because I am unfamiliar with both crates and their authors and did not have the capacity to audit them. Especially, `jemalloc-sys` has a footprint of 127106 lines and I didn't want to deal with it before getting feedback to this PR. Information of each decision is in the corresponding commit

If you would be interested into improving the auditability further, [cargo-auditable](https://github.com/rust-secure-code/cargo-auditable) seems very nice for embedding a dependency list into the built binary without effecting reproducibility. Embedding this info allows various tools like [OWASP's blint](https://github.com/owasp-dep-scan/blint), [cargo-auditable](https://github.com/rust-secure-code/cargo-auditable), [Google's osv-scanner](https://github.com/google/osv-scanner), [Anchore's syft](https://github.com/anchore/syft) and [Aqua Security's trivy](https://github.com/aquasecurity/trivy) to pick it up from the binary and create SBOMs for a project, whole system or use them in different ways like vulnerability scanning via [grype](https://github.com/anchore/grype). In fact, Go already does this by default and it is a great value add. I actually use part of this info automatically for updating my manually installed Go binaries to the newest version.

---

_Comment by @BurntSushi on 2025-04-27 13:26_

So my initial reaction here was to reject this because I don't think it's worth this level of auditing for ripgrep. In particular, I stopped using `cargo crev` (many moons ago) for similar reasons: I liked the idea in principle, but in practice, it was _way_ too much ceremony to bother with it. And it just wasn't buying me much. What would end up happening is that I would just avoid updating dependencies at all. Eventually this got to be untenable and so I ripped it out. On top of that, I am already quite selective about the dependencies I use. If ripgrep is the first project you applied `cargo vet` to, you might find that its dependency tree is quite a bit smaller than many other tools. And especially so if you consider that a huge chunk of them were authored by myself.

With that said, I am potentially open to something here. But:

* I need to understand how it impact the [release checklist](https://github.com/BurntSushi/ripgrep/blob/master/RELEASE-CHECKLIST.md).
* Probably anything by dtolnay should be trusted, e.g., `anyhow`

My main concern here is that, if I want/need to update a dependency to a version without an audit, then in practice, that review burden is going to fall to me. And I have _very little_ bandwidth to do that. So I am worried that this just isn't going to be sustainable.

Moreover, ripgrep doesn't have frequent releases. So its blast radius if a malicious dependency gets into its tree is likely quite limited. So I'm skeptical that this ceremony is really worth it at all.

---

_Comment by @janvhs on 2025-04-30 15:43_

After reading your feedback, I agree it's not worth the effort it introduces. You're right the dependency graph of ripgrep is pleasantly small and I reviewed less than 10 crates. I'll keep the fork for a few releases and update it once a while to see how much there actually it to review between the changes.

I can 100% understand the hesitation here and think it's valid.
Originally I only started this for my own peace of mind, because now I'm not asking myself "should I really install this program" anymore and can just enjoy using ripgrep

Thank you for considering and writing ripgrep ^^

---

_Closed by @janvhs on 2025-04-30 15:43_

---

_Comment by @BurntSushi on 2025-04-30 15:51_

Yes! Thanks for understanding. It has been a lot of work to keep the dependency tree "small."

---
