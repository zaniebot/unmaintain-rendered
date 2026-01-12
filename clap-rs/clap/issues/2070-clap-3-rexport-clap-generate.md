```yaml
number: 2070
title: Clap 3 rexport clap_generate
type: issue
state: closed
author: nikoneufeld
labels: []
assignees: []
created_at: 2020-08-14T14:43:57Z
updated_at: 2020-08-14T15:11:57Z
url: https://github.com/clap-rs/clap/issues/2070
synced_at: 2026-01-12T16:14:12Z
```

# Clap 3 rexport clap_generate

---

_@nikoneufeld_

### Make sure you completed the following tasks

- [ ] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [ ] Searched the closed issues

### Describe your use case

Describe the problem you're trying to solve. This is not mandatory and we *do* consider features without a specific use case, but real problems have priority.

Also, please follow your explanation with a snippet of code, if applicable.

**If a project of yours is blocked on this feature, please, mention it explicitely.**

### Describe the solution you'd like

Please explain what the wanted solution should look like. You are **strongly encouraged** to attach a snippet of (pseudo)code.

### Alternatives, if applicable

A clear and concise description of any alternative solutions or features you've managed to come up with.

### Additional context

Add any other context about the feature request here.


---

_Label `T: new feature` added by @nikoneufeld on 2020-08-14 14:43_

---

_Comment by @pksunkara on 2020-08-14 14:45_

Please actually describe your problem.

---

_Comment by @nikoneufeld on 2020-08-14 14:59_

Sorry I was rushing a bit :-).


I will explain

---

_Comment by @nikoneufeld on 2020-08-14 15:00_

I want that when you import clap v3, you get clap, clap derive _**and clap_generate**_.

Also, I know that there is also an issue about it but, are man pages going to come to clap?.





---

_Comment by @nikoneufeld on 2020-08-14 15:03_

I also saw that there is a man page implementation on [https://github.com/clap-rs/clap_generate/blob/master/src/manual.rs](url) but not on https://github.com/clap-rs/clap/tree/master/clap_generate.

---

_Comment by @pksunkara on 2020-08-14 15:10_

> I want that when you import clap v3, you get clap, clap derive and clap_generate.

That is not possible. `clap_generate` relies on `clap`. But we do have a separate issue to make it easy to use.

> Also, I know that there is also an issue about it but, are man pages going to come to clap?.
> I also saw that there is a man page implementation ...

We decided not to generate manpages because manpages are not meant to be generated but rather written. If you really want one, you can use our `--help` and then use `help2man` command to convert the output to manpages.

---

_Closed by @pksunkara on 2020-08-14 15:10_

---

_Comment by @nikoneufeld on 2020-08-14 15:11_

So then, how do I get clap generate.

---

_Comment by @nikoneufeld on 2020-08-14 15:11_

Or use from clap

---
