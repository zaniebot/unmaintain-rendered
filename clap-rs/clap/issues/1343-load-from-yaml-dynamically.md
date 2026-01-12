```yaml
number: 1343
title: load from YAML dynamically
type: issue
state: closed
author: whoizit
labels: []
assignees: []
created_at: 2018-09-17T02:42:45Z
updated_at: 2018-09-19T05:29:17Z
url: https://github.com/clap-rs/clap/issues/1343
synced_at: 2026-01-12T16:14:10Z
```

# load from YAML dynamically

---

_@whoizit_

Just a question.
Can I load YAML file dynamically, without recompilation?

---

_Comment by @whoizit on 2018-09-19 05:29_

Ok, I have a answer from Russian community. Just I shouldn't use load_yaml!
from
```
let yml = load_yaml!("17_yaml.yml");
let m = App::from_yaml(yml).get_matches();
```
to
```
let yml = "17_yaml.yml";
let m = App::from_yaml(yml).get_matches();
```
or something like that

---

_Closed by @whoizit on 2018-09-19 05:29_

---
