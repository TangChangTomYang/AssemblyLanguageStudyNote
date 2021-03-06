**常用细节: **

- **函数调用栈**<br>
    在断点模式下,在lldb 中输入 bt 就可以打印当前的函数调用栈
    ![](/assets/Snip20180717_2.png)



<br>



Clutch
------------
*Clutch* is a high-speed iOS decryption tool. Clutch supports the iPhone, iPod Touch, and iPad as well as all iOS version, architecture types, and most binaries. **Clutch is meant only for educational purposes and security research.**

Usage
------------

**Important!**

Clutch may encounter `Segmentation Fault: 11` when dumping apps with a large number of frameworks.
Increase your device's maximum number of open file descriptors with `ulimit -n 512` (default is 256)

```sh
Clutch [OPTIONS]
-b --binary-dump Only dump binary files from specified bundleID
-d --dump Dump specified bundleID into .ipa file
-i --print-installed Print installed application
--clean Clean /var/tmp/clutch directory
--version Display version and exit
-? --help Display this help and exit
```
Getting started
------------
#### Have Xcode installed, install the additional command line tools
```sh
xcode-select install
```
#### Clone the repo
```sh
git clone git@github.com:KJCracks/Clutch.git
cd Clutch
```
#### Build
```sh
xcodebuild -project Clutch.xcodeproj -configuration Release ARCHS="armv7 armv7s arm64" build
```
#### Install

```sh
scp /path/to/Clutch root@<your.device.ip>:/usr/bin/
```
```sh
ssh root@<your.device.ip>
```
```sh
Clutch
```
_Note: you may need to `chmod +x Clutch`_
Licenses
------------
*Clutch* uses the following libraries under their respective licenses.

[optool] (https://github.com/alexzielenski/optool) by Alex Zielenski<br />
[GBCli] (https://github.com/tomaz/GBCli) by Tomaz Kragelj<br />
[ZipArchive] (https://github.com/mattconnolly/ZipArchive/) by Matt Connolly, Edward Patel, et al.<br />
[MiniZip] (http://www.winimage.com/zLibDll/minizip.html) by Gilles Vollant and Mathias Svensson.

Thanks
------------
*Clutch* woudn't be what it is without these people:

dissident - The original creator of *Clutch* (pre 1.2.6)
Nighthawk - Code contributor (pre 1.2.6)
Rastignac - Inspiration and genius
TheSexyPenguin - Inspiration
Contributors:
[iT0ny](https://github.com/iT0ny)
[ttwj](https://github.com/ttwj)
[NinjaLikesCheez](https://github.com/NinjaLikesCheez)
[Tatsh](https://github.com/Tatsh)
[C0deH4cker](https://github.com/C0deH4cker)
[DoubleDoughnut](https://github.com/DoubleDoughnut)
[iD70my](https://github.com/iD70my)
[OdNairy](https://github.com/OdNairy)

(c) [Kim Jong-Cracks](http://cracksby.kim) 1819-2016
