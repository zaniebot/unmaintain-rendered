---
number: 285
title: Support positional arguments for Option arguments
type: issue
state: closed
author: hgrecco
labels: []
assignees: []
created_at: 2015-09-29T14:57:53Z
updated_at: 2018-08-02T03:29:45Z
url: https://github.com/clap-rs/clap/issues/285
synced_at: 2026-01-07T13:12:19-06:00
---

# Support positional arguments for Option arguments

---

_Issue opened by @hgrecco on 2015-09-29 14:57_

I am trying to support the following syntax:

```
cmd subcmd --myoption val1 val2 val3
```

where val1, val2 and val3 are mandatory after myoption. I tried using the following YAML file:

```
          - myoption:
              long: myoption
              args:                 
                - val1:
                    index: 1
                - val2:
                    index: 2
                - val3:
                    index: 3
```

but it panics with the following message: 

> 'Unknown Arg setting 'args' in YAML file for arg 'myoption''

Is there any way to achieve this?

(I apologize if this is already supported but I was not able to find out how)


---

_Comment by @sru on 2015-09-29 16:52_

You can use `value_names`:

``` yaml
- myoption:
    long: myoption
    takes_value: true # this indicates that this is an option not a flag
    multiple: true    # this is needed to take multiple values
    value_names:
      - val1
      - val2
      - val3
```


---

_Label `T: RFC / question` added by @sru on 2015-09-29 16:53_

---

_Closed by @hgrecco on 2015-09-29 23:53_

---
