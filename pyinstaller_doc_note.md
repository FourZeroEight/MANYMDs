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
* 有些modules無法偵測到, e.g. `__import__()`, `sys.path`
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
> bootloader 造一個暫時性的 python 環境, 讓 interpreter 可以在資料夾裡找到想要的modules
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





