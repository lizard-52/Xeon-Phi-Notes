# Can I get FFMPEG Running on a Xeon Phi?
TLDR; Not the latest version at least. x264 does kinda work though

First step was following <a href= https://aidancrowther.com/project/xeonphi> this guide </a> to get a cross compilation toolchain working. I originally had a Ubuntu 14.11 VM working with VirtualBox, but a Windows update(?) broke the UI. Issue persisted across multiple computers with the VMs being rebuilt from scratch each time. I gave up and moved to VMware. So far the only issue is <a href=https://docs.vmware.com/en/VMware-Tools/12.1.0/com.vmware.vsphere.vmwaretools.doc/GUID-C48E1F14-240D-4DD1-8D4C-25B6EBE4BB0F.html> installing open vm tools </a> is a bit of a pain since you need to fix all the targets for the package manager first. 

Once the VM was up and running I tried to follow <a href=https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu> this guide </a> to cross-compiling ffmpeg. Surprisingly some of the dependencies come with makefiles that work properly with k1om!

First step is to follow the ffmpeg guide until you get to the NASM heading.

## NASM
This is not actually needed for compiling to k1om, or anything else really. Some libraries use it to optimize performance sensitive code for x86.

That being said you can totally still compile NASM for k1om. I followed the directions exactly, except for using the following instead of the usual ./configure command and exporting the following environment variables.

```
export CC=k1om-mpss-linux-gcc
export CXX=k1om-mpss-linux-g++
PATH="$HOME/bin:$PATH" CC=k1om-mpss-linux-gcc ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --host=k1om
```

"--host=k1om" tells the build script that we're compiling for k1om. This option only sometimes works on ./configure scripts as they are not standardized.

If everything runs properly you should get `nasm` and `ndiasm` executables in the ~/bin/ folder created during the initial setup. Hopefully they fail to run on the host machine (expect a invalid binary format or similar error). If they do run something is wrong with the compiler setup and the makefile is using the standard gcc install.

Next step is to copy the files over to the phi. I use WinSCP to copy the executable over then SSH in to run it, but there are a bunch of ways to run things on Xeon Phi.

