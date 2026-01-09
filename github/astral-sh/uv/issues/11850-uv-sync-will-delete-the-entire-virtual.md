---
number: 11850
title: "`uv sync` will delete the entire virtual environment and recreate it every time"
type: issue
state: closed
author: rainzee
labels:
  - needs-mre
assignees: []
created_at: 2025-02-28T12:35:04Z
updated_at: 2025-02-28T14:06:11Z
url: https://github.com/astral-sh/uv/issues/11850
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv sync` will delete the entire virtual environment and recreate it every time

---

_Issue opened by @rainzee on 2025-02-28 12:35_

### Summary

![Image](https://github.com/user-attachments/assets/e5510747-bd43-4283-b45b-1252c8682de5)

### Platform

Windows 10

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

CPython 3.12.9

---

_Label `bug` added by @rainzee on 2025-02-28 12:35_

---

_Comment by @konstin on 2025-02-28 12:39_

Can you share verbose logs (as text, not as screenshot), and how did you install Python? See #9452 for more information on the information we need from bug reports.

---

_Label `bug` removed by @konstin on 2025-02-28 12:39_

---

_Label `needs-mre` added by @konstin on 2025-02-28 12:39_

---

_Comment by @rainzee on 2025-02-28 12:42_

> Can you share verbose logs (as text, not as screenshot), and how did you install Python? See [#9452](https://github.com/astral-sh/uv/issues/9452) for more information on the information we need from bug reports.

i use https://github.com/astral-sh/python-build-standalone and just add it to path

---

_Comment by @rainzee on 2025-02-28 12:43_

```
atlas is 📦 v0.1.0 via 🐍 v3.12.9
❯ uv --version
uv 0.6.3 (a0b9f22a2 2025-02-24)

atlas is 📦 v0.1.0 via 🐍 v3.12.9
❯ uv sync
Using CPython 3.12.9 interpreter at: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 19 packages in 0.92ms
Installed 18 packages in 840ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + colorama==0.4.6
 + dynamsoft-barcode-reader-bundle==10.5.2100
 + greenlet==3.1.1
 + idna==3.10
 + loguru==0.7.3
 + msgspec==0.19.0
 + pyside6==6.8.2.1
 + pyside6-addons==6.8.2.1
 + pyside6-essentials==6.8.2.1
 + pyzbar==0.1.9
 + requests==2.32.3
 + shiboken6==6.8.2.1
 + sqlalchemy==2.0.38
 + typing-extensions==4.12.2
 + urllib3==2.3.0
 + win32-setctime==1.2.0

atlas is 📦 v0.1.0 via 🐍 v3.12.9
❯ uv sync
Using CPython 3.12.9 interpreter at: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 19 packages in 0.86ms
Installed 18 packages in 806ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + colorama==0.4.6
 + dynamsoft-barcode-reader-bundle==10.5.2100
 + greenlet==3.1.1
 + idna==3.10
 + loguru==0.7.3
 + msgspec==0.19.0
 + pyside6==6.8.2.1
 + pyside6-addons==6.8.2.1
 + pyside6-essentials==6.8.2.1
 + pyzbar==0.1.9
 + requests==2.32.3
 + shiboken6==6.8.2.1
 + sqlalchemy==2.0.38
 + typing-extensions==4.12.2
 + urllib3==2.3.0
 + win32-setctime==1.2.0

