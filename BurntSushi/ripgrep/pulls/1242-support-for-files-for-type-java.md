```yaml
number: 1242
title: "Support for files for type `java`"
type: pull_request
state: closed
author: hupfdule
labels: []
assignees: []
base: master
head: type-java
created_at: 2019-04-09T15:09:30Z
updated_at: 2019-04-15T01:55:22Z
url: https://github.com/BurntSushi/ripgrep/pull/1242
synced_at: 2026-01-12T18:23:13Z
```

# Support for files for type `java`

---

_@hupfdule_

- `*.jspx` for XHTML JSP files
- `*.properties` for Java Properties files (resource bundles, etc.)
- `pom.xml` for Maven project build files

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:159 on 2019-04-09 15:18_

I'm not well versed in Java, but [according to this page](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html), `pom.xml` looks like an XML file. If I were a Java programmer, I'd be surprised if `rg -tjava` searched XML data. (Just like I'd be surprised if `rg -trust` searched `Cargo.toml`.)

---

_@BurntSushi reviewed on 2019-04-09 15:18_

---

_@hupfdule reviewed on 2019-04-09 15:30_

---

_Review comment by @hupfdule on `ignore/src/types.rs`:159 on 2019-04-09 15:30_

I have to think about that a bit. :-)

---

_Closed by @BurntSushi on 2019-04-14 23:38_

---

_Comment by @BurntSushi on 2019-04-14 23:38_

I merged this PR, but dropped the `pom.xml` entry.

---

_Comment by @hupfdule on 2019-04-15 01:55_

Yes, I think that's ok.
Thank you! 

---
