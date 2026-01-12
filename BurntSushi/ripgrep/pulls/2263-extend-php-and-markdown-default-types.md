```yaml
number: 2263
title: "Extend `php` and markdown default types"
type: pull_request
state: merged
author: mal-tee
labels: []
assignees: []
merged: true
base: master
head: default_types
created_at: 2022-07-18T10:28:58Z
updated_at: 2022-07-18T14:35:10Z
url: https://github.com/BurntSushi/ripgrep/pull/2263
synced_at: 2026-01-12T18:23:14Z
```

# Extend `php` and markdown default types

---

_@mal-tee_

_No description provided._

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:173 on 2022-07-18 11:05_

What about `*.php6`?

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:174 on 2022-07-18 11:06_

I don't think I've ever seen `pht` used for PHP before. Can you show me a project that uses it?

---

_@BurntSushi reviewed on 2022-07-18 11:07_

Thanks! I left some questions since I'm a little unsure.

---

_Review comment by @mal-tee on `crates/ignore/src/default_types.rs`:174 on 2022-07-18 11:50_

It quite obscure. But I've just noticed that it's also used by other language eco-systems. Maybe we shouldn't include it to prevent overlaps and false-positives.

---

_@mal-tee reviewed on 2022-07-18 11:50_

---

_Review comment by @mal-tee on `crates/ignore/src/default_types.rs`:173 on 2022-07-18 11:51_

PHP 6 doesn't exist [1], but we could include the extension as well. :) 

[1] https://wiki.php.net/rfc/php6

---

_@mal-tee reviewed on 2022-07-18 11:51_

---

_@BurntSushi reviewed on 2022-07-18 11:52_

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:174 on 2022-07-18 11:52_

Basically, I'd prefer to stick to extensions that are actually used somewhere. I'm okay with obscure, but I'd like to see a real project somewhere using them.

---

_@BurntSushi reviewed on 2022-07-18 11:56_

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:173 on 2022-07-18 11:56_

Haha. Nice. OK, yes, leave it out. Could you add a small comment with a link to that page in the source here? I'm sure I won't be the only one to notice and ponder about why `.php6` is absent.

---

_@mal-tee reviewed on 2022-07-18 12:03_

---

_Review comment by @mal-tee on `crates/ignore/src/default_types.rs`:174 on 2022-07-18 12:03_

[Github's search ](https://github.com/search?q=extension%3Apht) reveals some repos using it, e.g. https://github.com/chrisbroski/lulaleasuretime/tree/master/inc and https://github.com/RundizBones/ModuleAdmin/tree/3599dfbe41708dd94cfb7306dad1a44045004bd4/Console/CreateModuleTemplate

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:174 on 2022-07-18 12:05_

Fair enough...

---

_@BurntSushi reviewed on 2022-07-18 12:05_

---

_Review requested from @BurntSushi by @mal-tee on 2022-07-18 12:28_

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:171 on 2022-07-18 13:16_

```suggestion
        // note that PHP 6 doesn't exist
        // See: https://wiki.php.net/rfc/php6
```

---

_@BurntSushi requested changes on 2022-07-18 13:16_

---

_Review comment by @mal-tee on `crates/ignore/src/default_types.rs`:171 on 2022-07-18 14:23_

Oops, forgot the link. Thank you.

---

_@mal-tee reviewed on 2022-07-18 14:23_

---

_@BurntSushi approved on 2022-07-18 14:26_

Thanks!

---

_Merged by @BurntSushi on 2022-07-18 14:35_

---

_Closed by @BurntSushi on 2022-07-18 14:35_

---
