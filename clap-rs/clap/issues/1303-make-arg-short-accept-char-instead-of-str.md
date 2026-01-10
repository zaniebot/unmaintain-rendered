---
number: 1303
title: "Make arg.short accept char instead of &str"
type: issue
state: closed
author: Pzixel
labels:
  - M-breaking-change
  - E-medium
  - E-easy
assignees: []
created_at: 2018-06-24T18:31:28Z
updated_at: 2018-08-02T03:30:25Z
url: https://github.com/clap-rs/clap/issues/1303
synced_at: 2026-01-10T01:26:48Z
---

# Make arg.short accept char instead of &str

---

_Issue opened by @Pzixel on 2018-06-24 18:31_

I'm writing an app using clap and it's wonderful. But I've encountered a problem with flags. I used this code:

```rust
let matches = App::new("App")
	.arg(
		Arg::with_name("externalAddress")
			.short("ea")
			.long("externalAddress")
			.help("Sets the external address where webhook should be setted up")
			.takes_value(true)
			.required(true),
	)
	.get_matches();
```

The problem here is that when I pass `ea` to the app I get a runtime error.

I have found the answer relatively quick but watching to the source code and documentation which says:

> To set short use a single valid UTF-8 code point.

If it have to be a single UTF-8 code point then it's better to have `char` parameter instead, which would allow to have a compile time error instead of silently trim the rest of the string out:

```rust
pub fn short<S: AsRef<str>>(mut self, s: S) -> Self {
	self.s.short = s.as_ref().trim_left_matches(|c| c == '-').chars().nth(0);
	self
}
```


---

_Comment by @kbknapp on 2018-06-26 14:55_

I agree. This will be changed in v3.

I think initially I was planning on supporting `-foo` at some point. Although now that proposal has morphed.

---

_Label `W: 3.x` added by @kbknapp on 2018-06-26 14:55_

---

_Label `E: breaking change` added by @kbknapp on 2018-07-22 02:44_

---

_Label `P2: need to have` added by @kbknapp on 2018-07-22 02:44_

---

_Label `C: args` added by @kbknapp on 2018-07-22 02:44_

---

_Label `D: easy` added by @kbknapp on 2018-07-22 02:44_

---

_Label `M: mentored` added by @kbknapp on 2018-07-22 02:44_

---

_Label `T: refactor` added by @kbknapp on 2018-07-22 02:44_

---

_Label `good first issue` added by @kbknapp on 2018-07-22 02:44_

---

_Added to milestone `v3-alpha.1` by @kbknapp on 2018-07-22 02:44_

---

_Comment by @kbknapp on 2018-07-23 19:10_

Implemented on v3-master

---

_Closed by @kbknapp on 2018-07-23 19:10_

---
