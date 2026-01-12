```yaml
number: 368
title: App name is not used in help
type: issue
state: closed
author: ghost
labels:
  - C-bug
  - A-builder
assignees: []
created_at: 2015-12-24T13:55:28Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/368
synced_at: 2026-01-12T16:14:09Z
```

# App name is not used in help

---

_@ghost_

The first line of help should be the app name but it's always the binary name (whether bin_name is 
configured or not.) 

For example, when this app is compiled to `mraa`

```
extern crate clap;

use clap::App;

fn main() {
    App::new("My-Really-Awesome-App").get_matches();

    println!("Hello, world!");
}
```

`mraa --help` prints

```
mraa 

USAGE:
    mraa [FLAGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

whereas it should print

```
My Really Awesome App

USAGE:
    mraa [FLAGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```


---

_Label `T: bug` added by @Vinatorul on 2015-12-24 14:53_

---

_Label `P1: urgent` added by @Vinatorul on 2015-12-24 14:53_

---

_Label `C: app` added by @Vinatorul on 2015-12-24 14:53_

---

_Label `D: easy` added by @Vinatorul on 2015-12-24 14:53_

---

_Comment by @kbknapp on 2015-12-24 16:26_

Thanks for filing this, I agree and guess I've never really thought about it too much before since the binary name is usually the same as the app name for what I've made :smile: 

We should have this fixed soon :+1: 


---

_Added to milestone `1.5.5` by @kbknapp on 2015-12-24 16:27_

---

_Closed by @kbknapp on 2016-01-04 08:55_

---
