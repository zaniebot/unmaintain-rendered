```yaml
number: 840
title: ArgGroup yaml creation does not allow multiple option
type: issue
state: closed
author: ZoeyR
labels:
  - C-enhancement
assignees: []
created_at: 2017-02-02T19:06:43Z
updated_at: 2017-02-03T20:46:27Z
url: https://github.com/clap-rs/clap/issues/840
synced_at: 2026-01-12T16:14:10Z
```

# ArgGroup yaml creation does not allow multiple option

---

_@ZoeyR_

### Affected Version of clap
2.20.1

### Expected Behavior Summary
`ArgGroup` is created from Yaml

### Actual Behavior Summary
Program errors out with   
`thread 'main' panicked at 'Unknown ArgGroup setting 'multiple' in YAML file for ArgGroup 'io'', C:\Users\<user>\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-2.20.1\src\args\group.rs:493`

### Steps to Reproduce
create yaml file similar to the following  
```yaml
name: dungeon
version: "0.1.0"
author: Daniel Griffen <daniel@griffen.io>
args:
    - save:
        short: s
        long: save
        value_name: FILE
        min_values: 0
        max_values: 1
    - load:
        short: l
        long: load
        value_name: FILE
        min_values: 0
        max_values: 1
groups:
    - io:
        args: [save, load]
        multiple: true
```

Try and create matches object with  
```rust
let yaml = load_yaml!("cli.yml");
let matches = App::from_yaml(yaml).get_matches();
```

---

_Label `C: arg groups` added by @kbknapp on 2017-02-03 16:11_

---

_Label `C: yaml parser` added by @kbknapp on 2017-02-03 16:11_

---

_Label `P2: need to have` added by @kbknapp on 2017-02-03 16:11_

---

_Label `T: enhancement` added by @kbknapp on 2017-02-03 16:11_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-03 16:11_

---

_Added to milestone `2.20.2` by @kbknapp on 2017-02-03 16:11_

---

_Assigned to @kbknapp by @kbknapp on 2017-02-03 16:11_

---

_Closed by @kbknapp on 2017-02-03 20:46_

---
