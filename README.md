# RosBE-Unix-2.2.2
RosBE for ReactOS upgraded to 2.2.2

## About

[RosBE](https://reactos.org/wiki/Build_Environment) aka ReactOS Build Environment aims at offering a build environment for [ReactOS](https://reactos.org/).
That way you could customize your own build livecd/boot iso.

This repository 

- is an personal fork of the original project 
- is not affiliated to ReactOS

This repository is an attempt to upgrade this environment by offering

- MinGW-W64 upgraded fron 6.0.0 to 6.0.1
- Gcc upgraded fron 8.4.0 to 8.5.0 with minor patches

Refer to `README_ReactOS` file for more instructions

## Instructions

This has been built inside a Ubuntu 18.04.060 chroot so it should be fine regarding your glibc version

Download and decompress

```
curl https://github.com/rboxeur/RosBE-Unix-2.2.2/releases/download/RosBE-Unix-2.2.2-linux/RosBE-Unix-2.2.2-linux.tar.xz --output RosBE-Unix-2.2.2-linux.tar.xz
sudo tar xf RosBE-Unix-2.2.2-linux.tar.x -C /
```

RosBE should be located into /opt/RosBE. 

Now you can download ReactOS sources and enter the RosBE environment.

```
git clone --recursive https://github.com/reactos/reactos.git
cd reactos
# Enter the RosBE environment
/opt/RosBE/RosBE.sh
# You can now perform your own commands
./configure.sh -DCMAKE_BUILD_TYPE=Release 
cd output-MinGW-i386
ninja bootcd
# To quit the RosBE environment just run exit
```

Once done then you have a hybrid boot iso to fill either on a USB key using dd command or a CD to burn.

## For those curious: details for patched Gcc are provided here

Gcc was patched refering to 

- URL https://github.com/gentoo/gcc-patches
- commit = 814ca13216462c81498ecbefb2a25fe54b27c1bc

- URL = https://github.com/Alexpux/MINGW-packages/tree/master/mingw-w64-gcc 
- commit = d20ad9115e4e5707ff7fc6ab8c4d9bedce26903a

You can clone this mingw-w64-gcc patched repository using following instructions

```
mkdir -pv MINGW-packages
cd MINGW-packages
git init
git remote add -f origin https://github.com/Alexpux/MINGW-packages.git
git config core.sparseCheckout true
echo "mingw-w64-gcc/*" >> .git/info/sparse-checkout
git pull origin master
git checkout d20ad9115e4e5707ff7fc6ab8c4d9bedce26903a
```

Since I am not on Arch then commands were applied manually refering to PKGBUILD = https://github.com/Alexpux/MINGW-packages/blob/master/mingw-w64-gcc/PKGBUILD
Only patches listed below were applied.


- 0002-Relocate-libintl.patch 
- 0003-Windows-Follow-Posix-dir-exists-semantics-more-close.patch 
- 0004-Windows-Use-not-in-progpath-and-leave-case-as-is.patch 
- 0005-Windows-Don-t-ignore-native-system-header-dir.patch 
- 0006-Windows-New-feature-to-allow-overriding.patch 
- 0007-Build-EXTRA_GNATTOOLS-for-Ada.patch 
- 0008-Prettify-linking-no-undefined.patch 
- 0009-gcc-make-xmmintrin-header-cplusplus-compatible-depre.patch 
- 0010-Fix-using-large-PCH.patch 
- 0011-Enable-shared-gnat-implib.patch 
- 0019-gcc-8-branch-Backport-patches-for-std-filesystem-from-master.patch
- 0140-gcc-8.2.0-diagnostic-color.patch
- pr88568.patch

Additional slightly modified patched https://raw.githubusercontent.com/mxe/mxe/refs/heads/master/plugins/gcc8/gcc8.patch
