# Version:	PyInstaller 3.2

How to install Pyinstaller
==========================
installed commands
------------------
* pyinstaller 
* pyi-makespec - build spec file
* pyi-archive_viewer - inspect a bundled application
* pyi-bindepend - display dependencies of an executable
* pyi-grab_version

``` bash
ModuleNotFoundError: No module named 'pandas._libs.tslibs.np_datetime'
During handling of the above exception, another exception occurred:
Traceback (most recent call last):
...
If you want to import pandas from the source directory, you may need to run 'python setup.py build_ext --inplace --force' to build the C extensions first.
```

What PyInstaller Does and How It Does It
========================================
> PyInstaller reads a Python script written by you. It analyzes your code to discover every other module and library your script needs in order to execute. Then it collects copies of all those files – including the active Python interpreter! – and puts them with your script in a single folder, or optionally in a single executable file.

> 讀script並copy需要的files和interpreter

Analysis: Finding the Files Your Program Needs
----------------------------------------------
> PyInstaller understands the “egg” distribution format often used for Python packages. If your script imports a module from an “egg”, PyInstaller adds the egg and its dependencies to the set of needed files.
PyInstaller also knows about many major Python packages, including the GUI packages Qt (imported via PyQt or PySide), WxPython, TkInter, Django, and other major packages. For a complete list, see Supported Packages.

> pyinstaller 接受egg作為import來源

# 問題:
* 有些modules無法偵測到
* e.g. `__import__()`, `sys.path`, etc.

# 解法:
* 在command line 額外增加 檔案
* 在command line 增加額外 路徑
* 修改 .spec
* 寫 hook file 讓 pyinstaller 知道

> If your program depends on access to certain data files, you can tell PyInstaller to include them in the bundle as well. You do this by modifying the spec file, an advanced topic that is covered under **Using Spec Files**.

> In order to locate included files at run time, your program needs to be able to learn its path at run time in a way that works regardless of whether or not it is running from a bundle. This is covered under **Run-time Information**.

Bundling to One Folder
---------------------
pros:
* 容易debug
* 改code對整個資料夾異動不大

cons:
* 產生的檔案數量很多

How the One-Folder Program Works
--------------------------------
> A bundled program always starts execution in the PyInstaller **bootloader**. This is the heart of the myscript executable in the folder.

bootloader 造一個暫時性的 python 環境, 讓 interpreter 可以在資料夾裡找到想要的modules
> (This is an overview. For more detail, see **The Bootstrap Process in Detail below**.)

Bundling to One File
--------------------
pros:
* 檔案好找, 方便執行

cons:
* 其他檔案像是README需要另外放置
* 啟動速度比 one-folder bundle 還慢

How the One-File Program Works
------------------------------
> The bootloader is the heart of the one-file bundle also. When started it creates a temporary folder in the appropriate temp-folder location for this OS. The folder is named `_MEIxxxxxx`, where xxxxxx is a random number.

同樣依賴 `bootloader`, 不過是建立tmp folder 並解壓檔案到folder裡, 這也是為什麼 one-file app 速度會比 one-folder app 慢

> Do not give administrator privileges to a one-file executable (setuid root in Unix/Linux, or the “Run this program as an administrator” property in Windows 7). There is an unlikely but not impossible way in which a malicious attacker could corrupt one of the shared libraries in the temp folder while the bootloader is preparing it. Distribute a privileged program in one-folder mode instead.

> Applications that use os.setuid() may encounter permissions errors. The temporary folder where the bundled app runs may not being readable after setuid is called. If your script needs to call setuid, it may be better to use one-folder mode so as to have more control over the permissions on its files.

Using a Console Window
----------------------
> Do this when your script has a graphical interface for user input and can properly report its own diagnostics.

Hiding the Source Code
----------------------
bundled app 並沒有包含 source code.而是經過 compiled 過的 .pyc, 不過也是有可能會被 decompiled 去了解程式邏輯

解決方法: Cython
modules ----convert----> C ----compile---> machine language

Pyinstaller 可以 import Cython C object 然後打包他們

