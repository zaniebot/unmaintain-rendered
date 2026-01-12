```yaml
number: 146
title: "add jinja type for *.jinja and *.jinja2"
type: pull_request
state: merged
author: samuelcolvin
labels: []
assignees: []
merged: true
base: master
head: add-jinja-type
created_at: 2016-10-04T12:03:23Z
updated_at: 2016-10-04T12:25:23Z
url: https://github.com/BurntSushi/ripgrep/pull/146
synced_at: 2026-01-12T18:23:12Z
```

# add jinja type for *.jinja and *.jinja2

---

_@samuelcolvin_

see #145.

I guess this is quite niche, it's just one templating language (although popular) but without the option for an override or extension locally I guess this is the best option.


---

_@BurntSushi reviewed on 2016-10-04 12:05_

---

_Review comment by @BurntSushi on `src/types.rs` on 2016-10-04 12:05_

You you put this in alphabetical order? Trying to keep things tidy. :-)


---

_Comment by @BurntSushi on 2016-10-04 12:06_

> but without the option for an override or extension locally I guess this is the best option.

Not sure I follow. `--type-add` will let you do this. e.g., `rg --type-add 'jinga:*.{jinga,jinga2}' -tjinga QUERY_PATTERN` will only search Jinga files. If you do `alias rg="rg --type-add 'jinga:*.{jinga,jinga2}'"` then `rg -tjinga QUERY_PATTERN` will do the right thing.

(I'd still like to accept this PR too. Jinga is pretty popular.)


---

_Comment by @samuelcolvin on 2016-10-04 12:14_

(Note, it's jinja, not "jinga".)

> `alias rg="rg --type-add 'jinga:*.{jinga,jinga2}'"`

Makes sense, and I guess that's leaner and more canonical than `rg` having it's own config file.

I had originally assumed that since ripgrep broadly followed git's example wrt to ignore files it might do the same wrt config settings where users are likely to have very specific and esoteric preferences, but bash aliases should suffice.


---

_Review comment by @samuelcolvin on `src/types.rs` on 2016-10-04 12:14_

good point, changed.


---

_@samuelcolvin reviewed on 2016-10-04 12:14_

---

_Merged by @BurntSushi on 2016-10-04 12:25_

---

_Closed by @BurntSushi on 2016-10-04 12:25_

---

_Comment by @BurntSushi on 2016-10-04 12:25_

@samuelcolvin Gotya. And yes, `ripgrep` may wind up with a config file or something similar, but I want to see how far we can get with just aliases first. :-)

In any case, thank you!


---
