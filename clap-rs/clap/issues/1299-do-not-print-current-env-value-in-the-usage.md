---
number: 1299
title: Do not print current env value in the usage
type: issue
state: closed
author: zrzka
labels: []
assignees: []
created_at: 2018-06-18T07:15:36Z
updated_at: 2018-08-02T03:30:25Z
url: https://github.com/clap-rs/clap/issues/1299
synced_at: 2026-01-10T01:26:48Z
---

# Do not print current env value in the usage

---

_Issue opened by @zrzka on 2018-06-18 07:15_

I have a long list of configuration options and you can set values via short, long variants along with environment variables as well. Application documentation for operations includes the complete usage (`--help`) as well with additional comments. Usage is a simple copy & paste of `--help` output from my side.

Others found (during documentation review), that we have accidentally leaked some credentials. Not a big deal, test environment and credentials were rotated immediately. How this happened?

* Environment variables were set.
* `--help` usage printed.
* Copy & pasted to the documentation.

And because `--help` prints environment variables values along with configuration options, these values were copy & pasted as well.

Is there a way how to disable this behavior? Do not print current environment variables values in the usage even if they're set?

---

_Comment by @kbknapp on 2018-06-22 01:12_

This can be done with [`Arg::hide_env_values(true)`](https://docs.rs/clap/2.31.2/clap/struct.Arg.html#method.hide_env_values) for the argument you wish to hide the values of.

If this doesn't fix it, feel free to re-open and we can re-address :wink:

---

_Closed by @kbknapp on 2018-06-22 01:12_

---

_Label `T: RFC / question` added by @kbknapp on 2018-06-22 01:12_

---

_Comment by @kbknapp on 2018-06-22 01:13_

It looks like I forgot to add the docs (:worried: ) for that item! Apologies! It should be fairly self explanatory, but if you need assistance please don't hesitate to ask and ping me here or in the Gitter channel!

---

_Comment by @zrzka on 2018-06-22 05:13_

Mea culpa, how did I miss this. Thanks!

---