atlas is 📦 v0.1.0 via 🐍 v3.12.9
❯
```

---

_Comment by @rainzee on 2025-02-28 12:44_

> ```
> atlas is 📦 v0.1.0 via 🐍 v3.12.9
> ❯ uv --version
> uv 0.6.3 (a0b9f22a2 2025-02-24)
> 
> atlas is 📦 v0.1.0 via 🐍 v3.12.9
> ❯ uv sync
> Using CPython 3.12.9 interpreter at: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
> Removed virtual environment at: .venv
> Creating virtual environment at: .venv
> Resolved 19 packages in 0.92ms
> Installed 18 packages in 840ms
>  + certifi==2025.1.31
>  + charset-normalizer==3.4.1
>  + colorama==0.4.6
>  + dynamsoft-barcode-reader-bundle==10.5.2100
>  + greenlet==3.1.1
>  + idna==3.10
>  + loguru==0.7.3
>  + msgspec==0.19.0
>  + pyside6==6.8.2.1
>  + pyside6-addons==6.8.2.1
>  + pyside6-essentials==6.8.2.1
>  + pyzbar==0.1.9
>  + requests==2.32.3
>  + shiboken6==6.8.2.1
>  + sqlalchemy==2.0.38
>  + typing-extensions==4.12.2
>  + urllib3==2.3.0
>  + win32-setctime==1.2.0
> 
> atlas is 📦 v0.1.0 via 🐍 v3.12.9
> ❯ uv sync
> Using CPython 3.12.9 interpreter at: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
> Removed virtual environment at: .venv
> Creating virtual environment at: .venv
> Resolved 19 packages in 0.86ms
> Installed 18 packages in 806ms
>  + certifi==2025.1.31
>  + charset-normalizer==3.4.1
>  + colorama==0.4.6
>  + dynamsoft-barcode-reader-bundle==10.5.2100
>  + greenlet==3.1.1
>  + idna==3.10
>  + loguru==0.7.3
>  + msgspec==0.19.0
>  + pyside6==6.8.2.1
>  + pyside6-addons==6.8.2.1
>  + pyside6-essentials==6.8.2.1
>  + pyzbar==0.1.9
>  + requests==2.32.3
>  + shiboken6==6.8.2.1
>  + sqlalchemy==2.0.38
>  + typing-extensions==4.12.2
>  + urllib3==2.3.0
>  + win32-setctime==1.2.0
> 
> atlas is 📦 v0.1.0 via 🐍 v3.12.9
> ❯
> ```

As you can see in the log above, every time I use the sync command, uv completely removes the entire environment and then creates

---

_Comment by @rainzee on 2025-02-28 12:46_

```
atlas is 📦 v0.1.0 via 🐍 v3.12.9
❯ uv sync --verbose
DEBUG uv 0.6.3 (a0b9f22a2 2025-02-24)
DEBUG Found project root: `C:\Users\rainzee\Dev\Workspaces\atlas`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\Users\rainzee\Dev\Workspaces\atlas`
DEBUG Reading Python requests from version file at `C:\Users\rainzee\Dev\Workspaces\atlas\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version does not satisfy `3.12`
DEBUG Searching for Python 3.12 in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\rainzee\AppData\Roaming\uv\python`
DEBUG Found `cpython-3.12.9-windows-x86_64-none` at `C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe` (first executable in the search path)
Using CPython 3.12.9 interpreter at: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
DEBUG Released lock at `C:\Users\rainzee\AppData\Local\Temp\uv-96f3b64e7f2cc68d.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: atlas @ file:///C:/Users/rainzee/Dev/Workspaces/atlas
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 19 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: dynamsoft-barcode-reader-bundle==10.5.2100
DEBUG Registry requirement already cached: loguru==0.7.3
DEBUG Registry requirement already cached: msgspec==0.19.0
DEBUG Registry requirement already cached: pyside6==6.8.2.1
DEBUG Registry requirement already cached: pyzbar==0.1.9
DEBUG Registry requirement already cached: requests==2.32.3
DEBUG Registry requirement already cached: sqlalchemy==2.0.38
DEBUG Registry requirement already cached: colorama==0.4.6
DEBUG Registry requirement already cached: win32-setctime==1.2.0
DEBUG Registry requirement already cached: pyside6-addons==6.8.2.1
DEBUG Registry requirement already cached: pyside6-essentials==6.8.2.1
DEBUG Registry requirement already cached: shiboken6==6.8.2.1
DEBUG Registry requirement already cached: certifi==2025.1.31
DEBUG Registry requirement already cached: charset-normalizer==3.4.1
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: urllib3==2.3.0
DEBUG Registry requirement already cached: greenlet==3.1.1
DEBUG Registry requirement already cached: typing-extensions==4.12.2
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\py.typed
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\py.typed
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DAnimation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DExtras.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DInput.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DLogic.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DRender.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtAxContainer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtBluetooth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCharts.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtConcurrent.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDataVisualization.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDesigner.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphs.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphsWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGui.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHelp.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHttpServer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtLocation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimedia.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimediaWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetwork.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetworkAuth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNfc.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGL.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGLWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdf.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdfWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPositioning.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPrintSupport.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick3D.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickControls2.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtRemoteObjects.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtScxml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSensors.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialPort.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSpatialAudio.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSql.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtStateMachine.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvg.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvgWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTextToSpeech.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtUiTools.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebChannel.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebSockets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebView.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtXml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_config.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_git_pyside_version.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__feature__.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__init__.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DAnimation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DExtras.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DInput.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DLogic.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DRender.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtAxContainer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtBluetooth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCharts.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtConcurrent.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDataVisualization.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDesigner.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphs.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphsWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGui.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHelp.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHttpServer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtLocation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimedia.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimediaWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetwork.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetworkAuth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNfc.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGL.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGLWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdf.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdfWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPositioning.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPrintSupport.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick3D.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickControls2.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtRemoteObjects.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtScxml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSensors.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialPort.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSpatialAudio.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSql.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtStateMachine.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvg.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvgWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTextToSpeech.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtUiTools.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebChannel.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebSockets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebView.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtXml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_config.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_git_pyside_version.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__feature__.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__init__.py
Installed 18 packages in 799ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + colorama==0.4.6
 + dynamsoft-barcode-reader-bundle==10.5.2100
 + greenlet==3.1.1
 + idna==3.10
 + loguru==0.7.3
 + msgspec==0.19.0
 + pyside6==6.8.2.1
 + pyside6-addons==6.8.2.1
 + pyside6-essentials==6.8.2.1
 + pyzbar==0.1.9
 + requests==2.32.3
 + shiboken6==6.8.2.1
 + sqlalchemy==2.0.38
 + typing-extensions==4.12.2
 + urllib3==2.3.0
 + win32-setctime==1.2.0

