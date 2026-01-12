```yaml
number: 2681
title: Add Completion Files + Manpage to Source Repository 
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2023-12-11T04:35:37Z
updated_at: 2023-12-11T21:27:08Z
url: https://github.com/BurntSushi/ripgrep/issues/2681
synced_at: 2026-01-12T16:13:24Z
```

# Add Completion Files + Manpage to Source Repository 

---

_@ghost_

#### Describe your feature request

Hi, I am a package contributor to Guix and I recently submitted a patch series to [update ripgrep to version 14.0.3](https://issues.guix.gnu.org/67757#19). I noted the recent change to how shell completions and the manpage was generated via the ripgrep binary itself instead of using an external build tool like asciidoc. 

Unfortunately, this presents a challenge for Guix because it makes it virtually impossible to cross-compile ripgrep since the target binary (that being ripgrep) has to be run in order to generate those files. 

A good solution to this would be to simply have the generated completion files and manpage already within the actual source code repository itself. If this is already the case, feel free to point me to it and close this issue. If not, this would be a very welcome addition in the next point release or so as it would resolve this conundrum entirely and allow us (at Guix) to have reproducible builds with cross-compilation. 

---

_Comment by @BurntSushi on 2023-12-11 05:11_

> it makes it virtually impossible to cross-compile ripgrep since the target binary (that being ripgrep) has to be run in order to generate those files.

ripgrep's CI also does cross compilation, but its still able to generate man pages and completion files via qemu. Why can't you do that?

Another option is to download one of the release artifacts. Those include the man page and completion files.

I don't want to add these files to the repository because they risk getting out of date. It's also not clear to me why Guix has a problem with this, but nobody else seems to.

---

_Comment by @ghost on 2023-12-11 20:59_

> Why can't you do that...It's also not clear to me why Guix has a problem with this.

I was talking to some folks on Guix IRC who informed me of a potential issue with cross-comp. I should have mentioned that I'm still investigating the issue on my end. I was just eager to see what the thoughts were about the possibility of including these files in the source repo (which admittedly would make it a lot easier for me as a packager). 

> Another option is to download one of the release artifacts. Those include the man page and completion files.

Downloading the release artifacts would not be an option due to the rules of Guix upstream, but it's good to know that cross-comp is something on my end to improve. 

Thank you for such a speedy reply and congrats on the new major release. 

---

_Closed by @ghost on 2023-12-11 20:59_

---

_Comment by @BurntSushi on 2023-12-11 21:27_

Thanks! Let me know if it doesn't work as-is. I'm still happy to brainstorm other ideas. Another idea is to provide a full source archive with each release that you would download instead of cloning the repo. But I confess it is difficult to brainstorm without knowing what the rules are.

---
