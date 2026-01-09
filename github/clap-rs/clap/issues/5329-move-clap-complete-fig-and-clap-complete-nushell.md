---
number: 5329
title: Move clap_complete_fig and clap_complete_nushell into  clap_complete
type: issue
state: closed
author: jcgruenhage
labels:
  - C-enhancement
  - A-completion
assignees: []
created_at: 2024-01-24T15:36:36Z
updated_at: 2025-11-24T15:49:33Z
url: https://github.com/clap-rs/clap/issues/5329
synced_at: 2026-01-07T13:12:20-06:00
---

# Move clap_complete_fig and clap_complete_nushell into  clap_complete

---

_Issue opened by @jcgruenhage on 2024-01-24 15:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.9

### Describe your use case

As a distro maintainer, I want to ship shell completions for all shells shipped by my distro. Even if a package ships completions generated with clap_complete,  the ones for nushell for example aren't included, because  people only take the ones listed in clap_complete directly. See https://github.com/mitsuhiko/minijinja/pull/403/files#diff-cea125128a0bc3a52233a2d72ca6519778801f05f6e5c45612dcf6045c1c0075R33-R37, to see why.

### Describe the solution you'd like

I'd like to move clap_complete_nushell and clap_complete_fig into clap_complete itself. This way, it's easier to generate completions for everything.

### Alternatives, if applicable

It would also be possible to keep them in separate crates, which are then depended upon by clap_complete, with that inclusion being hidden behind feature flags.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @jcgruenhage on 2024-01-24 15:36_

---

_Comment by @epage on 2024-01-24 16:13_

This is intentional as we aren't as committed to the same level of support for these shells.  If we had this model back when Elvish was added, it would be in a separate package as well.

Similarly, when #3166 is stabilized, we might be changing what shells are supported in `clap_complete`.


---

_Closed by @epage on 2024-01-24 16:13_

---

_Comment by @jcgruenhage on 2024-01-24 18:25_

Wouldn't it make more sense to hide the shells with less commitment behind a feature flag like "experimental_shells" then? That'd still make it a lot easier to package completions for those tools as a package maintainer for distros.


---

_Comment by @epage on 2024-01-24 18:43_

Its easier to manage the support level as a distinct package, rather than dealing with moving a package in and out (e.g. `clap_complete_nushell` was started outside of the clap project).

Also, with #3166, there will be shells (like fig) that will be incompatible with what `clap_complete` will turn into.   When we make the transition to shell-native completions to rust-native completions, I expect we'll migrate all of the shell-native completions into individual packages which will be at a lower level of support.

---

_Comment by @jcgruenhage on 2024-01-25 09:53_

So that means there won't be a way to iterate over all shells anymore in the future? Then package maintainers would have to do a lot more patching to ensure all shell completions are built.

I understand why that makes sense from a clap PoV, but as a distro maintainer this does make my life a bit harder.

On a more positive note: Very much looking forward to rust-native completions :)

---

_Comment by @epage on 2024-01-25 15:42_

Isn't this a choice for the CLI maintainer?  It seems odd for a distro to be trying to extend a program beyond what the maintainer supports.

---

_Comment by @jcgruenhage on 2024-01-26 12:17_

Yeah, but the patches for those CLI tools often come from distros to the upstream repo. The cases I've seen are more that the upstream maintainers aren't even aware that they could improve their UX by providing (more) completions or a man page. There's probably a dozen or so cases where I've packaged CLI tooling and upstreamed generation of completions and man pages so that we could ship them in Void Linux.

Ideally, the patch does land upstream, but providing a man page and cli completions auto-generated from a clap definition where the patch does not land upstream does occasionally happen too. Considering we're not changing how the tool itself works, I think that's fine too.

---

_Referenced in [wiktor-k/clap_allgen#1](../../wiktor-k/clap_allgen/pulls/1.md) on 2024-08-12 08:02_

---

_Label `A-completion` added by @epage on 2025-11-24 15:44_

---

_Comment by @epage on 2025-11-24 15:45_

Related: #5880

---

_Comment by @epage on 2025-11-24 15:49_

From https://github.com/clap-rs/clap/discussions/5880#discussioncomment-15064094

> There are actually two distinctions for us to make:
> 
>   * integrated into `clap_complete`
> 
>   * enabled by default
> 
> 
> Thinking more on this, there are multiple considerations
> 
>   * The expected longevity / popularity of the shell
> 
>   * Our ability to maintain support for the shell (e.g. problems like [`clap_complete::env::Elvish` generates code deprecated in 0.18, removed in 0.21 #5729](https://github.com/clap-rs/clap/issues/5729))
> 
>   * Whether we should automatically add to the compatibility surface of callers
> 
> 
> For that last one, if we add new shells to `Shell`, existing applications will get it for free by upgrading, whether they want it or not. I wonder if we should extend `Shell` in breaking releases and we should likely shrink it from where it is currently at to be very conservative.



---

_Referenced in [rust-lang/rustup#4614](../../rust-lang/rustup/pulls/4614.md) on 2025-11-24 15:54_

---
