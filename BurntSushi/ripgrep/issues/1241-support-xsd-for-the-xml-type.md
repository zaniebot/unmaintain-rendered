```yaml
number: 1241
title: "Support `*.xsd` for the `xml` type"
type: issue
state: closed
author: hupfdule
labels: []
assignees: []
created_at: 2019-04-09T15:00:57Z
updated_at: 2019-04-09T15:29:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1241
synced_at: 2026-01-12T16:13:23Z
```

# Support `*.xsd` for the `xml` type

---

_@hupfdule_

#### What version of ripgrep are you using?

0.10.0

#### How did you install ripgrep?

Via official Debian Buster repository.

#### What operating system are you using ripgrep on?

Debian Linux (Buster)

#### Describe your question, feature request, or bug.

The `xml` type of ripgrep currently only supports files ending in `*.xml` and `*.xml.dist` (and I have actually not idea, what a `*.xml.dist` file should be…
However there are a lot more files that are XML files and should be included in that type.
See for example the supported filetype of [ack-grep](https://beyondgrep.com/):

    --[no]xml          .xml .dtd .xsl .xslt .ent; first line matches /<[?]xml/

There are some filetypes missing from ack-grep as well:

- `*.xsd` (XML Schema)
- `*.xjb` (JAXB Binding)

Those should be included as well.

One interesting bit in ack-grep is 

    first line matches /<[?]xml/

This is actually a good candidate to detect XML files that don't match any of the above mentioned filename suffixes and should be supported by ripgrep as well.

---

_Comment by @BurntSushi on 2019-04-09 15:05_

Please see my comment in your previous issue: https://github.com/BurntSushi/ripgrep/issues/1240#issuecomment-481288418

Just open a PR with the requisite changes and we can discuss them there.

ripgrep does not support file type detection by looking at the contents of the file. It's a performance trap, so it's unlikely that ripgrep will ever support it. But if you feel strongly about it, please open a separate issue with compelling arguments in favor of it.

---

_Closed by @BurntSushi on 2019-04-09 15:05_

---

_Comment by @hupfdule on 2019-04-09 15:29_

Sorry, I wrote this Issue before seeing your comment on #1240.

See #1243 

---
