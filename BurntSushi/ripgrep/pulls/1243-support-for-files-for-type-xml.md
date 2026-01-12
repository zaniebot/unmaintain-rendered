```yaml
number: 1243
title: "Support for files for type `xml`"
type: pull_request
state: merged
author: hupfdule
labels: []
assignees: []
merged: true
base: master
head: xml-type
created_at: 2019-04-09T15:28:17Z
updated_at: 2019-04-09T19:17:58Z
url: https://github.com/BurntSushi/ripgrep/pull/1243
synced_at: 2026-01-12T18:23:13Z
```

# Support for files for type `xml`

---

_@hupfdule_

- `*.dtd` for Document Type Definitions
- `*.xsl` and `*.xslt` for XSL Transformation descriptions
- `*.xsd` for XML Schema definitions
- `*.xjb` for JAXB bindings
- `*.rng` for Relax NG files
- `*.sch` for Schematron files

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:304 on 2019-04-09 15:42_

I think these all look good to me.

Could you please use a style consistent with the surrounding code? In particular, please wrap code to 79 columns inclusive.

Thanks!

---

_@BurntSushi requested changes on 2019-04-09 15:42_

---

_@hupfdule reviewed on 2019-04-09 16:23_

---

_Review comment by @hupfdule on `ignore/src/types.rs`:304 on 2019-04-09 16:23_

No problem, I will update it.

Do you want me to add another commit or can i force-push the change to the existing commit (rewriting history there)?

---

_@BurntSushi reviewed on 2019-04-09 16:27_

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:304 on 2019-04-09 16:27_

Force push please, if convenient, in order to keep this as a single commit. (Otherwise, I will squash this into one commit for you. I try to keep ripgrep's revision history very clean.)

---

_Review comment by @hupfdule on `ignore/src/types.rs`:304 on 2019-04-09 16:31_

Done.

---

_@hupfdule reviewed on 2019-04-09 16:31_

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:305 on 2019-04-09 16:34_

Sorry to be a pain about this, but please format the code in a style that is consistent with the surrounding code. Please consult other file types, such as `systemd`, for how to format the list when it gets to be too long.

---

_@BurntSushi requested changes on 2019-04-09 16:35_

---

_@hupfdule reviewed on 2019-04-09 16:42_

---

_Review comment by @hupfdule on `ignore/src/types.rs`:305 on 2019-04-09 16:42_

No problem. Didn't notice the difference.

It did this change now.

(BTW: The definition of `asp` is 87 characters long. ;-)) 

---

_@BurntSushi reviewed on 2019-04-09 16:44_

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:305 on 2019-04-09 16:44_

Yes, I don't always catch violations of the line lengths, especially if they're close. (Because the GitHub UI doesn't make it easy to see.) And I rarely edit this file myself any more, so I didn't even know about it. :-)

---

_@BurntSushi approved on 2019-04-09 16:45_

Great, thanks! And thanks for your patience on the formatting. :-)

---

_Merged by @BurntSushi on 2019-04-09 19:17_

---

_Closed by @BurntSushi on 2019-04-09 19:17_

---
