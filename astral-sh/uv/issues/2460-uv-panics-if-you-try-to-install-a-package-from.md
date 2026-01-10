```yaml
number: 2460
title: "`uv` panics if you try to install a package from git using a web URL and you incorrectly put two colons after `https`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
assignees: []
created_at: 2024-03-14T14:52:26Z
updated_at: 2024-03-14T17:47:17Z
url: https://github.com/astral-sh/uv/issues/2460
synced_at: 2026-01-10T05:40:32Z
```

# `uv` panics if you try to install a package from git using a web URL and you incorrectly put two colons after `https`

---

_Issue opened by @AlexWaygood on 2024-03-14 14:52_

```zsh
(.venv) [101] % RUST_BACKTRACE=full uv pip install 'uv @ git+https:://github.com/astral-sh/uv.git'                                                                                                 ~/dev/typeshed
⠋ Resolving dependencies...                                                                                                                                                                                       ⠙ Resolving dependencies...                                                                                                                                                                                       :52:
called `Option::unwrap()` on a `None` value
stack backtrace:
   0:        0x10558bea0 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::hb478ebbfb46e27ce
   1:        0x1055afb00 - core::fmt::write::he4d5fa2daff1f531
   2:        0x105587ff8 - std::io::stdio::_eprint::h1c951d35316f84c2
   3:        0x10558bcd4 - <std::time::SystemTimeError as core::fmt::Display>::fmt::h12f69f0587879e14
   4:        0x10558d698 - std::panicking::default_hook::h1e49abbb3f1d7dbf
   5:        0x10558d3e0 - std::panicking::default_hook::h1e49abbb3f1d7dbf
   6:        0x10558dae0 - std::panicking::rust_panic_with_hook::h1e70c5d905e30e9d
   7:        0x10558d9a8 - <std::panicking::begin_panic_handler::StaticStrPayload as core::panic::PanicPayload>::take_box::hd628570880feb11f
   8:        0x10558c324 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::hb478ebbfb46e27ce
   9:        0x10558d75c - _rust_begin_unwind
  10:        0x10561c134 - core::panicking::panic_fmt::h33e40d2a93cab78f
  11:        0x10561c1bc - core::panicking::panic::h57fd475c037a9df3
  12:        0x10502e258 - cache_key::canonical_url::CanonicalUrl::new::h6e9457242f6e0c70
  13:        0x104dbc34c - uv_resolver::resolver::urls::Urls::from_manifest::hbde111ec4f3216e8
  14:        0x104dbc19c - uv_resolver::resolver::urls::Urls::from_manifest::hbde111ec4f3216e8
  15:        0x104db0664 - uv_resolver::pubgrub::dependencies::PubGrubDependencies::push::h497f790fb9fe3a30
  16:        0x104daedd4 - uv_resolver::pubgrub::dependencies::PubGrubDependencies::from_requirements::h8cfcfaf0bbf534e5
  17:        0x104ac9e58 - __mh_execute_header
  18:        0x104ac39ec - __mh_execute_header
  19:        0x104955500 - __mh_execute_header
  20:        0x104911644 - __mh_execute_header
  21:        0x104a24a1c - __mh_execute_header
  22:        0x104a55060 - __mh_execute_header
  23:        0x104a6ea94 - __mh_execute_header
  24:        0x104a45d9c - __mh_execute_header
  25:        0x104aadb80 - __mh_execute_header
  26:        0x104b0c2d8 - __mh_execute_header
  27:        0x104c2b830 - __mh_execute_header
  28:        0x104a28424 - __mh_execute_header
  29:        0x104c07558 - __mh_execute_header
  30:        0x10557eccc - std::rt::lang_start_internal::hf4f3eb1e51305b96
  31:        0x104c61460 - _main
```

---

_Label `bug` added by @AlexWaygood on 2024-03-14 14:52_

---

_Comment by @AlexWaygood on 2024-03-14 14:56_

(This is using uv==0.1.20)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-14 14:58_

---

_Comment by @charliermarsh on 2024-03-14 15:00_

Oh, the URL is wrong. There are two colons after `https`.

---

_Comment by @charliermarsh on 2024-03-14 15:00_

But we shouldn't panic.

---

_Renamed from "`uv` panics if you try to use `uv` to install `uv` from git" to "`uv` panics if you try to install a package from git using a web URL and you put two colons after `https`" by @AlexWaygood on 2024-03-14 15:30_

---

_Renamed from "`uv` panics if you try to install a package from git using a web URL and you put two colons after `https`" to "`uv` panics if you try to install a package from git using a web URL and you incorrectly put two colons after `https`" by @AlexWaygood on 2024-03-14 15:33_

---

_Closed by @charliermarsh on 2024-03-14 17:47_

---