atlas is 📦 v0.1.0 via 🐍 v3.12.9
❯
```

---

_Comment by @rainzee on 2025-02-28 13:01_

```
DEBUG The virtual environment's Python version does not satisfy `3.12`
DEBUG Searching for Python 3.12 in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\rainzee\AppData\Roaming\uv\python`
DEBUG Found `cpython-3.12.9-windows-x86_64-none` at `C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe` (first executable in the search path)
Using CPython 3.12.9 interpreter at: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
```

The environment in venv is the target environment, but the environment will still be removed and re-created with the same version

---

_Comment by @rainzee on 2025-02-28 13:14_

Below is the complete log. You can see that the interpreter version in .venv is 3.12.*, but it still outputs a mismatch. Then re-use the same interpreter version to continue building

```
atlas is 📦 v0.1.0 via 🐍 v3.12.9 (atlas) 
❯ .\.venv\Scripts\activate.ps1

atlas is 📦 v0.1.0 via 🐍 v3.12.9 (atlas) 
❯ python --version
Python 3.12.9

atlas is 📦 v0.1.0 via 🐍 v3.12.9 (atlas) 
❯ uv sync --verbose
DEBUG uv 0.6.3 (a0b9f22a2 2025-02-24)
DEBUG Found project root: `C:\Users\rainzee\Dev\Workspaces\atlas`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\Users\rainzee\Dev\Workspaces\atlas`
DEBUG Using Python request `==3.12.*` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version does not satisfy `==3.12.*`
DEBUG Searching for Python ==3.12.* in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\rainzee\AppData\Roaming\uv\python`
DEBUG Found `cpython-3.11.11-windows-x86_64-none` at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv/Scripts\python.exe` (first executable in the search path)
DEBUG Ignoring Python interpreter at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Scripts\python.exe`: system interpreter required
DEBUG Found `cpython-3.11.11-windows-x86_64-none` at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv/Scripts\python.exe` (search path)
DEBUG Ignoring Python interpreter at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Scripts\python.exe`: system interpreter required
DEBUG Found `cpython-3.11.11-windows-x86_64-none` at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv/Scripts\python.exe` (search path)
DEBUG Ignoring Python interpreter at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Scripts\python.exe`: system interpreter required
DEBUG Found `cpython-3.11.11-windows-x86_64-none` at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv/Scripts\python.exe` (search path)
DEBUG Ignoring Python interpreter at `C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Scripts\python.exe`: system interpreter required
DEBUG Found `cpython-3.12.9-windows-x86_64-none` at `C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe` (search path)
Using CPython 3.12.9 interpreter at: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: C:\Users\rainzee\Dev\SDK\Python\core\cpython-3.12.9-x86_64\python.exe
DEBUG Released lock at `C:\Users\rainzee\AppData\Local\Temp\uv-96f3b64e7f2cc68d.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: atlas @ file:///C:/Users/rainzee/Dev/Workspaces/atlas
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 19 packages in 16ms
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: dynamsoft-barcode-reader-bundle==10.5.2100
DEBUG Registry requirement already cached: loguru==0.7.3
DEBUG Registry requirement already cached: msgspec==0.19.0
DEBUG Registry requirement already cached: pyside6==6.8.2.1
DEBUG Registry requirement already cached: pyzbar==0.1.9
DEBUG Registry requirement already cached: requests==2.32.3
DEBUG Registry requirement already cached: sqlalchemy==2.0.38
DEBUG Registry requirement already cached: colorama==0.4.6
DEBUG Registry requirement already cached: win32-setctime==1.2.0
DEBUG Registry requirement already cached: pyside6-addons==6.8.2.1
DEBUG Registry requirement already cached: pyside6-essentials==6.8.2.1
DEBUG Registry requirement already cached: shiboken6==6.8.2.1
DEBUG Registry requirement already cached: certifi==2025.1.31
DEBUG Registry requirement already cached: charset-normalizer==3.4.1
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: urllib3==2.3.0
DEBUG Registry requirement already cached: greenlet==3.1.1
DEBUG Registry requirement already cached: typing-extensions==4.12.2
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\py.typed
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\py.typed
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DAnimation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DExtras.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DInput.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DLogic.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DRender.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtAxContainer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtBluetooth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCharts.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtConcurrent.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDataVisualization.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDesigner.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphs.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphsWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGui.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHelp.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHttpServer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtLocation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimedia.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimediaWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetwork.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetworkAuth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNfc.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGL.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGLWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdf.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdfWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPositioning.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPrintSupport.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick3D.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickControls2.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtRemoteObjects.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtScxml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSensors.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialPort.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSpatialAudio.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSql.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtStateMachine.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvg.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvgWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTextToSpeech.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtUiTools.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebChannel.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebSockets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebView.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtXml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_config.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_git_pyside_version.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__feature__.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__init__.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DAnimation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DExtras.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DInput.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DLogic.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\Qt3DRender.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtAxContainer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtBluetooth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCharts.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtConcurrent.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDataVisualization.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtDesigner.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphs.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGraphsWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtGui.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHelp.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtHttpServer.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtLocation.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimedia.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtMultimediaWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetwork.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNetworkAuth.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtNfc.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGL.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtOpenGLWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdf.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPdfWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPositioning.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtPrintSupport.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuick3D.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickControls2.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtQuickWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtRemoteObjects.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtScxml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSensors.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialBus.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSerialPort.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSpatialAudio.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSql.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtStateMachine.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvg.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtSvgWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTest.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtTextToSpeech.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtUiTools.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebChannel.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineCore.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineQuick.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebEngineWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebSockets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWebView.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtWidgets.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\QtXml.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_config.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\_git_pyside_version.py
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__feature__.pyi
DEBUG File already exists (subsequent attempt), overwriting: C:\Users\rainzee\Dev\Workspaces\atlas\.venv\Lib\site-packages\PySide6\__init__.py
Installed 18 packages in 1.09s
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + colorama==0.4.6
 + dynamsoft-barcode-reader-bundle==10.5.2100
 + greenlet==3.1.1
 + idna==3.10
 + loguru==0.7.3
 + msgspec==0.19.0
 + pyside6==6.8.2.1
 + pyside6-addons==6.8.2.1
 + pyside6-essentials==6.8.2.1
 + pyzbar==0.1.9
 + requests==2.32.3
 + shiboken6==6.8.2.1
 + sqlalchemy==2.0.38
 + typing-extensions==4.12.2
 + urllib3==2.3.0
 + win32-setctime==1.2.0

atlas is 📦 v0.1.0 via 🐍 v3.12.9 (atlas) 
❯
```

---

_Comment by @konstin on 2025-02-28 13:20_

Can you try `uv cache clean`? Maybe there's some cache corruption, otherwise I'm having trouble figuring out the reason for this odd behavior.

---

_Closed by @rainzee on 2025-02-28 13:29_

---

_Comment by @konstin on 2025-02-28 13:32_

Did the `uv cache clean` work?

---

_Comment by @rainzee on 2025-02-28 14:06_

> Did the `uv cache clean` work?

it workds

---
