---
number: 2294
title: Consider using idiomatic Rust errors
type: issue
state: closed
author: sharpvik
labels: []
assignees: []
created_at: 2021-01-17T04:11:59Z
updated_at: 2021-01-17T16:06:58Z
url: https://github.com/clap-rs/clap/issues/2294
synced_at: 2026-01-10T01:27:15Z
---

# Consider using idiomatic Rust errors

---

_Issue opened by @sharpvik on 2021-01-17 04:11_

### Describe your use case

Currently, the errors look like this:

![clap](https://user-images.githubusercontent.com/23066595/104830700-16b24100-5879-11eb-9999-6f782518ff93.png)

With the colon in **`error:`** being red just like the error label itself. However, the idiomatic Rust compiler errors look like this:

![idioma](https://user-images.githubusercontent.com/23066595/104830722-482b0c80-5879-11eb-9d87-a4734c751d41.png)

### Describe the solution you'd like

It is a very minor thing, but it causes stylistic incoherence in projects that adhere to the idiomatic error style. Particularly, a few of my projects. (As you can see, I am a perfectionist... I know, sorry.) It would be great if this thing got fixed. There are libraries out there that could potentially make it easier (e.g. [idioma](https://docs.rs/idioma/))



---

_Label `T: new feature` added by @sharpvik on 2021-01-17 04:11_

---

_Comment by @ldm0 on 2021-01-17 04:19_

Feel free to send a PR as this is only a minor stylistic incoherence.

---

_Referenced in [clap-rs/clap#2295](../../clap-rs/clap/pulls/2295.md) on 2021-01-17 06:04_

---

_Comment by @pksunkara on 2021-01-17 11:19_

Yes, it is stylistic, but I would still like the see others reasons for it. "Rust does it" is not good enough because rust is not primarily CLI tool.

Is there research into famous CLI parsers from other languages and how they print the error messages when color is turned on? If there's a convention there, we will follow.

---

_Comment by @sharpvik on 2021-01-17 16:00_

> Is there research into famous CLI parsers from other languages and how they print the error messages when color is turned on? If there's a convent

@pksunkara it looks like colorized out is not a widely supported function. Argparse and the like don't support it out of the box. So it looks like you can decide how exactly you want to colorize the output of Clap since you are the authors.

I just think that Rust's errors look very nice and why not follow that convention out of all others.

---

_Closed by @pksunkara on 2021-01-17 16:06_

---
