```yaml
number: 3703
title: Allow simple container literals as default values
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: allow-simple-container-literals-as-default-values-pyi
created_at: 2023-03-24T00:02:05Z
updated_at: 2023-03-24T03:16:28Z
url: https://github.com/astral-sh/ruff/pull/3703
synced_at: 2026-01-12T15:55:13Z
```

# Allow simple container literals as default values

---

_@JonathanPlasse_

- Implement PyCQA/flake8-pyi#358

---

_@charliermarsh reviewed on 2023-03-24 00:16_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI011.pyi`:16 on 2023-03-24 00:16_

It looks like nested containers are disallowed -- is that right? And do we have any tests for those?

---

_Comment by @github-actions[bot] on 2023-03-24 00:17_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -244, 0 error(s))

<details><summary>typeshed (+0, -244)</summary>
<p>

```diff
- stdlib/_compression.pyi:20:73: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/_dummy_thread.pyi:10:103: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/_dummy_threading.pyi:65:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/_sitebuiltins.pyi:13:69: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/_sitebuiltins.pyi:13:95: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/argparse.pyi:138:49: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/argparse.pyi:155:49: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/asyncio/windows_utils.pyi:18:71: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/builtins.pyi:1842:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/concurrent/futures/process.pyi:177:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/concurrent/futures/process.pyi:187:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/concurrent/futures/thread.pyi:56:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/configparser.pyi:104:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/configparser.pyi:105:43: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/configparser.pyi:72:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/configparser.pyi:73:43: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/configparser.pyi:88:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/configparser.pyi:89:43: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/copy.pyi:11:69: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/dataclasses.pyi:255:35: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/dataclasses.pyi:274:35: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/dataclasses.pyi:292:35: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/distutils/command/config.pyi:77:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/distutils/fancy_getopt.pyi:34:49: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/email/message.pyi:108:56: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/ftplib.pyi:109:59: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/functools.pyi:85:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/functools.pyi:86:30: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/functools.pyi:90:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/functools.pyi:91:30: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/getopt.pyi:3:67: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/getopt.pyi:4:71: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/http/client.pyi:161:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/http/cookiejar.pyi:102:47: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/importlib/__init__.pyi:12:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/inspect.pyi:453:44: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/inspect.pyi:454:52: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/inspect.pyi:455:42: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/lib2to3/pytree.pyi:60:126: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/locale.pyi:118:33: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/logging/handlers.pyi:181:48: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/mailcap.pyi:9:123: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/mimetypes.pyi:46:53: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/modulefinder.pyi:47:40: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/modulefinder.pyi:48:56: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/context.pyi:75:35: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/dummy/__init__.pyi:53:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/dummy/__init__.pyi:54:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/dummy/__init__.pyi:72:116: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/managers.pyi:153:97: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/managers.pyi:57:68: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/managers.pyi:57:95: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/pool.pyi:118:121: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/pool.pyi:75:35: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/pool.pyi:79:68: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/pool.pyi:79:98: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/pool.pyi:83:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/pool.pyi:84:35: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/process.pyi:20:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/process.pyi:21:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/util.pyi:53:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/multiprocessing/util.pyi:60:71: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pdb.pyi:131:73: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pickle.pyi:124:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pickle.pyi:132:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/platform.pyi:36:60: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/platform.pyi:39:118: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/platform.pyi:39:73: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:116:36: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:117:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:118:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:129:36: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:130:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:139:36: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:140:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:141:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:33:75: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/pydoc.pyi:43:91: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/sched.pyi:33:96: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/sched.pyi:36:97: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/shutil.pyi:185:51: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/smtplib.pyi:114:58: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/smtplib.pyi:115:57: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/smtplib.pyi:137:39: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/smtplib.pyi:138:39: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/smtplib.pyi:145:39: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/smtplib.pyi:146:39: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/sqlite3/dbapi2.pyi:380:63: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/string.pyi:50:60: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/string.pyi:51:65: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:1873:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:1904:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:1936:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:1967:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:1998:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2029:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2062:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2092:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2123:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2153:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2183:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2213:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2245:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2274:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2304:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2333:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2362:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2391:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2421:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2446:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2472:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2497:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2522:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/subprocess.pyi:2547:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/threading.pyi:81:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:1006:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:1103:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:1724:30: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:1749:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:1868:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:1979:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2031:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2111:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2224:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2284:33: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:2285:43: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:2288:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2309:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2335:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2355:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2378:58: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2382:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2404:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2431:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2452:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2475:79: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2494:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2580:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2643:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2747:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2838:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:2910:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:3081:39: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:3095:74: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:3184:40: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:3225:60: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:3241:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:3294:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:3312:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:3452:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:3517:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:507:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:517:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:828:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:864:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:899:41: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/__init__.pyi:923:48: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:923:55: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:923:65: PYI014 Only simple default values allowed for arguments
- stdlib/tkinter/__init__.pyi:950:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/dialog.pyi:15:83: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/simpledialog.pyi:17:30: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:102:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:105:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:108:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:111:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:112:60: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:113:62: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:119:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:125:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:126:52: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:130:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:134:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:138:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:142:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:147:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:150:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:155:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:160:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:165:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:166:53: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:167:74: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:180:61: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:181:64: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:188:66: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:189:69: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:207:74: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:208:71: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:212:64: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:215:53: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:221:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:231:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:239:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:249:56: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:260:53: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:265:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:266:52: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:270:63: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:274:77: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:275:52: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:281:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:282:52: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:290:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:294:44: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:295:42: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:57:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:58:30: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:65:64: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:72:61: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:76:44: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:79:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:80:73: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:84:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:85:52: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:89:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/tkinter/tix.pyi:96:84: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/trace.pyi:37:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/trace.pyi:38:37: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/turtle.pyi:409:98: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/turtle.pyi:679:94: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/types.pyi:557:31: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/types.pyi:563:42: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/typing.pyi:801:72: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/typing_extensions.pyi:248:72: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/unittest/mock.pyi:262:52: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/unittest/mock.pyi:70:34: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/unittest/mock.pyi:77:29: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/unittest/suite.pyi:11:53: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/urllib/request.pyi:105:45: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/weakref.pyi:56:103: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xml/dom/domreg.pyi:8:102: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xml/dom/minidom.pyi:223:28: PYI014 Only simple default values allowed for arguments
- stdlib/xml/sax/__init__.pyi:31:50: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xml/sax/__init__.pyi:39:46: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xml/sax/saxutils.pyi:7:53: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xml/sax/saxutils.pyi:8:55: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xml/sax/saxutils.pyi:9:56: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xmlrpc/client.pyi:233:120: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xmlrpc/client.pyi:262:50: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xmlrpc/client.pyi:291:50: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xmlrpc/server.pyi:111:36: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xmlrpc/server.pyi:112:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/xmlrpc/server.pyi:113:38: PYI011 [*] Only simple default values allowed for typed arguments
- stdlib/zipfile.pyi:214:74: PYI011 [*] Only simple default values allowed for typed arguments
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.6±0.10ms     3.0 MB/sec    1.03     14.0±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.02      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.5±1.05µs     6.0 MB/sec    1.01    493.8±0.78µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.3 MB/sec    1.03      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.04ms     5.6 MB/sec    1.06      7.7±0.02ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1637.8±27.05µs    10.2 MB/sec    1.04   1695.1±7.06µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.9±2.07µs    16.5 MB/sec    1.04    186.2±0.65µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.05      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     15.9±0.20ms     2.6 MB/sec    1.00     15.2±0.12ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   538.4±12.84µs     5.5 MB/sec    1.00    535.1±7.92µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.9±0.07ms     3.7 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.05      8.7±0.10ms     4.7 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1897.3±26.30µs     8.8 MB/sec    1.00  1819.6±29.47µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    205.5±3.91µs    14.4 MB/sec    1.00    201.8±5.55µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.04ms     6.4 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@JonathanPlasse reviewed on 2023-03-24 00:18_

---

_Review comment by @JonathanPlasse on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI011.pyi`:16 on 2023-03-24 00:18_

- Yes, I will add the added tests from PyCQA/flake8-pyi#358.

---

_Comment by @JonathanPlasse on 2023-03-24 00:48_

> typeshed (+0, -244)

Nice to see.

---

_Merged by @charliermarsh on 2023-03-24 02:51_

---

_Closed by @charliermarsh on 2023-03-24 02:51_

---

_Comment by @charliermarsh on 2023-03-24 02:51_

Thank you!!

---

_Branch deleted on 2023-03-24 03:16_

---
