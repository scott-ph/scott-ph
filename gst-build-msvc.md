---
title: gst-build with MSVC
---

1. install [visual c++ build tools](https://www.visualstudio.com/downloads/#microsoft-visual-c-build-tools-2015-update-3)
* install [python](https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe)
 * select adding to path
* install [git](https://github.com/git-for-windows/git/releases/download/v2.10.2.windows.1/Git-2.10.2-64-bit.exe)
 * select adding to path
 * checkout-as-is, checkin-as-is
* download & extract [ninja-win](https://github.com/ninja-build/ninja/releases/download/v1.7.2/ninja-win.zip)
* install [msys2](http://repo.msys2.org/distrib/x86_64/msys2-x86_64-20161025.exe)
* Press \[win\]+X, "Command Prompt (Admin)"

  ```dos
  cd %LOCALAPPDATA%\Programs\Python\Python35
  mklink python3.exe python.exe
  ```
* click Start >> MSYS2 MinGW 64-bit

  ```bash
  pacman -S mingw-w64-x86_64-json-glib
  pacman -S mingw-w64-x86_64-pkg-config
  pacman -S mingw-w64-x86_64-libxml2
  pacman -S bison
  pacman -S perl
  vim /mingw64/include/zconf.h # change Z_HAVE_UNISTD_H to undefined
  ```
* click Start >> VS2015 x64 Native Tools Command Prompt

  ```powershell
  powershell
  Set-PSReadlineOption -EditMode Emacs
  # ^ required by brain
  cd $env:HOMEPATH\Downloads
  git clone https://github.com/scott-d-phillips/mingw_make_libs
  python3 mingw_make_libs\make_libs.py
  git clone git://anongit.freedesktop.org/gstreamer/gst-build
  cd gst-build
  git clone https://github.com/mesonbuild/meson
  $env:PATH += ';C:\msys64\mingw64\bin;C:\msys64\usr\bin;C:\Program Files (x86)\Windows Kits\10\bin\x64\ucrt`
  $env:PATH += ";$env:HOMEDRIVE$env:HOMEPATH\Downloads\ninja-win"
  py setup
  ninja -C build
  rm -recurse -force -ea silentlycontinue build\dll; mkdir build\dll; ls build -filter *.dll -recurse | %{join-path -path $_.directory -childpath $_.name} | cp -destination build\dll
  .\gst-uninstalled.py
  ```
* In the new window

  ```dos
  PATH=C:\msys64\mingw64\bin;C:\mingw64\usr\bin;%PATH%;%HOMEDRIVE%%HOMEPATH%\gst-build\build\dll
  gst-inspect-1.0
  ```
