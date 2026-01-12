```yaml
number: 2068
title: Named Capture Groups
type: issue
state: closed
author: balupton
labels: []
assignees: []
created_at: 2021-11-15T01:32:29Z
updated_at: 2021-11-15T01:41:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2068
synced_at: 2026-01-12T16:13:24Z
```

# Named Capture Groups

---

_@balupton_

#### Describe your feature request

It would be nice if ripgrep supported ECMAScript's Named Capture Groups:


``` javascript
let personList = `First_Name: John, Last_Name: Doe
First_Name: Jane, Last_Name: Smith`;

let regexpNames =  /First_Name: (?<firstname>\w+), Last_Name: (?<lastname>\w+)/mg;
let match = regexpNames.exec(personList);
do {
  console.log(`Hello ${match.groups.firstname} ${match.groups.lastname}`);
} while((match = regexpNames.exec(personList)) !== null);
```


---

_Comment by @balupton on 2021-11-15 01:34_

Nevermind, I'm an idiot. I didn't realise that this supports the rust regex format, of which named capture groups are supported, just a different syntax: https://docs.rs/regex/1.5.4/regex/#example-replacement-with-named-capture-groups

---

_Closed by @balupton on 2021-11-15 01:34_

---

_Reopened by @balupton on 2021-11-15 01:38_

---

_Comment by @balupton on 2021-11-15 01:38_

Actually, seems it doesn't work:

``` bash
echo "2012-03-14, 2013-01-01 and 2014-07-05" | rg '(?P<y>\d{4})-(?P<m>\d{2})-(?P<d>\d{2})' --replace "$m/$d/$y"
# outputs: //, // and //
```

---

_Comment by @balupton on 2021-11-15 01:40_

nevermind, still an idiot, needed single quotes around the replacement:

``` bash
echo "2012-03-14, 2013-01-01 and 2014-07-05" | rg '(?P<y>\d{4})-(?P<m>\d{2})-(?P<d>\d{2})' --replace '$m/$d/$y'
```

---

_Closed by @balupton on 2021-11-15 01:40_

---

_Comment by @BurntSushi on 2021-11-15 01:41_

No it does work. Use single quotes. Otherwise your shell interpolates your variables.

---
