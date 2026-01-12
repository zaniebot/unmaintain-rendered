```yaml
number: 891
title: Implementing .format checks
type: issue
state: closed
author: olliemath
labels:
  - rule
assignees: []
created_at: 2022-11-23T13:15:32Z
updated_at: 2022-11-25T18:14:32Z
url: https://github.com/astral-sh/ruff/issues/891
synced_at: 2026-01-12T15:54:40Z
```

# Implementing .format checks

---

_@olliemath_

Hi, I'm interested in contributing pyflakes' `.format(...)` checks to ruff. These can give runtime errors (especially, in rarely hit code paths, like trying to give nice exception messages), so I think they'd be necessary to have for my team to move from flake8 to ruff.

As far as I can tell, the hard part is getting a rusty equivalent of python's `_string.formatter_parser`, which is implicitly used in pyflakes: https://github.com/PyCQA/pyflakes/blob/master/pyflakes/checker.py#L26

In RustPython, the logic is in [vm/src/format.rs](https://github.com/RustPython/RustPython/blob/787d9d0bbb59da79fa061b870c234314877ff9b0/vm/src/format.rs#L671) would it be acceptable to add `rustpython-vm` as a dependency in order to use the parser? Specifically the logic would then look something like
```rust
match FormatString::from_str(text.as_str()) {
    Err(e) => // bad format string .. handle or ignore
    Ok(formatter) => {
        some_check_of_the_format_parts(formatter.format_parts)
    }
}
```

The alternative is to re-implement a minimal parser by hand, taking inspiration from the [RustPython](https://github.com/RustPython/RustPython/blob/787d9d0bbb59da79fa061b870c234314877ff9b0/vm/src/format.rs#L671), [CPython](https://github.com/python/cpython/blob/135ec7cefbaffd516b77362ad2b2ad1025af462e/Objects/stringlib/unicode_format.h#L654) and [PyPy](https://foss.heptapod.net/pypy/pypy/-/blob/branch/py3.8/pypy/objspace/std/newformat.py#L47) implementations. This would be far more lightweight, but carries a few more risks and probably a bit of maintenance overhead with it.

Once the formatter parser is implemented, the checks themselves are quite simple.

In pyflakes they're inter-dependent and implemented in a single large function. That leads to some strange edge cases (for example a format string which fails both F525 and F522 will return no errors if you disable F525). My aim would be to implement them as separate functions for sanity and clarity. This would be a mild departure from Flake8's behaviour (i.e. we won't reproduce what I regard as bugs) and might involve a few more CPU cycles than implementing all the format checks in a single pass. Taking a brief look at `check_ast.rs` I think this approach would also fit with the style used there.

Let me know your thoughts and if you have any guiding principles on the architecture that should influence the implementation.

#### Sidenote: percent formatting checks

For percent formatting, pyflakes implement their own parser (copied from pyupgrade actually). This could pretty easily be translated into rust, assuming a rustpython equivalent of `FormatString` is not available for percent formatting.

---

_Label `rule` added by @charliermarsh on 2022-11-23 14:53_

---

_Comment by @charliermarsh on 2022-11-23 15:02_

Awesome! This would be very welcome and I'm happy to support you in any way I can. (In the first sentence, you said "pylint's `.format(...)`" checks -- did you mean Pyflakes? Just confirming.)

I think I'd rather vendor (i.e., copy over) `format.rs` than add `rustpython-vm` as a dependency since `rustpython-vm` itself is a pretty hefty crate. We could also deviate a bit from `format.rs` (it doesn't have to be a 1:1 copy) if it facilitates the implementation in any way. I'm also open to implementing our own port in Rust, if that ends up being easier -- I admittedly haven't read `format.rs` closely, but in general, I'd rather have our own thing than pull in `rustpython-vm`, so whatever's easiest there is fine with me as long as we have decent test coverage.


---

_Comment by @charliermarsh on 2022-11-23 15:03_

It makes sense to me, too, to split these into separate checks, similar to how docstrings are handled, even if there's a bit of repeated work. Your summary suggests to me that you have good intuition on where the Pyflakes checks are useful vs. confusing :)


---

_Comment by @olliemath on 2022-11-23 15:20_

> did you mean Pyflakes? Just confirming.)

I did indeed!

> I think I'd rather vendor (i.e., copy over) `format.rs` than add `rustpython-vm` as a dependency since `rustpython-vm` itself is a pretty hefty crate.

OK, cool - so my first step will be adding a vendored version of the functions from `format.rs` with some good test coverage.
If there's an obvious way to make them a bit lighter (only pulling out information present in the checks) I'll deviate a bit, but try not to reinvent the wheel.

I was thinking to place them in `src/pyflakes/helpers.rs` unless you have a better location in mind? Maybe `src/vendored/format.rs`?

---

_Comment by @charliermarsh on 2022-11-23 15:22_

Maybe `pyflakes/format.rs`? Or `pyflakes/strings.rs`? But not picky.

---

_Closed by @charliermarsh on 2022-11-25 18:14_

---
