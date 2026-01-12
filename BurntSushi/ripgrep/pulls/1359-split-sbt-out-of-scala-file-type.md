```yaml
number: 1359
title: Split sbt out of scala file type
type: pull_request
state: closed
author: MaT1g3R
labels: []
assignees: []
base: master
head: master
created_at: 2019-08-29T00:39:19Z
updated_at: 2020-02-15T13:23:15Z
url: https://github.com/BurntSushi/ripgrep/pull/1359
synced_at: 2026-01-12T18:23:13Z
```

# Split sbt out of scala file type

---

_@MaT1g3R_

sbt is a build tool for Scala. It is often useful to search only in sbt
files and not Scala files (e.g. looking for dependencies)

---

_Review comment by @BurntSushi on `ignore/Cargo.toml`:3 on 2019-08-29 00:47_

Please don't do this. I handle versioning of crates in this repo.

---

_@BurntSushi requested changes on 2019-08-29 00:47_

Sorry, but since I am not involved in the Scala community, I don't feel comfortable merging this. Firstly, this is a breaking change. Breaking changes are themselves fine, but I always want to be judicious about them. Secondly, I don't know whether this is the right change or not.

Could you perhaps gather opinions from folks in the Scala community about whether this is okay or not?

---

_@MaT1g3R reviewed on 2019-08-29 00:49_

---

_Review comment by @MaT1g3R on `ignore/Cargo.toml`:3 on 2019-08-29 00:49_

Reverted this commit

---

_Comment by @MaT1g3R on 2019-08-29 00:51_

I think filter only sbt files is a very useful feature, since there can be a lot of these files in large Scala projects. How do you feel about adding `*.sbt` back to Scala file type, and just add the new `sbt` type?

---

_Comment by @BurntSushi on 2019-08-29 00:55_

@MaT1g3R I'm not quite sure how I feel about it. It is very easy to just write `-g '*.sbt'` (or define your own file type via an alias or `.ripgreprc`). ripgrep generally doesn't have file types for every possible build system within every programming language, and I'm not sure if I want to start that precedent.

---

_Comment by @MaT1g3R on 2019-08-29 01:13_

Since `gradle` is grouped under groovy, I think it's reasonable that `sbt` is its own file type, since `sbt` is more of a programming language than a config file.

Adding this change will introduce the file type to other programs like `fd`. `fd` doesn't seems to be configured via `.ripgreprc` (It would be weird if it did)

---

_Comment by @BurntSushi on 2020-02-15 13:23_

I'm going to close this because I haven't received any credible guidance on whether removing `sbt` from the Scala type is appropriate or not.

I'd probably be okay with having a separate `sbt` type that overlaps with `scala`.

---

_Closed by @BurntSushi on 2020-02-15 13:23_

---
