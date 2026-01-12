```yaml
number: 145
title: "I'm getting \"invalid argument\" with trying to add new types"
type: issue
state: closed
author: samuelcolvin
labels: []
assignees: []
created_at: 2016-10-04T10:45:23Z
updated_at: 2016-10-04T11:13:54Z
url: https://github.com/BurntSushi/ripgrep/issues/145
synced_at: 2026-01-12T18:23:11Z
```

# I'm getting "invalid argument" with trying to add new types

---

_@samuelcolvin_

Best described with terminal output

``` shell
> rg --type-add 'jinja:*.{jinja,jinja2}'
Invalid arguments.

Usage: rg [options] -e PATTERN ... [<path> ...]
       rg [options] <pattern> [<path> ...]
       rg [options] --files [<path> ...]
       rg [options] --type-list
       rg [options] --help
       rg [options] --version
> echo $?
1
> rg --version
0.2.1
```

However `--type-add` works fine with combined with a search, eg. `rg --type-add 'jinja:*.{jinja,jinja2}'  -tjinja foobar`, but that doesn't add the type to the global list and has to be used on every search.

OS: ubuntu 16.04, kernel `4.4.0-36-generic`.


---

_Comment by @BurntSushi on 2016-10-04 11:09_

> but that doesn't add the type to the global list and has to be used on every search.

There is no global list or persistent configuration that `ripgrep` writes. You need to pass `--type-add` to each invocation. You might consider using aliases to make this less painful.

I would very much appreciate it if you could tell me where the docs have led you astray. :-( How can I fix them?


---

_Comment by @samuelcolvin on 2016-10-04 11:13_

Docs are clear now I look more properly, just me being dumb.

Would you accept a pull request to add jinja files to the types?


---

_Closed by @samuelcolvin on 2016-10-04 11:13_

---

_Comment by @BurntSushi on 2016-10-04 11:13_

@samuelcolvin Absolutely. Thanks. :-)


---
