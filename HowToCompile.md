
How to compile
AlessandroZ edited this page on Mar 4, 2019 · 9 revisions
Using pyinstaller

    Download pyinstaller or pip install pyinstaller
    Create the executable file:pyinstaller --onefile -w lazagne.py

For Windows only

    Install PyWin32.

    To get a better compatibility between systems, msvcp100.dll and msvcr100.dll could be added. These files could be found in "C:\Windows\System32". Place them in the Windows folder of the project.

    Create a .spec file adding all options wanted. Mine is as follow:

# -*- mode: python -*-
import sys
a = Analysis(
        ['laZagne.py'],
        pathex=[''],
        hiddenimports=[],
        hookspath=None,
        runtime_hooks=None
)

for d in a.datas:
  if 'pyconfig' in d[0]:
        a.datas.remove(d)
        break

pyz = PYZ(a.pure)
exe = EXE(
        pyz,
        a.scripts,
        a.binaries + [('msvcp100.dll', 'C:\\Windows\\System32\\msvcp100.dll', 'BINARY'),
                      ('msvcr100.dll', 'C:\\Windows\\System32\\msvcr100.dll', 'BINARY')]
        if sys.platform == 'win32' else a.binaries,
        a.zipfiles,
        a.datas,
        name='lazagne.exe',
        debug=False,
        strip=None,
        upx=True,
        console=True
)

    Generate your executable file: pyinstaller --onefile -w lazagne.spec
    For cross compilation, check how I did it using travis.