> PyInstaller can follow import statements that refer to Cython C object modules and bundle them.

Python bytecode 可以透過 AES256 加密 bytecode, 但還是容易被解回原來的bytecode...

Using PyInstaller
=================
``` bash
$ pyinstaller myscript.py
```

PyInstaller analyzes myscript.py and:
1. Writes myscript.spec in the same folder as the script.
2. Creates a folder `build` in the same folder as the script if it does not exist.
3. Writes some log files and working files in the `build` folder.
4. Creates a folder `dist` in the same folder as the script if it does not exist.
5. Writes the myscript executable folder in the `dist` folder.

> Normally you name one script on the command line. If you name more, all are analyzed and included in the output. However, the first script named supplies the name for the spec file and for the executable folder or file. Its code is the first to execute at run-time.

Pyinstall 可以允許包多個scripts, 然而執行時是執行第一個script的code
 
> For certain uses you may edit the contents of myscript.spec (described under Using Spec Files). After you do this, you name the spec file to PyInstaller instead of the script:
``` bash
$ pyinstaller myscript.spec
```

Options
-------
略, 直接去看文件

Running PyInstaller with Python optimizations
---------------------------------------------
* 還沒測試優化效果如何
```
# run with basic optimizations
python -O -m PyInstaller myscript.py

# also discard docstrings
python -OO -m PyInstaller myscript.py
```

Using UPX
---------
被UPX壓縮過後的執行順序：
1. The compressed program start up in the UPX decompressor code.
2. After decompression, the program executes the PyInstaller bootloader, which creates a temporary environment for Python.
4. The Python interpreter executes your script.

Encrypting Python Bytecode
--------------------------
* PyCrypto


> Please note that it is still very easy to extract the key and get back the original bytecode, but it should prevent most forms of “casual” tampering.

```Shortening the Command
--key = 'xxxxxxxxxxxxxxxx'
```

Defining the Extraction Location
--------------------------------
可以手動決定暫存檔位置, 避免執行過程中掛掉, 檔案遺留在 /tmp 浪費空間
```
 --runtime-tmpdir = .
```

Supporting Multiple Platforms
-----------------------------
目前開發沒有多平台或多版本的需求, 略

Making Linux Apps Forward-Compatible
------------------------------------
在Linux環境, bundled app 並不包含 `libc`, 而是**動態連結**執行環境時OS的 `libc`

問題:
* 所以可能在新版打包的 app, 在舊版的 Linux 無法執行

解決方法: 
* 在舊版的 Linux 打包 app, 比較可以確保在之後新版的 Linux 可以正常執行

Capturing Windows Version Data
------------------------------
目前工作沒這需求, 略

Building Mac OS X App Bundles
-----------------------------
目前工作沒這需求, 略

Run-time Information
====================
還不清楚應用在哪
> In order to locate included files at run time, your program needs to be able to learn its path at run time in a way that
works regardless of whether or not it is running from a bundle.

Using Spec Files
================
> The spec file tells PyInstaller how to process your script. It encodes the script names and most of the options you give to the pyinstaller command. The spec file is actually executable Python code. PyInstaller builds the app by executing the contents of the spec file.

There are four cases where it is useful to modify the spec file:
1. When you want to bundle data files with the app.
2. When you want to include run-time libraries (.dll or .so files) that PyInstaller does not know about from any other source.
3. When you want to add Python run-time options to the executable.
4. When you want to create a multiprogram bundle with merged common modules.

``` bash
# make spec file
$ pyi-makespec xxx.py

# build one-folder apps
$ pyinstaller xxx.spec

# build one-file apps
$ pyi-build xxx.spec
```

Notes about specific Features
=============================
Cython support
--------------
> PyInstaller can not determine if the Cython C object module is importing some Python module. These will typically show up as in a traceback like this (mind the .pyx extension):
``` bash
Traceback (most recent call last):
[…]
File "myapp\cython_module.pyx", line 3, in init myapp.cython_module
ModuleNotFoundError: No module named 'csv'
```
> So if you are using a Cython C object module, which imports Python modules, you will have to list these as `--hidden-import`.
