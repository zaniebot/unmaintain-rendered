```yaml
number: 1417
title: Surprising feature - ripgrep respects git excludes
type: issue
state: closed
author: nnathan
labels: []
assignees: []
created_at: 2019-11-04T05:24:40Z
updated_at: 2019-11-04T15:15:15Z
url: https://github.com/BurntSushi/ripgrep/issues/1417
synced_at: 2026-01-12T16:13:23Z
```

# Surprising feature - ripgrep respects git excludes

---

_@nnathan_

#### What version of ripgrep are you using?

`ripgrep 11.0.1`

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

The macos before broken arse catalina.

#### Describe your question, feature request, or bug.

ripgrep supports local excludes `$GIT_DIR/info/exclude`.

Nowhere in the manpage does it discuss the excludes, but only .gitignore and the global core.excludesFile. This is a feature of ripgrep [according to the guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md).

But it seems to be a feature you cannot specifically opt-out of - only all .gitignore and exclusions.

I know it's meant to be smart - but not being able to opt-out of your local excludes while respecting repository shared ignores is dumb. I would argue that it should also be specifically opt-in because it can be surprising to some - the only tools that respect the local exclude are git based tools and seemingly only rg. But that'd change the expected UI.

---

_Comment by @BurntSushi on 2019-11-04 10:39_

I don't really understand this ticket. ripgrep is working as intended.

---

_Closed by @BurntSushi on 2019-11-04 10:39_

---

_Comment by @nnathan on 2019-11-04 10:50_

I'm fine with closing the ticket.

But I think you fundamentally misunderstand something and don't seem keen to even entertain it.

1. A .gitignore is a repository _shared_ file.
2. A .git/info/exclude is a _local_ exception that's not shared.

When ripgrep manual says it respects .gitignore intuitively it means (1) and **not** (2).
One could say it respects any file git would ignore which would encompass (1) and (2).
A more precise explanation would be to say it respects .gitignore and local git exclusions. But none of the manual page discusses the distinction of (1) and (2) and completely omits any explanation of (2).

From a user experience perspective it doesn't do what the user expects, I was using it as a drop-in replacement for ag, which it cannot be, because it does more than rg, and it can't even be compatible with ag since you can't even tell it to respect (1) and (2).

If anything, I would recommend you update the manual page to reflect the distinction, precisely because it's confusing. And that you can only opt out of both.

---

_Comment by @BurntSushi on 2019-11-04 13:18_

I don't see how it's confusing. ripgrep respects all gitignores that it can, including gitignores in parent directories, child directories, global rules and local overrides. Your comparison with ag is strange, given the incredible number of bugs and problems with ag's support for gitignores. While ripgrep takes inspiration from tools such as grep and ag, needling me over the fact that ripgrep isn't a drop in replacement for ag because it doesn't have the same bugs is pretty unproductive.

---

_Comment by @nnathan on 2019-11-04 14:58_

Fair enough. I guess we can both agree some people are idiots. And idiots deserve inferior tools.

---
