```yaml
number: 5792
title: Value delimiter and num_args should work together
type: issue
state: closed
author: eserilev
labels:
  - C-enhancement
assignees: []
created_at: 2024-10-29T05:28:58Z
updated_at: 2024-10-29T21:33:34Z
url: https://github.com/clap-rs/clap/issues/5792
synced_at: 2026-01-12T16:14:17Z
```

# Value delimiter and num_args should work together

---

_@eserilev_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4

### Describe your use case

I've recently learned that `num_args` and `value_delimiter` don't work as I've imagined when used together

I would assume if I provided

`num_args=0..=2`
and
`value_delimiter=','`

I would be able to enforce 0 to 2 comma separated args. However this isn't the case...

Based on the discussion [here](https://github.com/clap-rs/clap/discussions/4469) `value_delimiter` doesnt support `num_args` in the case of variable arg limits (i.e. `0..=2`)


This seems odd to me, and forces me to write my own post processing logic. In my opinion supporting variable arg limits w/ a custom value delimiter is a pretty standard use case in CLI land, and it would be really nice if clap could support this out of the box.

### Describe the solution you'd like

I would like num_args and value_parser to work together, or alternatively provide some other attribute that I can use to easily set variable arg limits on a flag w/ a custom value delimiter.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @eserilev on 2024-10-29 05:28_

---

_Comment by @epage on 2024-10-29 16:20_

`num_args` controls how many values of `std::env::args_os()` to capture and to reduce the chance of confusion is why we didn't name it `num_values`.

Previously, we did have `min_values` and `max_values` which were about how many values total are resent for an argument.  This was across occurrences.

Considering we have
- Validation for number of arguments (which also affects parsing)
- Validation across occurrences
- Validation within occurrences

And trying to manage these, it doesn't quite work to cover every case especially since we are also trying to weigh out
- Binary size
- Build times
- API bloat (the larger the API, the harder it is to discover whats there, the less value people get out of the API)

Besides post-validation, this an also be handled by a custom `value_parser` and, depending on the use case, might be more appropriate as controlling for the number of values suggests more structured data and we have value_delimiter more geared towards unstructured data.  value_parser works much better for that.

We've also been exploring the idea of user-extended validation, see #3476

In light of this, I'm going to go ahead and close this. If there is a reason we should revisit this decision, let us know!

---

_Closed by @epage on 2024-10-29 16:20_

---

_Comment by @eserilev on 2024-10-29 21:30_

Thanks for the detailed response, its much appreciated

re: custom `value_parser` as far as I can understand value parser gets called for each argument

i.e. --some-flag val1,val2,val3
value parser will be called three times (for val1 val2 and val3 respectively). Is there a way to get clap to execute some custom value parsing method once for the range of values?

---

_Comment by @epage on 2024-10-29 21:33_

If you don't pass a `value_delimiter`, then `value_parser` will be called once and handle the delimited values.

---
