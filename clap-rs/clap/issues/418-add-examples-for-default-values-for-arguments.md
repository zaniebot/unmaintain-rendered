```yaml
number: 418
title: Add examples for Default values for arguments
type: issue
state: closed
author: Fiedzia
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2016-02-05T14:54:02Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/418
synced_at: 2026-01-12T16:14:09Z
```

# Add examples for Default values for arguments

---

_@Fiedzia_

Its really excellent library, but I am missing ability to define default value for arguments. Could this be added?


---

_Comment by @sru on 2016-02-05 17:11_

Since `value_of` from `ArgMatches` struct returns `Option`, you can use `unwrap_or` function. i.e.

``` rust
let s = matches.value_of("something").unwrap_or("default");
```


---

_Label `T: RFC / question` added by @sru on 2016-02-05 17:11_

---

_Comment by @Fiedzia on 2016-02-05 17:31_

Sure, but if I'm accessing this in 5 places (say every time I'm checking verbosity), I'll have to define default  value in those 5 places, and in documentation, and then make sure all those places are in sync. Having it defined once would be nicer.


---

_Comment by @sru on 2016-02-05 19:50_

You don't have to define those default value in 5 different places, you'd just define one constant and use it over those five different places. I believe having default value has been brought up previously (#367 briefly). I think adding default value would be simple enough addition though I am skeptical if it is actually needed. Need @kbknapp to chime in.


---

_Comment by @kbknapp on 2016-02-05 20:46_

I'm not opposed to adding it, as with everything else in clap it'd be opt in and there's really no overhead to using it or it being there if you don't use it. Short of a quick cycle through the defaults if they're not already provided.

Like @sru said I personally think there are ways get by without it, but it does seem to be something that I'm asked about from time to time. 

I'll add this to issue tracker for implementing.

Thanks for taking the time to file this ;)


---

_Label `T: new feature` added by @kbknapp on 2016-02-05 21:04_

---

_Label `P4: nice to have` added by @kbknapp on 2016-02-05 21:04_

---

_Label `C: args` added by @kbknapp on 2016-02-05 21:04_

---

_Label `D: easy` added by @kbknapp on 2016-02-05 21:04_

---

_Label `C: matches` added by @kbknapp on 2016-02-05 21:04_

---

_Label `C: parsing` added by @kbknapp on 2016-02-05 21:04_

---

_Label `W: 2.x` added by @kbknapp on 2016-02-05 21:04_

---

_Label `M: mentored` added by @kbknapp on 2016-02-05 21:04_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-02-05 21:04_

---

_Comment by @kbknapp on 2016-02-09 18:31_

I have a local branch with this implemented - once #422 merges I'll put in the PR. Just waiting because this feature bumps the minor version, but #422 does not.


---

_Comment by @jpastuszek on 2016-02-13 12:33_

I am also looking for default value support. 
Ideally you just define it with .default("<value>") on Arg.
Also it should be displayed in help message in square brackets (?) after help string using Display trait or alternatively if .default_label("<displayed in help>") is used it should display provided string instead.

This is what I do now:

```
    -c, --data-processor-url <URL>     Nanomsg URL to data processor [ipc:///tmp/rdms_data_store.ipc]
```

```
    let url = value_t!(args, "data-processor-url", Url).unwrap_or_else(|err|
        match err.kind {
            clap::ErrorKind::ArgumentNotFound => FromStr::from_str("ipc:///tmp/rdms_data_store.ipc").unwrap(),
            _ => err.exit()
        }
    );
```

Is there a better way?


---

_Comment by @kbknapp on 2016-02-14 08:52_

@jpastuszek The ~~just merged~~ soon to merge #423 includes this. It contains a new `Arg::default_value` method for setting the value when the argument is not used at runtime.

The next ~~PR~~ commit will add the adding to help text automatically, good idea.

Edit: after the merge I'll upload v2.1.0 to crates.io


---

_Closed by @homu on 2016-02-14 11:32_

---

_Comment by @davidmyersdev on 2016-09-17 01:49_

@kbknapp is there any way we can get this added to the docs or the examples? I feel like this is a pretty useful feature, and it took me a while to find it. Is there an API reference available for the args commands? I'm loving this project so far. Thanks!


---

_Comment by @kbknapp on 2016-09-17 02:25_

@drm2 absolutely! 


---

_Reopened by @kbknapp on 2016-09-17 02:25_

---

_Renamed from "Default values for arguments" to "Add examples for Default values for arguments" by @kbknapp on 2016-09-17 02:26_

---

_Label `T: enhancement` added by @kbknapp on 2016-09-17 02:27_

---

_Label `C: docs` added by @kbknapp on 2016-09-17 02:27_

---

_Label `C: args` removed by @kbknapp on 2016-09-17 02:27_

---

_Label `C: matches` removed by @kbknapp on 2016-09-17 02:27_

---

_Label `C: parsing` removed by @kbknapp on 2016-09-17 02:27_

---

_Label `M: mentored` removed by @kbknapp on 2016-09-17 02:27_

---

_Label `T: new feature` removed by @kbknapp on 2016-09-17 02:27_

---

_Added to milestone `2.12.2` by @kbknapp on 2016-09-17 02:27_

---

_Comment by @davidmyersdev on 2016-09-17 02:42_

Wow. Incredible response time. Thank you!


---

_Closed by @homu on 2016-09-18 20:54_

---
